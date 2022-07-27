---
title: Docker
description: 
published: true
date: 2022-05-19T13:18:59.505Z
tags: 
editor: markdown
dateCreated: 2020-09-23T00:34:32.550Z
---

>Docker is not an officially supported installation method. If you need support while using a docker container you will need to contact the container author.
{.is-warning}

You can use Docker to run Foundry VTT. You may use several different approaches.

Here is a table of the approaches detailed within as well as notes on their complexity and extra features.

| Author                                     | Difficulty | Extra Features     | Notes                        |
| ------------------------------------------ | ---------- | ------------------ | ---------------------------- |
| [mikysan](#mikysans-simple-dockerfile)     | simple     |                    | Simple Dockerfile            |
| [Jake](#jake)        | simple     |                    | Updated version of mikysan's |
| [Felddy](#felddys-easy-one-step-docker-container)      | moderate     |                    | Set and Forget, Configurable           |
| [trotroyanas](#trotroyanass-docker-compose-setup) | moderate   |                    |                              |
| [thomasfa18](#mikysans-simple-dockerfile)  | moderate   |                    |                              |
| [DireckHit](#direckthits-guide-to-running-fvtt-docker-with-traefik-and-portainer)   | complex    | Traefik, Portainer | Good for Remote Hosting      |
| [Vicknesh](#vickneshs-docker-deployment-guide)    | complex    | Caddy for TLS      |                              |
| [MBRound18's](#mbround18-foundryvtt-docker) | simple | auto restarts on issues | Simple drop in link and ready to go |
---

# mikysan's simple dockerfile

Dockerfile and guides for manual and docker-compose setup available at [https://github.com/mikysan/simple-fvtt-dockerfile](https://github.com/mikysan/simple-fvtt-dockerfile).

---

# [Felddy's Easy, One-Step, Docker Container](https://github.com/felddy/foundryvtt-docker#readme)

<div align="center">
<img width="230" src="https://raw.githubusercontent.com/felddy/foundryvtt-docker/develop/assets/logo.png">
</div>

[![Docker Pulls](https://img.shields.io/docker/pulls/felddy/foundryvtt)](https://hub.docker.com/r/felddy/foundryvtt)
[![Docker Image Size (latest by date)](https://img.shields.io/docker/image-size/felddy/foundryvtt)](https://hub.docker.com/r/felddy/foundryvtt)
![Platforms](https://img.shields.io/badge/platforms-amd64%20%7C%20arm%2Fv6%20%7C%20arm%2Fv7%20%7C%20arm64%20%7C%20ppc64le%20%7C%20s390x-blue)

You can get a [Foundry Virtual Tabletop](https://foundryvtt.com) instance up and
running in minutes using this container.  This Docker container is designed to
be secure, reliable, compact, and simple to use.  It only requires that you
provide the credentials or URL needed to download a Foundry Virtual Tabletop
release.

### Using Docker with credentials ###

You can use the following command to start up a Foundry Virtual Tabletop server.
Your [foundryvtt.com](https://foundryvtt.com) credentials are required so the
container can install and license your server.

```console
docker run \
  --env FOUNDRY_USERNAME='<your_username>' \
  --env FOUNDRY_PASSWORD='<your_password>' \
  --publish 30000:30000/tcp \
  --volume <your_data_dir>:/data \
  felddy/foundryvtt:release
```

If you are using `bash`, or a similar shell, consider pre-pending the Docker
command with a space to prevent your credentials from being committed to the
shell history list.  See:
[`HISTCONTROL`](https://www.gnu.org/software/bash/manual/html_node/Bash-Variables.html#index-HISTCONTROL)

### Using Docker with a temporary URL ###

Alternatively, you may acquire a temporary download token from your user profile
page on the Foundry website.  On the "Purchased Licenses" page, click the [🔗]
icon to the right of the standard `Node.js` download link to obtain a temporary
download URL for the software.

```console
docker run \
  --env FOUNDRY_RELEASE_URL='<temporary_url>' \
  --publish 30000:30000/tcp \
  --volume <your_data_dir>:/data \
  felddy/foundryvtt:release
```

For more information about the available configuration options please see the [project README](https://github.com/felddy/foundryvtt-docker#readme).  If you have any questions please feel free to contact me on the FoundryVTT discord: `@felddy`

### Installation Guide for a Synology NAS ###

We wrote a [full guide for setting up this Docker image on a Synology NAS](/setup/hosting/Synology). The guide also covers HTTPS configuration.


---


# DirecktHit's Guide to Running FVTT-Docker with Traefik and Portainer

Please visit [DirecktHit's blog](https://benprice.dev/posts/fvtt-docker-tutorial/) for the most up to date directions.

## DirecktHit's Docker Hub Image via `docker-compose`

Please visit the README in the [fvtt-docker repository](https://github.com/BenjaminPrice/fvtt-docker) for the most up to date directions.

---

# trotroyanas's docker-compose Setup

### For this you only need 3 files.
      * Dockerfile
      * docker-compose.yml
      * foundryvtt-x.x.x.zip (Available on [https://foundryvtt.com](foundryvtt.com))

**Dockerfile** describe installation of your container

```yaml
FROM alpine:3.13

# Set the foundry install home
RUN adduser -D foundry
RUN mkdir -p /home/foundry/fvtt
RUN mkdir -p /home/foundry/fvttdata

ENV FOUNDRY_HOME=/home/foundry/fvtt
ENV FOUNDRY_DATA=/home/foundry/fvttdata

RUN apk add --update nodejs=14.16.1-r1

# Set the current working directory
WORKDIR "${FOUNDRY_HOME}"

#copy found
COPY ./foundryvtt-0.8.5.zip .

#unzip
RUN unzip foundryvtt*.zip
RUN rm foundryvtt*.zip

EXPOSE 30000
CMD node ${FOUNDRY_HOME}/resources/app/main.js --dataPath=${FOUNDRY_DATA}
```

###  **docker-compose.yml** describe creation image
```yaml
version: '3.3'

services:
   fvtt:
        container_name: Fvtt
        build:
            context: ./
            dockerfile: ./Dockerfile
        image: fvtt:3.0
        volumes:
          - /data/your/folder/app:/home/foundry/fvtt
          - /data/your/folder/data:/home/foundry/fvttdata
        ports:
            - "30000:30000"
```

#### **foundryvtt-x.8.5.zip** FoundryVTT (Available on [https://foundryvtt.com](foundryvtt.com))
copy your foundryvtt-x.8.5.zip file to the same level as the other files


### *Build image*
`sudo docker-compose build`

After build image you can use or test your container with this commands :

Run the container (for test no save modifications)
#### `sudo docker run --restart=unless-stopped --name Fvtt -p 30000:30000 -d fvtt:3.0`


#### Run the docker with volume map for save yours modifications : ####
```
sudo docker run \
--restart=unless-stopped \
--name Fvtt \
-p 30000:30000 \
-v /data/your/folder/app:/home/foundry/fvttd \
-v /data/your/folder/data:/home/foundry/fvttdata \
-d fvtt:3.0
```

---

# Jake
updated version of mikysan's dockerfile.  
Using the node version zip. Extract the contents into foundryvtt directory below. This dockerfile will copy your modules, worlds, assets, etc. into your docker image. 

I created a directory structure as:

```
* FoundryVtt
    * foundryvtt
    * foundrydata
        * Config
        * Data
        * Logs
    * Dockerfile
```

### Contents of Docker File
```
FROM node:12-alpine

ENV UID=1000
ENV GUID=1000

RUN deluser node
RUN adduser -u $UID -D foundry

USER foundry
RUN mkdir -p /home/foundry/data
RUN mkdir -p /home/foundry/app

WORKDIR /home/foundry/data
COPY --chown=$UID /foundrydata/ .

WORKDIR /home/foundry/app
COPY /foundryvtt/ .

EXPOSE 80:30000
CMD ["node", "/home/foundry/app/resources/app/main.js", "--headless", "--dataPath=/home/foundry/data" ]
```
### Docker commands
```
docker build . -t {something/something}
```

after the build is finished

```
docker run --rm -it  -p 80:30000/tcp {something/something}:latest
```

---

# thomasfa18's dockerhub image
 
This is a prebuilt node image that expects to have two paths (pkg & data) mounted to it:
/pkg - which is the contents of the node.js foundryvtt.zip from foundryvtt.com
/data - this is/will be your userdata folder. (If you are migrating from an existing install you can copy your existing data or target that folder)

The benefit of this image is that your app and data is decoupled from the node.js instance so you can place them on a shared folder if you like, and because ts a dockerhub image is can be easily pulled and run by many standalone home NAS devices (synology and qnap for instance).

Firstly configure you app and data directories (if you are using a NAS, create these folder on your NAS)
1. Create a new folder in the file system called `Docker`
2. Under this folder create a new folder called `foundry`
3. Under the foundry folder create a new folder called `pkg` and another folder called `data`
3. Copy the contents of the node.js foundryvtt.zip to the `pkg` folder
4. If you have existing userdata, copy this to the `data` folder

To download and run the container on a synology NAS
1. Install Docker from the package center
2. From docker goto registry and search for thomasfa18
3. Download the thomasfa18/node-foundry image
4. Go to Image, click on thomasfa18/node-foundry:latest, click launch
5. Click advance settings
6. On the first tab tick the checkbox for 'Enable auto-restart'
7. On the volume tab, add folders and mount paths that contain your data. eg. `docker/foundry/data` as mount path `/data` and `docker/foundry/pkg` as `/pkg`
8. On Port Setting tab set the local port to the port you want to use on your NAS to access the foundry website eg 30000, and the container port to the default 30000 (if you are migrating use the port that corresponds to your existing configuration)
9. Click next and then click apply
10. Click on the container menu and confirm the docker container is now running (if you right click and go to detail you can also review the log file for the container). You can now access the foundry server from any browser on you home network, and the internet if you configure your port forwarding.

It is a similar process to achieve the same using a computer, assuming you have already installed docker
1. `docker pull thomasfa18/node-foundry:latest`
2. `docker run -v [your windows path to foundry data]:/data -v [your windows path to the extracted node.js foundry package]:/pkg -it -p 30000:30000 thomasfa18/node-foundry:latest`
*Note:* using `-it` runs the container interactively, if you close the command window you will shut down the container. If you omit the `-it` form the command you will need to find the container name using `docker stats` or something to be able to shut it down via `docker kill [container name]`

---

# Vicknesh's Docker Deployment guide

Guide for setting up FoundryVTT with Caddy for TLS can be found [https://github.com/svicknesh/foundryvtt-docker-deploy](https://github.com/svicknesh/foundryvtt-docker-deploy)

---

<h1 id="mbround18-foundryvtt-docker">
  <a target="_blank" href="https://github.com/mbround18/foundryvtt-docker">MBRound18's FoundryVTT Docker</a>
</h1>

[YouTube Tutorial](https://www.youtube.com/watch?v=5L8Y2-JoGss)

1. Launch Via Docker
```sh
docker run --rm -it \
  -p 4444:4444 \
  -e HOSTNAME="127.0.0.1" \
  -e SSL_PROXY="false" \
  -v ${PWD}/foundry/data:/foundrydata \
  -v ${PWD}/foundry/app:/foundryvtt \
  mbround18/foundryvtt-docker:latest
```

2. Navigate to [127.0.0.1:4444](http://1237.0.0.1:4444)
3. Follow directions on screen. 
