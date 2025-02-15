---
title: Docker
description: 
published: true
date: 2025-02-15T19:44:42.263Z
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
| [BTBTravis'](#btbtravis-foundryvtt-docker) | moderate | fly.io deploy | Deploy to fly.io via Dockerfile |
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

Note:  Latest docker images do not work! However, instructions/labels are useful if you're trying to get it working with Traefik.
~~Please visit [DirecktHit's blog](https://benprice.dev/posts/fvtt-docker-tutorial/) for the most up to date directions.~~

## DirecktHit's Docker Hub Image via `docker-compose`

Note: Latest docker images do not work!
~~Please visit the README in the [fvtt-docker repository](https://github.com/BenjaminPrice/fvtt-docker) for the most up to date directions.~~

---

# trotroyanas's docker-compose Setup

### For this you only need 3 files.
      * Dockerfile
      * docker-compose.yml
      * foundryvtt-x.x.x.zip (Available on [https://foundryvtt.com](foundryvtt.com))

**Dockerfile** describe installation of your container

```yaml
# Based on availability of versions, you might need to update the alpine version and the nodejs version - see version available here https://hub.docker.com/_/node/
FROM alpine:3.20

# Set the foundry install home
RUN adduser -D foundry
RUN mkdir -p /home/foundry/fvtt
RUN mkdir -p /home/foundry/fvttdata

ENV FOUNDRY_HOME=/home/foundry/fvtt
ENV FOUNDRY_DATA=/home/foundry/fvttdata

RUN apk add --update nodejs=20.15.1-r0  #if you get an error in nodejs update, search for newer alpine version and update the nodejs version based on the error message from terminal

# Set the current working directory
WORKDIR "${FOUNDRY_HOME}"

#copy found
COPY ./FoundryVTT-XX.YYY.zip .  #insert exact name of your downloaded FoundryVTT zip file - case sensitive

#unzip
RUN unzip FoundryVTT-XX.YYY.zip
RUN rm FoundryVTT-XX.YYY.zip

EXPOSE 30000
CMD node ${FOUNDRY_HOME}/resources/app/main.js --dataPath=${FOUNDRY_DATA}
```

###  **docker-compose.yml** describe creation image
```yaml

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
--init
--restart unless-stopped \
--name Fvtt \
--publish 30000:30000 \
--volume /data/your/folder/app:/home/foundry/fvttd \
--volume /data/your/folder/data:/home/foundry/fvttdata \
fvtt:3.0
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

<h1 id="btbtravis-foundryvtt-docker">BTBTravis' Fly.io Setup</h1>

Please visit the README in the [travisshears/foundry-vtt repository](
https://git.sr.ht/~travisshears/foundry-vtt) for the most up to date instructions.

---

# MarekXcz's docker-compose with Cloudflare on Windows setup
> Disclaimer: I am not Docker expert nor Cloudflare expert. I came up with the solution below for my own needs and thought to share. If you think it can be optimized, feel free to update. 
{.is-info}


This guide is a modified version of trotroyanas's docker-compose Setup suited to be run on Docker Desktop on Windows and using a Cloudflare tunnel (also run in Docker) to expose your server to the internet (e.g. get past your NAT router).

With this setup, you will be able to run FOundryVTT on your home PC/server on Windows even if your internet provider does not give you public IP. You can also run multiple instances of FoundryVTT, each in it's own container and with it's own CLoudflare tunnel. 

The cloudflare tunnel bypasses your NAT router and allows you to assign a domain name to your FoundryVTT instance.

## Configuring your server
Make sure your host server has a static internal IP (you might need to assign it one in your router DHCP settings - google how to do this for your specific router).
You of course need a running installation of Docker (e.g. Docker Desktop for Windows)

## Configuring Docker
Cloudflare and FoundryVTT will be running in two separate containers. In order for them to be able to communicate, they need to run on the same docker network (unless you want to connect them to your host server network).

First, create the network by running this command in Terminal or Command Prompt:
```
docker network create my-net-name
```  
where instead of "my-net-name" put your preferred network name (important for Docker setup purposes only). Importantly, you need to create this network first before you do the other steps in this guide.

## Setting up Cloudflare tunnel in Docker
Set up your Cloudflare account and follow [the Step 1 in this Cloudflare guide](/en/setup/hosting/cloudflare-proxy-tunnel) to set up your domain name you want to use to access your FoundryVTT. 

Once your domain is set, [follow this guide](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/get-started/create-remote-tunnel/) to set up a Zero-Trust Cloudflare tunnel. In step 6 of this guide, choose Docker as an operating system and copy the docker run command shown there (containing your secret tunnel token). it will look like this: 

> docker run cloudflare/cloudflared:latest tunnel --no-autoupdate run --token YOUR_SECRET_TUNNEL_TOKEN

Modify this command to insert the name of the docker network you created earlier (instead of "my-net-name" insert the name of your network and isntead of YOUR_SECRET_TUNNEL_TOKEN use the actual token from the Cloudflare site):
```
docker run --network=my-net-name cloudflare/cloudflared:latest tunnel --no-autoupdate run --token YOUR_SECRET_TUNNEL_TOKEN
```
Now run this in Terminal or Command prompt of your server. If you did all correctly, you should now see a running container with cloudflare in your Docker.

Go back to Zero-Trust, where you created the tunnel, and switch to the `Public Hostname` tab. 
- Under Subdomain, write down what subdomain you want to use (i.e. you can use different subdomain for each of your FoundryVTT instance in docker but keep using the same domain name)
- Under Domain choose the domain name you set up in Step 1.
- Leave Path empty.
- Under Type select HTTP.
- Under URL enter the internal IP address of your host server and the port FoundryVTT is using (e.g. X.Y.Z.Q:30000).
- Click on Save hostname. 

The tunnel should now be routing your subdomain.domain.xyz request from the internet to your host server.

## Setting up FoundryVTT in Docker
First, create a folder for the FoundryVTT in your preferred location and in it create two subfolders. One for the app data (e.g. foundry-container) and one for your game data and other assets (e.g. resources). Let's say, for the purpose of this example, the compelte path looks like this:
```
D:\FoundryVTT\countainer_files
D:\FoundryVTT\resources
```

you can change the path, but then you need to update the paths in the code and command examples below as well.

### Create the needed files
Similar to trotroyanas's guide above, you will need to consolidate the docker files in the app folder (in our case D:\FoundryVTT\countainer_files):

- Dockerfile
- docker-compose.yml
- FoundryVTT-XX.YYY.zip (Available on [https://foundryvtt.com](foundryvtt.com) if you already purchased a license - **download the Linux version!**)

Create a new file (e.g. in Notepad) and save it as **Dockerfile** without any file name extension. This file describes the installation of your container:

```yaml
# Based on availability of versions, you might need to update the alpine version and the nodejs version - see version available here https://hub.docker.com/_/node/
FROM alpine:3.20

# Set the foundry install home
RUN adduser -D foundry
RUN mkdir -p /home/foundry/fvtt  #do not change any paths in this file, these are the paths to be set up inside the docker container
RUN mkdir -p /home/foundry/fvttdata

ENV FOUNDRY_HOME=/home/foundry/fvtt
ENV FOUNDRY_DATA=/home/foundry/fvttdata

RUN apk add --update nodejs=20.15.1-r0  #if you get an error in nodejs update, search for newer alpine version and update the nodejs version based on the error message from terminal

# Set the current working directory
WORKDIR "${FOUNDRY_HOME}"

#copy found
COPY ./FoundryVTT-XX.YYY.zip .  #insert exact name of your downloaded FoundryVTT zip file - case sensitive

#unzip
RUN unzip FoundryVTT-XX.YYY.zip
RUN rm FoundryVTT-XX.YYY.zip

EXPOSE 30000
CMD node ${FOUNDRY_HOME}/resources/app/main.js --dataPath=${FOUNDRY_DATA}
```

Next, create another new file and name it **docker-compose.yaml** - it describes the creation of a docker image (compared to trotroyanas's guide, here we also add the network connection):
```yaml
networks:
  my-net-name:
    external: true
    
services:
   fvtt:
        container_name: Fvtt
        build:
            context: ./
            dockerfile: ./Dockerfile
        image: fvtt:3.0
        volumes:  #change the paths below to your actual paths, but only the first half left of the semicolon
          - D:\FoundryVTT\countainer_files:/home/foundry/fvtt
          - D:\FoundryVTT\resources:/home/foundry/fvttdata
        ports:
            - "30000:30000"
```

### Compose the docker image
Now that you have all three files in the folder, navigate in Command Prompt to the same folder (if you don't know how to navigate folders in Command Prompt, google it) and run the following command, to compose the image file:

```
docker-compose build
```
If you get any errors during the composing process, check the comments in the yaml Dockerfile and you should be able to fix it. Simply change the line that is giving the error, resave the file and run this command again.

### Run the Docker container using the image that you just created:
Still in the Command Prompt, paste in the following code (again change the paths to your actual ones):
```
docker run ^
--init ^
--sig-proxy=false ^
--restart unless-stopped ^
--name Fvtt ^
--publish 30000:30000 ^
--volume D:\FoundryVTT\countainer_files:/home/foundry/fvttd ^ 
--volume D:\FoundryVTT\resources:/home/foundry/fvttdata ^
fvtt:3.0
```

You should now see your Fvtt container also running in Docker. 
Try accessing your subdomain.domain.xyz in your webbroser to see if it works. You should see the Foundry setup page. 
> If the page doesn't load, try accessing the Foundry on the host server by going to the browser and typing localhost:30000 
{.is-warning}

If that works, it means the settings of your cloudflare tunnel is somehow not working (e.g. you entered wrong URL target for the hostname or you did not connect your domain to CLoudflare properly) or you did not setup the container network in Docker properly.

If that doesn't work, the problem is most likely in the setup of your docker container (and most likely the container is not running and is shown as stopped in Docker Desktop. 

## Updating Foundry
I did not test this myself yet but based on other comments I have read this should work. In case you need to update Foundry version, first stop your game (by returning to Setup) and backup your world. Then stop the docker container. Download the new version of FoundryVTT-XX.YYY.zip and place it into the container_files folder instead of the old file. Update the file version in the yaml file. Run the docker-compose build command again. Run the container again. Foundry version should be updated.



