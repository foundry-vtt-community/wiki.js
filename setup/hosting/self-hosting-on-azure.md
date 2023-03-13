---
title: Self Hosting on Azure
description: Simple Steps to follow to setup an Azure Free Tier VM
published: true
date: 2023-03-13T18:06:06.329Z
tags: azure, hosting, setup, getting started, vm
editor: markdown
dateCreated: 2020-11-19T05:34:12.177Z
---

>**NOTE:** The Azure Free Tier provides 1GB of RAM. This is below the [minimum requirements](https://foundryvtt.com/article/requirements/#dedicated-server) for a dedicated Foundry server. You run the risk of running out of RAM and Foundry crashing, especially on heavier game systems, using many modules, or importing a lot of content into a world. {.is-warning}

# Self Hosting on Azure
## 1. Introduction, Requirements, and A Disclaimer
So you ran out of your free year on AWS, did you? Or maybe you prefer to give Microsoft your patronage instead of Amazon? No matter the reason, welcome aboard the Azure Cloud Platform, where the slogan is "we have Linux stuff too!"

You may have seen the AWS article and noticed that I'm cribbing the format quite a bit. For example, this is the part of the article where I link [the free tier options that Azure provides](https://azure.microsoft.com/en-us/free/free-account-faq/). You'll find Azure's free tier quite generous. It will allow you to run two of their lowest tier VMs, one in Linux and one in Windows!

...Okay, we'll be ignoring the Windows portion of Azure for this tutorial, but it's there if you want to play around with it. Perhaps more useful to us, each VM has 64GiB of disk storage provisioned to it in free tier, so most users can go without using S3 bucket storage too if you're really trying to avoid AWS.

If you do need more than that, S3 might be your best option for now. Follow steps 3, 5, 6 and 7 of the [excellent AWS tutorial](https://foundryvtt.wiki/en/setup/hosting/Self-Hosting-on-AWS) once you've completed these steps.

## 2. Setting up an Azure Account

Go to azure.com and click Try free. You can sign in with your existing Microsoft account, Github account, or make a new Microsoft account. You will have to have a valid credit card on your Microsoft account to use Azure.

## 3. Setting up a Virtual Machine

1. Type `virtual machines` in the search bar.

1. Under Services, select Virtual machines.

1. In the Virtual machines page, select Add. The "Create a virtual machine page" opens.

1. In the Basics tab, under Project details, make sure the correct subscription is selected and then choose to Create new resource group. Type a resource group name. I recommend `FoundryResourceGroup`, but you can choose anything you like. I recommend putting ResourceGroup or RG in the name.

1. Under Instance details, type a Virtual machine name and choose your closest [Region](https://azure.microsoft.com/en-us/global-infrastructure/geographies/). I recommend a name like `FoundryVM-EastUS`, but using your chosen region.

1. Choose Ubuntu 18.04 LTS for your Image and choose Standard_B1s for your Size. These settings are important to keep your VM under free tier, although they aren't marked as such.

1. Under Administrator account, select SSH public key.

1. In Username type a username. Don't lose it! The default `azureuser` is also fine.

1. For SSH public key source, leave the default of Generate new key pair.

1. Under Inbound port rules > Public inbound ports, choose Allow selected ports and then select SSH (22), HTTP (80), and HTTPS (443) from the drop-down.

1. Click the button labeled Next: Disks.

1. For OS disk size, select Resize to 64 GiB (P6) Free account eligible from the drop-down. If it's not there, click previous and ensure you've chosen

1. Leave the remaining defaults and then select the Review + create button at the bottom of the page.

1. On the Review page, you can see the details about the VM you are about to create. When you are ready, select Create.

1. When the Generate new key pair window opens, select Download private key and create resource. Your key file will be download as `VMName_key.pem`. Make sure you know where the .pem file was downloaded, you will need the path to it in the next step.

1. When the deployment is finished, select Go to resource.

1. On the page for your new VM, select the public IP address and copy it to clipboard. Paste it somewhere to reference in the next step.

## 4. Connecting to your VM

You'll need the key pair you downloaded in order to log in via ssh. Before you log in, though, you'll need to change the permissions on the keyfile so other users on your computer can't read it. On Linux/MacOS, you can do so with the following command:

```
chmod 600 /path/to/keypair.pem
```

Follow the answer [here](https://superuser.com/questions/1296024/windows-ssh-permissions-for-private-key-are-too-open) to change file permissions in Windows.

A typical SSH command to log into an EC2 instance at the command line looks like this, with the appropriate information replaced. If you changed the username, use that instead of azureuser.

```
ssh -i /path/to/keypair.pem azureuser@<your-instance-public-ip>
```

This should work in both a Linux or MacOS terminal, *and* Windows PowerShell in Windows 10. Just remember that Windows points its \ slashes the other way.

You're now ready connected to your new Azure-hosted Ubuntu server. You can now follow the rest of the guide [here.](https://foundryvtt.wiki/en/setup/linux-installation)
