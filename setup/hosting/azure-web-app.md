---
title: Azure App Service
description: Getting Started with Foundry VTT hosted in Azure
published: true
date: 2021-12-16T03:43:02.061Z
tags: azure, self-hosting, docker, app service, web app, container, application service, web application
editor: markdown
dateCreated: 2021-12-10T03:41:47.183Z
---

# Azure App Service
So, you want to host Foundry in Azure? Let's do it!

## Cost
Azure can accomodate a friendly budget for most people. If you need a development environment, you could host Foundry completely for **free** (with a license of course!). If you are a once-a-week type of gamer, you could expect costs of about **$5-$10** a month. If you need something more robust, you could expect to spend about **$15-$20**. It's completely up to you!

## Docker
Great shout-out to [Felddy](https://github.com/felddy/foundryvtt-docker#readme) for creating the docker container for Foundry. I made a couple of tweaks to the image to make it compatible within Azure. We'll be using my docker image located here: 
| Name | Description | URL |
| ----------- | ----------- | ----------- |
| Docker Image | Prebuilt Image| armyguy255a/foundryvtt:latest |
| Git Repository | Manual Build |[URL](https://github.com/ArmyGuy255A/foundryvtt-docker/tree/armyguy/azureci) |

### Docker Compose
While docker images remain consistent, compose files may change from time-to-time. Therefore, please use the following compose file in this wiki to run within Azure. It's important that Foundry runs on port 80 in Azure. The Application Service or Container Instance will take care of handling all the SSL requirements for you.

### SSL
No need to worry about SSL if you are on a paid tier. If you care about encryption, be sure to choose the B1 subscription plan. It comes standard. If you want to implement a custom domain name with a custom SSL certificate, that's not a problem either. You can pick up a cheap .app domain name from GoDaddy that comes with a free SSL certificate for about $19. The B1 service plan will allow you to use a cert from GoDaddy and register your custom domain name.

## Let's PaaS
Nobody can deny that running Foundry on your favorite laptop at home is pretty sweet. But, imagine telling your friends that you're running it in the cloud without the virtual machine. That's right... Each of these implemetations takes advantage of a concept known as Platform as a Service. This is the cheapest way to implement anything in the cloud. Let's get started!

## Getting Started
First, ensure you have an active Azure Subscription. You can get one here: [Azure](https://portal.azure.com)

### Dev / Test Tier (F1 or B1)
All you need for this implementation is an F1 or B1 App Service Plan and an App Service. 

1. Copy the following YAML into the *docker-compose.yml* and save it locally. You will use this .yml file when you deploy to Azure
```yml
version: "3.8"

services:
  foundry:
    image: armyguy255a/foundryvtt:latest
    hostname: YourAppName.azurewebsites.net
    init: true
    restart: always
    volumes:
      - ${WEBAPP_STORAGE_HOME}/Data:/data
    environment:
      - FOUNDRY_PASSWORD=YourFoundryPassword
      - FOUNDRY_USERNAME=YourFoundryUsername
      - FOUNDRY_ADMIN_KEY=YourFoundryVTTAdminPassword
      - CONTAINER_CACHE=/data/cache
      - CONTAINER_PRESERVE_OWNER=^.*data.*$
      - CONTAINER_PATCHES=/data/patches
      - CONTAINER_PRESERVE_CONFIG=true
      - CONTAINER_VERBOSE=true
    ports:
      - 80:2085
```

### Deploy via CLI

Note: I appologize for not populating this section. I found some serious problems when deploying via the CLI and it required significant changes to make operational. I will only post instructions for manually deploying. You CAN deploy this via CI/CD if you wanted to. This is beyond the scope of this wiki entry. 

### Deploy via Azure Portal

1. Ensure you select the .yml file that is suits your budget. Remember, you get what you pay for!

2. Navigate to https://portal.azure.com

3. Click on "+ Create a Resource"

4. Create a Web App
**Basics Tab**
a. Recommend creating a new Resource Group called **FoundryVTT**
b. Name: Use a **unique** name
c. Publish: **Docker Container**
d. Operating System: **Linux**
e. Region: Choose a location **closest** to you
f. App Service Plan: Choose the Tier that suits your lifestyle. **You can always scale this up or down**.
**Docker Tab**
a. Options: **Docker Compose (Preview)**
b. Image Source: **Docker Hub**
c. Access Type: **Public**
d. Configuration File: Select your **docker-compose** file
**Review Tab**
a. Ensure you selected the correct App Service Plan tier. F1 is Free.
b. Click **Review + Create**
c. Click **Create**
**Post Deployment**
a. Click **Go To Resource** or navigate to your App Service you named in the Basic's Tab
b. Click on the **Configuration** blade
c. Set the **WEBSITES_ENABLE_APP_SERVICE_STORAGE** application setting to **true**\
	**Note**: This will enable persistent storage across reboots on the app service. 
d. Click on the **Overview** blade
e. Restart the App Service
f. Navigate to your application using the URL provided in the **Overview** blade

**Note**: Your installation can take from 5-10 minutes to initialize. Please be patient as your app service downloads the docker image and initializes

### Better Backend Storage (Storage Account)
I prefer to use a storage account to house all of my data. You should too. It's cheap and a great way to make sure you never lose your hardcore, custom-built campaign. Here's what you need to do!
1. Deploy a storage account


### Troubleshooting
Troubleshooting can be a pain. Luckily, there are only a few things that can go wrong here.

#### : ( Application Error
This is pretty common, and frustrating. Use these steps to help troubleshoot.
1. Click on the **Deployment Center** blade
2. Click on **Logs**

Scan through the logs and find your issue/resolution below. 

#### Foundry doesn't load the License Agreement
- This seems to happen most frequently when using the Free Tier. You could try using the B1 tier. It's still fairly cheap. I'm not sure what the exact cause is here. It could be related to the available CPU on the free tier. I think it might just be too slow, I'm afraid. You can always switch this back to F1 at the end of your game session. This will save you some $$$. 
- Maybe you didn't wait long enough. Try refreshing the browser.

#### Stopping site *YourAppNameHere* because it failed during startup.
- This one is easy. Something happened with your persistent volume.
- The app service can't map the volume in your docker-compose. Usually caused because the WEBSITES_ENABLE_APP_SERVICE_STORAGE setting wasn't set to **true**. Ensure you correct this problem, restart the app service and try again.




