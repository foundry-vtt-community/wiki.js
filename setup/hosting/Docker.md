---
title: Docker
description: 
published: true
date: 2021-06-10T20:55:15.926Z
tags: 
editor: markdown
dateCreated: 2020-09-23T00:34:32.550Z
---

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

---

# DirecktHit's Guide to Running FVTT-Docker with Traefik and Portainer

Please visit [DirecktHit's blog](https://benprice.dev/posts/fvtt-docker-tutorial/) for the most up to date directions.

## DirecktHit's Docker Hub Image via `docker-compose`

Please visit the README in the [fvtt-docker repository](https://github.com/BenjaminPrice/fvtt-docker) for the most up to date directions.

---


test