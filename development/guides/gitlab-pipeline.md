---
title: Gitlab Pipeline
description: How to use gitlab to distribute your module.
published: true
date: 2025-07-05T17:58:01.969Z
tags: module, git, gitlab, pipeline
editor: markdown
dateCreated: 2024-12-05T01:42:27.416Z
---

From the desk of: G Pa Dax (foundryserver.com)

> Howdy!
> I currently use Gitlab instead of Github as the home of my module repo. I decided to create a pipeline to package and release my module. It took me a few hours to work my way through to a functioning pipeline. I thought I would share what I learned and hopefully save some time for the next person. I am sure there are multiple ways to do this, this is just the one I was able to make work for me.
> {.is-info}

The idea of creating a release is to package up all necessary files and create a installable zip file for use by the Foundry VTT application. There is no real installer other than the module.json file. When foundry installs a module, it simply downloads and reads the module.json file form your git repository. This tells Foundry all about your module and where to find all the assets it requires. It needs to know the version of the module and it needs to know the download url for the zip file. There is other information in module.json, but we will just focus on these two items as it pertains to the pipeline. I "think" in Github a pipeline is called an action.

## File Structure of the repo

How you structure your files will impact how the pipeline functions. In order for you to understand this pipeline you have to understand the file system in use.

```
my-module --| module.json
            | /.git
            | /src---|
                     | /packs
                     | /scripts
                     | /styles
                     | /templates
```

You will have other files and directories in your project. These would be files required for development. Things like .gitignore, .vscode, package.json, readme.md node_modules etc. I chose this layout so that it would be easy for me to zip up only the files necessary to make the module work. Other than module.json, which has to live in the root dir, all other required files live in the /src directory. The pipeline file .gitlab-ci.yml has to be in the root directory of your project as well. This can be changed but why complicate things more?

## module.json

This file needs to reside in the root directory of the repo. Lets review the relevant sections of the file as it pertains to the pipeline.

```
{
  "version": "1.0.0",
  "manifest": "https://Gitlab.com/my_account/my-module/-/raw/main/module.json",
  "download": "https://Gitlab.com/my_account/my-module/-/releases/v1.0.0/downloads/my-module-v1.0.0.zip",
}
```

The version element is key to the whole pipeline. This will form the url for the zip file, the release name, and the tag name(id). This is the same version number you see in the module list in foundry vtt. This is also how Foundry will know that there is a new version available for download.

The manifest value is the url to the module.json file in the root of your project(repo). It is made up of the Gitlab account (my_account), the branch (main) and the name of the file (module.json)

The download url is a bit different. It points to your release NOT the repo. This is what messed me up the most. When you create a release it uploads the zip file to a special location. The url is Gitlab account (my_account), the release name (v1.0.0), and then the file name of the zip file. (my-module-v1.0.0.zip).

## Gitlab Authorization

This pipeline will need to use a project token. This token is created in the settings menu in the project. You will need to scope it to developer, and api. It is up to you to have a expiry date if you want. You will then need to go to the ci/cd settings, under variables and make a new variable with the token you just created. Name the variable CI_JOB_TOKEN (key) and the value is the token you copied from the previous step.

## The Pipeline Code

There is a lot going on in this file. I think the best thing is to show a section of the file and then describe what I am doing and why. You could try to just c/p this file and it might work. I would give it a shot. If it doesn't then this should help you trouble shoot the issue. So with out further adieu here is the pipeline file. An opinionated example can be found [here](https://gitlab.com/boyobejamin/foundryvtt-module-release-pipeline/-/raw/main/.gitlab-ci.yaml?ref_type=heads)

```
# Set global variables
variables:
  CI_DEBUG_TRACE: "false"  # Only enable when debugging
  GLAB_DEBUG: "false"      # Only enable when debugging

# Identify the stages of the pipeline
stages:
  - release

image:
  name: "gitlab/glab"
  entrypoint: [""]
```

The first variable can be set to enable verbose logging. This will help if you are having issues. We will only have one stage, I choose to call it release. You can call it whatever but you have to be consistent. The image is a a linux distribution that will will use to run our applications like zip and git.

```
# Define the release stage
Release New Version:
  stage: release
  only:
    - tags
```

