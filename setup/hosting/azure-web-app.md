---
title: Azure App Service
description: Getting Started with Foundry VTT hosted in Azure
published: true
date: 2021-12-10T03:43:23.963Z
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
| ----------- | ----------- |
| Docker Image | Prebuilt Image| armyguy255a/foundryvtt:latest |
| Git Repository | Manual Build |[URL](https://github.com/ArmyGuy255A/foundryvtt-docker/tree/armyguy/azureci)

### Docker Compose
While docker images remain consistent, compose files may change from time-to-time. Therefore, please use the following compose file when running this image within Azure. It's important that Foundry runs on port 80. The Application Service or Container Instance will take care of handling all the SSL requirements for you.

Here's the basic docker-compose.yml file we'll use for the deployments.


### SSL
No need to worry about SSL if you are on a paid tier. It comes standard. If you want to implement a custom domain name with a custom SSL certificate, that's not a problem either. You can pick up a cheap .app domain name from GoDaddy that comes with a free SSL certificate for about $19. 

## Let's PaaS
Nobody can deny that running Foundry on your favorite laptop at home is pretty sweet. But, imagine telling your friends that you're running it in the cloud without the virtual machine. That's right... Each of these implemetations takes advantage of a concept known as Platform as a Service. This is the cheapest way to implement anything in the cloud. Let's get started!

## Getting Started
First, ensure you have an active Azure Subscription. You can get one here: [Azure](https://portal.azure.com)

Once you're in, I highly recommend opening an [**Azure Cloud Shell**](https://docs.microsoft.com/en-us/azure/cloud-shell/overview) and using the commands I provide in this tutorial.

1. Open a new Cloud Shell
	a. You can create a new or use an existing storage account. (It's basically free.)
  - **Note** : Do this if you want lots of persistent storage for modules.

2. Create the .yml file in the Cloud Shell
```bash
code docker-compose.yml
```

3. Jump to the appropriate section that suit's your requirement

| Tier | Description |
| ----------- | ----------- |
| BUDGETEER | No dedicated backend storage |
| WEEKENDER | Persistent Storage but no SSL |
| ENTHUSIEST | When you gotta have the right Domain Name with SSL & Persistent Storage |

### BUDGETEER (FREE)
All you need for this implementation is a free App Service Plan and an App Service. 

1. Copy the following YAML into the *docker-compose.yml* in the cloud shell
```yml
version: "3.8"

services:
  foundry:
    image: armyguy255a/foundryvtt:latest
    hostname: yourAppName.azurewebsites.net
    init: true
    restart: always
    environment:
      - FOUNDRY_PASSWORD=YourFoundryPassword
      - FOUNDRY_USERNAME=YourFoundryUsername
      - FOUNDRY_ADMIN_KEY=yourCoolFoundaryPassword123
      - CONTAINER_PRESERVE_OWNER=^.*data.*$
      - CONTAINER_PRESERVE_CONFIG=true
```
2. Ensure you replace everything in here with your values. 
3. Save and close the file. 

#### Deploy via CLI

```bash
#/bin/bash

# Variables
appName="FoundryVTT-$random"
asp="$appName-ASP"
resourceGroup=FoundryVTT
location="EastUS"

# Create a Resource Group
az group create --name $resourceGroup --location $location

# Create an App Service Plan
az appservice plan create --name $asp --resource-group $resourceGroup --location $location --is-linux --sku F1

# Create a Web App
az webapp create --name $appName --plan $asp --resource-group $resourceGroup

# Configure Web App with a Multi-Container Docker Compose file
az webapp config container set --multicontainer-config-file docker-compose.yml --name $appName --resource-group $resourceGroup

# Copy the result of the following command into a browser to see the web app.
echo http://$appName.azurewebsites.net
```