Here we title the stage. Then indicate that this is the release stage. This has to match what you stated in the previous section. The only tags, tells Gitlab to only run this pipeline if the repo has been tagged.(If you don't know what tagging is, please go read the Gitlab docs.)

```
before_script: |
    apk add --no-cache zip git
```

Here we get into the meat of the pipeline. We are running all of these commands on a linux computer via the bash shell. The Gitlab pipeline has three stages of scripts. Before, current, and after. The before_script is to prep the environment for the packaging and release. We use an image that contains the `glab` cli tool to interact with the GitLab API to create a release. We also need to install the zip program and the git program.

```
    # Setup git configuration
    git config --global user.email "ci@Gitlab.com"
    git config --global user.name "GitLab CI"
```

Setup the configuration for the local version of git on this linux computer.

```
    # Update version in module.json with tag value
    TAG_VERSION=${CI_COMMIT_TAG#v} # Remove the 'v' from the tag
```

When we tag the repo, it will create a global variable that we can access inside of this linux computer. The global variable is CI_COMMIT_TAG. We then take that value and remove the "v" from the tag name if it exists and create a new global variable called TAG_VERSION. These are pipeline variables.

```
    # using sed, replace any text that is in the format of #.#.# with the tag version
    ESCAPED_TAG_VERSION=$(echo "$TAG_VERSION" | sed 's/[&/\]/\\&/g')
    sed -i -e "s/\"version\": \"[0-9]*\.[0-9]*\.[0-9]*\"/\"version\": \"$ESCAPED_TAG_VERSION\"/" module.json
    sed -i -e "s/v[0-9]*\.[0-9]*\.[0-9]*/v$ESCAPED_TAG_VERSION/g" module.json
```

This section looks complicated. To be honest I had to get chatgpt help me write it. We are taking the tag name v1.2.3 and replacing all occurrences of the pervious version with the new one inside of the file module.json. By changing this value, we now tell Foundry that there is a new version available.

```
    # Commit and push changes
    git add module.json
    git commit -m "Update version to $CI_COMMIT_TAG in module.json"
    git push "https://oauth2:${CI_JOB_TOKEN}@${CI_SERVER_HOST}/${CI_PROJECT_PATH}.git" HEAD:main
```

This part takes the new module.json and commits it back into the repo. We need to do this so that we always know what the current release version of our code it. We also need the module.json to be available to Foundryvtt at any time someone looks for an update with the current version.

```
  script: |

    # Generate release file name.
    echo "[Info] Preparing Release ${CI_PROJECT_NAME} at ${CI_COMMIT_TAG}"
    export RELEASE_FILE="${CI_PROJECT_DIR}/${CI_PROJECT_NAME}-${CI_COMMIT_TAG}.zip"
    export RELEASE_NAME="v${CI_COMMIT_TAG}"
    export MODULE_DIRECTORY="${CI_PROJECT_DIR}/src"
```

Here we are taking global Gitlab variables and creating linux shell environment variables. We do this as the applications will need this information to work.

```
    # copy module.json to src
    echo "[Info] Bundling module... this may take some time"
    cp -v "${CI_PROJECT_DIR}/module.json" "${MODULE_DIRECTORY}/module.json"

    # Package module
    cd "${MODULE_DIRECTORY}"
    zip -qr "${RELEASE_FILE}" .
    cd ${CI_PROJECT_DIR}
```

We need to make a second copy of the new module.json file in the /src dir. When Foundry instals this module it needs this information to make it run. However you will notice we don't commit this to the repo. We only need it for the release. Then we create the zip file for the release.

```
    # Authenticate with GitLab by setting environment variable
    export GITLAB_TOKEN=$CI_JOB_TOKEN
```

Here is where we get the token from the variable we set earlier on in the process.

```
# Get commit messages since last tag for changelog
    if [[ $(git describe --tags | wc -l) -gt 1 ]]; then
      echo "[Info] Generating changelog from ${LAST_TAG} to ${CI_COMMIT_TAG}"
      # Get commit messages since last tag for changelog

      LAST_TAG="$(git describe --tags --abbrev=0 HEAD^)"
      CHANGELOG="$(git log ${LAST_TAG}..HEAD --pretty=format:"* %s" --no-merges)"

      # Format release notes with proper newlines
      RELEASE_NOTES="$(echo -e "## Changelog (since ${LAST_TAG}):\n${CHANGELOG}")"
    else
      echo "[Info] Congratulations on your first release!!!"
      # If this is the first tag, keep the changelog short and sweet
      CHANGELOG="$(git log  --pretty=format:"* %s"  --no-merges)"

      # Format release notes with proper newlines
      RELEASE_NOTES="$(echo -e "## Changelog:\n${CHANGELOG}")"
    fi
```

This part is a nice to have. This will take all of the commit messages from the last tag and add them to the change log on the release page. Help tell the users what is in the new release.

```
   echo "[Info] Authenticating to ${CI_SERVER_HOST}"
   glab auth login \
     --hostname "${CI_SERVER_HOST}" \
     --job-token "${CI_JOB_TOKEN}"
```

Authenticate to GitLab

```
    # Create release using formatted notes
    glab release create "${RELEASE_NAME}" \
      "${RELEASE_FILE}" \
      --notes "${RELEASE_NOTES}"
```

The final part is to actually create the release. This will take the zipfile upload it to a special place so it is available for download by users.

## Trigger a Release

How do we make this all happen. During the normal course of development you will make many commits. This pipeline will not run each time you make a commit. That would be crazy and you would end up with so many releases that it would become too much. The trigger to the pipeline will be the process of adding a Tag to the repo. A tag, marks a moment in time and freezes the code at that point. Don't worry you can keep making changes. This makes it really easy to role back your work to a known working version. Here is how you make a tag and make the magic happen. From your command prompt in the root directory of your project.

```
git add .
git commit -m "Fix the bug"
git push
```

Make sure you have committed all of your changes.

```
git tag -a v1.2.3 -m "Version 1.2.3 release"
git push origin v1.2.3
```

You should now be able to login to Gitlab and look at your pipelines and see it in progress. When it is complete you can the look in Deploy->Releases and see your new release.

## Final Thoughts

I am sure there will be questions about this guide and the instructions. I will update this guide as I get feedback from all of you. You can find me on the Foundry Discord as "Dax" and also on my own Discord https://discord.gg/foundryserver as G Pa Dax. I hope this saves you a bunch of time. Remember there are more ways than one to do this. This is just my way.
