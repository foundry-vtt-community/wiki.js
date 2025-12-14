---
title: Always Free Oracle Cloud Hosting Guide for Foundry
description: This guide provides easy to follow steps for a relatively simple installation of Foundry plus a reverse proxy using Caddy at the end of which you will have a functional cloud-hosted Foundry installation using Oracle Cloud.
published: true
date: 2025-12-14T18:53:14.505Z
tags: foundry, oracle, free, linux, reverse proxy, cloud, https, cloud host, host, foundryvtt, always free, oci, ssl
editor: markdown
dateCreated: 2021-04-21T17:55:20.522Z
---

# Always Free Oracle Cloud Hosting Guide for Foundry
# A. Overview
## Objective 

This guide provides steps for a relatively simple installation of Foundry plus a reverse proxy using Caddy at the end of which you will have a functional cloud-hosted Foundry installation using [Oracle Cloud](https://www.oracle.com/cloud/free/) with optional backups and S3 integration at no cost with no time limit. 

* You will end up with:

  1. A VM with 1 cpu core (optionally increased to 4 cores) and 4GB of RAM (optionally increased to 24GB).
  2. Foundry running 24/7, including after restarts.
  3. A domain name and an automatically managed encrypted connection to your Foundry instance.
  4. Roughly 40GB (optionally increased to roughly 190GB) available storage in the [User Data folder](https://foundryvtt.com/article/configuration/#where-user-data). 
  5. Outbound data transfer of 10TB per month, more than enough for hosting Foundry even with daily, extremely media-rich sessions.
  6. A backup policy that automatically keeps backups in case of emergencies.
  


## Important Information and Requirements
This guide assumes that you are not an existing customer with [Oracle Cloud](https://www.oracle.com/cloud/free/) and that the services set up fall within the Oracle Always Free Tier resulting in no monthly charges. Potential pitfalls or notes to be aware of when using the Always Free Tier will be highlighted wherever appropriate. 

This guide also assumes that you are able to navigate the Oracle interface without additional assistance beyond general instructions, and that you are technical enough to understand the requirements below without additional assistance.

The following is required to complete this guide:
1.  Understanding of using a terminal that includes the ssh utility, such as:
a.  Powershell in Windows.
b.  Terminal in Linux and MacOS.
2.  Notepad or other text editor.
3. Valid domain name (for secure connection with HTTPS only), such as:
a.  A purchased domain name.
b.  A free subdomain from a service like [Duck DNS](http://duckdns.org). 
4. A valid credit or debit card is required to sign up for the Always Free Tier services on Oracle Cloud. Authorization charges may appear temporarily. 

>The Oracle Free Tier is available on a limited first-come, first-serve basis. It isn't possible to predict if an instance will be available in your region until you attempt to create it. If no instances are available, one may become available at a later time - within hours, days, weeks, or potentially months. {.is-info}

## Getting Help
If you get stuck on a particular step, please first ensure that all commands in black text quotes entered *exactly* as they appear. 

Troubleshooting assistance for this guide can be found on the official Foundry Discord. Copy the link from the specific step number (ie: C31) you are having difficulty with and then post in the **#install-and-connection** channel on the [Foundry Discord](https://discord.gg/foundryvtt).

>Was your A1 **instance disabled** or **reclaimed**? 
>
>Have you received an error message stating "**instance is disabled and will not accept any actions or requests**?"
>
>See the <a href="#archived-information">archived information</a> onward for an explanation and the <a href="#j-restore-disabled-a1-instance">Restore Disabled A1 Instance</a> section for instructions on how to restore your Foundry instance. {.is-danger}


## Disclaimer 
While this guide is written to target the Always Free Tier and should not result in any charges if followed correctly, you are fully responsible for ensuring that no costs will be incurred. It is recommended to conduct a [Cost Analysis and set a budget](#g-optional-budget-and-cost-analysis)  after this guide is complete to ensure that all services are Always Free. 

All information in this guide is accurate as of the date it was written. 

>Oracle at its sole discretion may remove access to free tier accounts at any point without warning. {.is-warning} 

# B. Account Setup
## Objective
At the end of this section, you will have a registered account with Oracle Cloud that has access to the Always Free Tier services. 

## Account Creation and Upgrade
> A valid credit or debit card is required to sign up for an account with Oracle Cloud in order to access the Always Free Tier services. 
{.is-warning}


<a id="B1" href="#B1">B1.</a> Review the availability of [Always Free services in your preferred region](https://www.oracle.com/cloud/data-regions.html). At minimum, this guide targets the Compute VM, Block Storage, and optionally the Object Storage services. Ensure that they are available in the region you want to use. 

<a id="B2" href="#B2">B2.</a> Sign up for an account at [Oracle Cloud](https://www.oracle.com/cloud/free/), entering your personal information as well as credit or debit card information when prompted. Ensure that you select the proper region to set as your Home Region. Once this is selected, it cannot be changed. 

>**READ THIS WARNING**: When choosing your home region, pay very special attention to any messages about Arm Ampere A1 Compute capacity. See example below. **Do not choose regions listed in the warning message** as you will not be able to continue with this guide until the A1 instances become available, which may take **several days**. Choose the next closest geographical region to you and your players and continue. 
> ![a1.availability-warning.webp](/images/oracle/a1.availability-warning.webp)
> (Image for example only, check your warning very closely for the regions listed) {.is-danger}


<a id="B3" href="#B3">B3.</a> Once your account is confirmed, a “Get Started” email will be sent to the registered email address providing access to the Oracle Cloud account. You are now ready to continue this guide!

<details><summary>Will Oracle reclaim my Always Free instance for being idle? ▼</summary>
  In January, 2023 Oracle sent the following message regarding idle A1 (Always Free) instances:
  
  >Oracle Cloud Infrastructure Customer,

  >Oracle Cloud Infrastructure (OCI) will be reclaiming idle Always Free compute resources from Always Free customers. Reclaiming idle resources allows OCI to efficiently provide services to Always Free customers. Your account has been identified as having one or more compute instances that have been idle for the past 7 days. These idle instances will be stopped one week from today, January 30, 2023. If your idle Always Free compute instance is stopped, you can restart it as long as the associated compute shape is available in your region. You can keep idle compute instances from being stopped by converting your account to Pay As You Go (PAYG). With PAYG, you will not be charged as long as your usage for all OCI resources remains within the Always Free limits.
  
  The definition of "idle" is found here: [Always Free Tier Resources - Idle Instances](https://docs.oracle.com/en-us/iaas/Content/FreeTier/freetier_topic-Always_Free_Resources.htm#:~:text=a%20private%20subnet.-,Idle%20Compute%20Instances,-Important)
  
  Oracle support has confirmed on January 31, 2023 that *all* the idle conditions above need to be met for the instance to be considered idle. With a 4GB RAM instance, Foundry will use more than 10% at all times. Thus, following this guide after January 31, 2023 should keep the instance safe from being reclaimed.
  
  If you would like to be absolutely sure, you can upgrade your account to Pay As You Go in the [Billing & Cost Management -> Upgrade and Manage Payment](https://cloud.oracle.com/invoices-and-orders/upgrade-and-payment) section. There is no charge to do so, however a temporary authorization must be placed on a credit card.
</details>


<details><summary>Don't I need to upgrade my account to Pay As You Go or my instance will be disabled? ▼</summary>

  As of January 2022 (possibly earlier), Oracle changed their policy for A1 always free instances. They will no longer be disabled at the expiry of your free trial credits and any A1 instances set up as directed in this guide will not be disabled by Oracle at that time. 
  
  See the updated FAQ under the [*What happens when my Free Trial expires or my credits are used up?*](https://www.oracle.com/cloud/free/faq.html#:~:text=What%20happens%20when%20my%20Free%20Trial%20expires%20or%20my%20credits%20are%20used%20up%3F) heading:
  
  
  >When you've reached the end of your 30-day trial or used all your Free Trial credits (whichever comes first), you’ll be notified and will have a grace period of 30 days, starting from the expiration date, to upgrade to paid.
  > ...
  >**Resources identified as Always Free will not be reclaimed**. After your Free Trial expires, you'll continue to be able to use and manage your existing Always Free resources, and can create new Always Free resources according to tenancy limits.
  >(Emphasis added)
  
  (Text quoted 2022-01-20)
  
  A Pay As You Go account is no longer needed to prevent A1 always free instances from being disabled after the initial 30-days.
  

</details>

>  Some users in certain regions may require manual account verification which could take a few days of extra time.{.is-info}

> If the services described in the next section are not listed, you may need to wait a few more minutes for the account to be fully provisioned. A status notification appears at the top of the page when logged in to Oracle Cloud if the account has not yet been fully provisioned. {.is-info}


 

# C. Compute and Networking Setup
## Objective
At the end of this section, you will have set up a Compute VM (Virtual Machine) with Ubuntu 22.04 as well as a VCN (Virtual Cloud Network) required to host Foundry. 

>Section C has been significantly simplified and generalized due to frequent changes to the Oracle console. As this guide is intended for more advanced users, it leaves the exact navigation instructions open for those users to find the correct sections themselves. {.is-info}

>Occasionally, Oracle may ask you to select or re-select a **Compartment**. If you see a message about a missing Compartment, select the **Account Name (root)** compartment from the left hand menu, under List scope -> Compartment. {.is-warning}

## Create a VCN (Virtual Cloud Network) and Security Policy
 

<a id="C1" href="#C1">C1.</a> From the [**Get Started**](https://cloud.oracle.com/) page, navigate to **Networking**, -> **Virtual cloud networks** (VCN). 

<a id="C2" href="#C2">C2.</a> Create a new VCN with the following properties:

**VCN Name**: `foundry`
**Use DNS hostnames in this VCN**: :white_medium_square:`unchecked`.
**IPv4 CIDR Blocks**: `10.0.0.0/24`


<a id="C3" href="#C3">C3.</a> In the **Public Subnet** for the VCN created, modify the default security list to include the following **Ingress Rules**:

**Stateless**: :ballot_box_with_check:`checked`
**Source CIDR**: `0.0.0.0/0`
**Desination Ports and Protocols**: `80 TCP, 443 TCP/UDP, 30000 TCP`


>Ports 80 and 443 are required for HTTP and HTTPS, and 30000 is required for Foundry. Once a reverse proxy is set up you may optionally remove that port as it will no longer be needed. Adding port 443 UDP here enables HTTP/3 connectivity in Caddy. {.is-info}



## Create a Compute VM (Virtual Machine) 

<a id="C4" href="#C4">C4.</a>  From the **Get Started** page, navigate to **Compute** -> **Instances**.

>Occasionally, Oracle may ask you to select or re-select a **Compartment**. If you see a message about a missing Compartment, select the **Account Name (root)** compartment from the left hand menu, under List scope -> Compartment. {.is-warning}

<a id="C5" href="#C5">C5.</a> Create a compute instance with the following properties:

**Name**: `foundry`
**Availability Domain**: `default` (optionally any other AD that is marked `always-free`)
**Image**: `Ubuntu 24.04 aarch64`
**Shape**: `VM.Standard.A1.Flex`
**RAM**: `4GB`
**Cores**: `1` 
**VCN**: `foundry` (created above)
**Subnet**: `public-subnet-foundry`
**Boot Volume Size**: `Up to 200GB` (ensure you do not allocate more than 200GB between all volumes you may create)


>**If you have upgraded your account to Pay As You Go to avoid instance reclaiming** you may optionally choose more than 4GB of RAM or 1 CPU core. {.is-info}

<a id="C6" href="#C6">C6.</a>  Before completing the creation of your compute instance, be sure to   **Save Private Key**. Save this key file in an easily accessible and memorable folder. Rename this key to `foundry.key`. 


>Due to security restrictions, in Windows it is recommended to save the foundry.key file in the Documents folder or similar user-specific folder. 
>
>In Linux or MacOS, you may need to `chmod 600` the keyfile if ssh reports an error due to the keyfile being too open.
{.is-info}

> You must download this key and keep it safe. This key is required to connect to your instance, and it will not be shown again. 
{.is-danger}

<details><summary>How many CPU cores and how much RAM should I use? ▼</summary>

  As of June 7, 2021 Oracle offers [significant and flexible resources](https://docs.oracle.com/en-us/iaas/Content/FreeTier/freetier_topic-Always_Free_Resources.htm#compute) for their Always Free Tier instances. You can now have up to 4 Ampere cores and 24GB of RAM spread over up to 4 instances, for free. This guide recommends the selection of 1 core and 4GB of RAM as that is more than enough to run Foundry. 
  
  An additional core may help increase performance of Foundry slightly and a few more gigs of RAM could help in the most extreme cases for resource-intensive modules. You can flexibly edit the shape after creation if you want to adjust the resources after creation. 
  
  In the vast majority of cases, 1 core and 4GB is recommended for Foundry. 
  
  If you choose more resources, you may run the risk of having your instance reclaimed due to being idle. For more details, see [Always Free Tier Documentation](https://docs.oracle.com/en-us/iaas/Content/FreeTier/freetier_topic-Always_Free_Resources.htm#:~:text=a%20private%20subnet.-,Idle%20Compute%20Instances,-Important).

</details>

<details><summary>What if I can't choose the VM.Standard.A1.Flex shape? ▼</summary>
  
  If this shape is not available to select, your account may still be provisioning. Wait until the provisioning banner at the top of the page disappears and try again. This may take a few hours in some cases. If your account is provisioned but you do not see the VM.Standard.A1.Flex shape, then you will have to contact Oracle Support to resolve the issue. Choosing other shapes may incur charges. 
	
  You may try switching to another Availability Domain (checking to ensure it is tagged `always-free`) to see if there are A1 instances in other Availability Domains within your home region. The number of Availability Domains differs in each reagion, with some regions only having 1.

  If no Availability Domains in your home region have A1 shapes available, you will have to check back periodically for new availability. This may happen at any time as others free up resources or Oracle adds more A1 resources to the region. It may take a few days, keep checking back regularly.
  
</details>

<a id="C7" href="#C7">C7.</a> All other options should be left at default. Click **Create**. 
>If you receive an `Out of capacity for shape VM.Standard.A1.Flex in availability domain` error, then there are no more Always Free Tier instances available at the moment. You can try selecting a different Availability Domain above (double check that it is always-free tagged). Otherwise, according to the [FAQ](https://www.oracle.com/cloud/free/faq.html), more should become available within a few days. Check back regularly to see when they become available. {.is-info}

<a id="C8" href="#C8">C8.</a> Once the large yellow square changes from <span style="color:goldenrod">**Provisioning**</span> to <span style="color:green">**Running**</span>, the instance is ready to be used. 

<a id="C9" href="#C9">C9.</a> Copy the **Public IP Address** and save it somewhere for future reference. This IP address is needed to connect to your VM. 

>This page contains a lot of very useful information about your Computer VM, and is a central place where adjustments can be made later on if needed. {.is-info}


# D. Server Setup and Installation
## Objective
At the end of this section you will have a functional installation of Foundry using HTTPS and Caddy as a reverse proxy. Foundry will be set to restart any time the Compute VM is restarted, managed by pm2.

## Connect to Compute VM Instance and Update
>In this section, terminal is used to refer to whichever command line interface you may be using, either Windows Powershell or Linux/MacOS terminal. On Windows cmd will not work, it must be Powershell. 
In many terminals, you can paste text with <kbd>right-click</kbd> or <kbd>shift</kbd>+<kbd>insert</kbd>.{.is-info}

<a id="D1" href="#D1">D1.</a> In your terminal, navigate to the folder where you saved `foundry.key`.

> In Windows, you can navigate to the folder in File Explorer. Then, hold the shift key and right click within the folder and choose Open Powershell window here.{.is-info}

<a id="D2" href="#D2">D2.</a> Run the ssh command where `<public ip address>` is the Public IP Address (do not include the `<>` brackets) of your Compute VM as noted in step [C9](#C9) above. 

```
ssh -i foundry.key ubuntu@<public ip address>
```

>If you receive a `connection refused` error, the instance is probably still starting up and needs another minute or two before it can accept connections. Wait two minutes and try again.
>
>If you receive an error about the keyfile being too open, see the info at step [C6](#C6). {.is-info}

<a id="D3" href="#D3">D3.</a> You will likely be prompted to accept the ECDSA key. Type `yes` and hit <kbd>enter</kbd> to accept and continue.

<a id="D4" href="#D4">D4.</a> You should see a prompt showing **ubuntu@foundry:~$**. 

>All commands going forward are to be entered when you see the **ubuntu@foundry:~$** prompt only. If you do not see the prompt then make sure the previous step is not still running. Some commands may take a minute or two to process. {.is-warning}

<a id="D5" href="#D5">D5.</a> We need to use iptables to open the ports within Ubuntu to allow network traffic on the needed ports. We can do that by:
```
sudo iptables -I INPUT 5 -m state --state NEW -p tcp --match multiport --dports 80,443,30000 -j ACCEPT
sudo iptables -I INPUT 5 -m state --state NEW -p udp --dport 443 -j ACCEPT
sudo netfilter-persistent save
```

> All further commands should continue to be entered into this terminal in the order written. Do not close the terminal until the end of the guide. {.is-info}

<a id="D6" href="#D6">D6.</a> Complete all the steps from the [**System Setup**](https://foundryvtt.wiki/en/setup/linux-installation#system-setup) section in the Linux Installation Guide, stopping at the end of Section C.


## Final Words and Ongoing Maintenance

<a id="D7" href="#D7">D7.</a> Restart the instance in order to apply any potentialy pending updates that require a restart and to test `pm2`'s ability to restart Foundry correctly. 

```
sudo shutdown -r now
```

>Give the instance a few minutes to restart. You should be able to connect to Foundry without issue once it is fully restarted. You should also be able to `ssh` into instance as in [D2](#D2). If Foundry has not started up after a good 10 minutes, please check your `pm2` startup command. {.is-info}

<a id="D8" href="#D8">D8.</a> It is good practice to keep your new instance up to date with security patches. To do so, log in to the instance periodically with ssh as in [D2](#D2) and run the following command: 

```
sudo apt update && sudo apt upgrade -y
``` 

<a id="D9" href="#D9">D9.</a>Then, restart the instance as in [D8](#D8).

> This concludes the portion of the guide that sets Foundry up and running. You may now continue using Foundry this way without issue going forward. However, if you want to set up backups or configure the S3 storage you can continue below. {.is-info}



# E. Optional: Backup Policy Setup
## Objective
At the end of this section you will have a policy that automatically retains 5 rotating backups, allowing you to seamlessly restore from backup should something ever go wrong and know how to restore from backup when needed. 

## Set Backup Policy and Attach to Volume

<a id="E1" href="#E1">E1.</a> Sign into your Oracle Cloud account, and from the **Get Started** page click on the three bars on the top left to open the menu. Click on **Block Storage** -> **Backup Policies**.

![Backup Policies Menu](/images/oracle/image18.webp)

<a id="E2" href="#E2">E2.</a> Click **Create Backup Policy**.

<a id="E3" href="#E3">E3.</a> Enter a name such as `foundry-backup`. Leave all other options default.

<a id="E4" href="#E4">E4.</a> Click **Create Backup Policy**.

![Create Backup Policy](/images/oracle/image13.webp)

<a id="E5" href="#E5">E5.</a> Click **Add Schedule**.

<a id="E6" href="#E6">E6.</a> Enter the following options (or change as you see fit):

* Schedule Type: `Weekly`
* Day of the Week: `Monday`
* Hour of the day: `03:00`
* Retention Time in Weeks: `5`
* Backup Type: `Incremental`
* Timezone: `Regional Data Center Time`

<!-- ![Add Schedule](/images/oracle/image27.webp) -->

> This schedule will set a rotating backup of 5 weeks, with the oldest week being deleted as a new one is created after 5 weeks. You are able to set your own schedule as desired, however the Always Free Tier only includes 5 backups. If you exceed 5 backups, the oldest will be deleted regardless of the retention period. {.is-info}

<a id="E7" href="#E7">E7.</a> Click **Add Schedule**.

<a id="E8" href="#E8">E8.</a> Assign the backup policy to the existing volume on our Compute VM by clicking the menu button, and choosing **Compute** -> **Instances**.

<a id="E9" href="#E9">E9.</a> Click the **Foundry** instance.

<a id="E10" href="#E10">E10.</a>  Scroll down the page, and click **Boot Volume** unde the Resources menu on the left side. 

<a id="E11" href="#E11">E11.</a>  Click the **instance-xxxxxxxxx (Boot Volume)** link under the Boot Volume Name heading.

<a id="E12" href="#E12">E12.</a>  Click the **Edit** button near the top of the page, just under the instance-xxxxxx (Boot Volume) title.

<a id="E13" href="#E13">E13.</a>  Scroll down the pop-out pane to Backup Policies, and select the **foundry-backup** policy from the dropdown. Click **Save Changes**.

<a id="E14" href="#E14">E14.</a>  You have now given your instance a backup policy that will run automatically. 

## Restoring From Backup

>Note: Following the steps below in order requires that you have enough Always Free Tier resources available to have two instances and two boot volumes active at the same time. If you followed the guide exactly as above, this will be the case. If you have increased the CPU/RAM or Volume size above 2CPUs/12GBs or 100GB then you may be prevented from creating a new instance or boot volume or be charged a small amount for the time they are both active. 
>
> You can terminate the existing instance before step [E17](#E17) to continue in this case. Be **absolutely sure that you have a working backup** if you choose to terminate the instance in advance. You risk losing all data if you terminate the instance and boot volume without a working backup. 
>
> Instructions for terminating the instance start at [E24](#E24). {.is-danger}

> Some regions have limited availability for the `VM.Standard.A1.Flex` shapes. If your region does not have A1 availability, you will not be able to create a new instance as in step [E19](#E19). Check back periodicially for more resources to become available. It may take a few days. {.is-info}

<a id="E15" href="#E15">E15.</a> From the Oracle Menu, click **Storage** then **Block Storage**. 

<a id="E16" href="#E16">E16.</a> From the side menu, click **Boot Volume Backups**.

<a id="E17" href="#E17">E17.</a> Find the backup you want to restore. Most recent backups should be at the top of the list. Click the three-dot menu on that backup and click **Restore Boot Volume**. 

<a id="E18" href="#E18">E18.</a> In the popup window, enter a **Name** such as `Foundry-restored` for the boot volume and then select the same **Backup Policy** to ensure this new boot volume will continue to be backed up. Leave all other options as default. 

> If you do not have enough Always Free Tier resources for the above step (ie: existing boot volume above 100GB) you may be prevented from completing [E17](#E17)-[E18](#E18) entirely, or may be charged money while both boot volumes are active. {.is-warning}

<a id="E19" href="#E19">E19.</a> In the new Boot Volume Details window, wait for the boot volume to become <span style="color:green">**available**</span> and then click `Create instance`.

<a id="E20" href="#E20">E20.</a> Give the instance a name, such as `Foundry-restored`. Scroll down to verify the **Shape** indicates `VM.Standard.A1.Flex` with 1 core OCPU and 6 GB memory. The **Image** should be the `Boot Volume Name` set in step [E18](#E18).

>Do **not modify the image**. {.is-info}

<a id="E21" href="#E21">E21.</a> Scroll down to **Add SSH Keys** and click `Save Private Key`. Save this key in a safe location, preferrably the same location as you previously saved the SSH key. You will need this key if you ever want to connect to your instance via SSH in the future. 

<a id="E22" href="#E22">E22.</a> Click `Create`. You now have a new `Always Free` A1 instance running that should not be disabled again. 

> If you do not have enough Always Free Tier resources for the above step (ie: existing boot volume above 100GB) you may be prevented from completing [E19](#E19)-[E22](#E22) entirely, or may be charged money while both boot volumes are active. {.is-warning}

<a id="E23" href="#E23">E23.</a> If you set up a domain name, you will need to point the domain's A record to the instances `<Public IP address>`. If you did not set up a domain name, you can now connect to Foundry directly, using a link like this: `http://<Public IP Address>:30000`

<a id="E24" href="#E24">E24.</a> Once you have verified the new instance works correctly and contains the restored data, use the Oracle Menu to navigate to **Compute** -> **Instances**. 

<a id="E25" href="#E25">E25.</a> On the old instance that is no longer needed, click the three-dot menu and click **Terminate**. 

<a id="E26" href="#E26">E26.</a> In the window that pops up, make sure that **Permanently delete the attached boot volume** is :ballot_box_with_check:`checked`. Click **Terminate Instance**.

> Be **absolutely sure** that this is the old instance and that your restored backup is functioning correctly before terminating. **Terminating the instance will permanently delete it and all the data contained**. {.is-warning}

> You should now have a working restored backup. {.is-info}



# F. Optional: Accessing Userdata Files
## Objective
If you would like to access the files in your userdata directory directly to move, delete, manage, or bulk upload, then we will need to set up Cyberduck, Nemo, Nautilus or VSCode to do so. 

## Install and Setup Cyberduck

<a id="F1" href="#F1">F1.</a> Download and install Cyberduck for your platform from the [Cyberduck website](https://cyberduck.io/download/).

<a id="F2" href="#F2">F2.</a> Once installed, open Cyberduck and click **Open Connection**:

![Cyberduck - Open Connection](/images/generic-linux/cyberduck1.webp)

<a id="F3" href="#F3">F3.</a> In the Open Connection window, click the dropdown menu and select **SFTP (SSH File Transfer Protocol)**

![Cyberduck - Choose SFTP](/images/generic-linux/cyberduck2.webp)

<a id="F4" href="#F4">F4.</a> Enter the following information in the corresponding fields, replacing any values in `<>` with the values as earlier in the guide:

* Server: `<your.domain.name>` or `<server IP address>` 
* Username: `ubuntu` 
* Password: Leave blank
* SSH Private Key: Click `Browse` and select your `foundry.key` file. You may need to change the file type to **All Files**. 

<a id="F5" href="#F5">F5.</a> Click **Connect**

<a id="F6" href="#F6">F6.</a> Double click on the `foundryuserdata` directory, then the `Data` directory.

<a id="F7" href="#F7">F7.</a> Click the **Bookmark** menu, then **New Bookmark**. Close the window that pops up. 

>You now have a bookmarked connection in Cyberduck to the location of your Foundry userdata directory. Simply launch Cyberduck and double click the bookmark to connect and manage your files. {.is-info}

## Nemo or Nautilas
Linux users won't be able to install CyberDuck, but they don't need to, because they can simply connect to their server within their File Browser.

>This section is for Linux users only. Windows and MacOS users can use Cyberduck above. {.is-warning}

<a id="F8" href="#F8">F8.</a> Move the `foundry.key` file that you created earlier in this guide to your `~/.ssh` directory, then open a terminal there.

<a id="F9" href="#F9">F9.</a> Create a new `.pem` file from it:
```
openssl rsa -in foundry.key -text > foundry.pem
```

<a id="F10" href="#F10">F10.</a> Edit your `.ssh` config:
```
gedit ~/.ssh/config
```
Add this entry:
```
Host foundry  
  HostName <public ip address>
  User ubuntu
  Port 22
  IdentityFile ~/.ssh/foundry.pem
  Compression yes
```

<a id="F11" href="#F11">F11.</a> Update the permissions of the new `.pem` file and the `.ssh` folder:
```
sudo chmod 600 foundry.pem
sudo chmod 755 ~/.ssh
```

<a id="F12" href="#F12">F12.</a> Open your File Browser.
### Nautilas
Go to "Other Locations" and enter `ssh://foundry` in the the text field on the bottom.
![nautilas.webp](/images/oracle/nautilas.webp)

### Nemo
Go to **File -> Connect to Server...** and then enter `foundry` where it says `server`:
![nemo.webp](/images/oracle/nemo.webp)

## Install and Setup VSCode

Visual Studio Code is a full featured, free IDE from Microsoft. With the support of the remote coding extension (also from Microsoft), you gain a full remote editor and file transfer platform. This allows you to remotely edit files and is a more complete service for power-users who may also be developing modules or wish to run services in addition to Foundry on the server.

<a id="F13" href="#F13">F13.</a> Download and install VSCode for your platform from the [Microsoft VSCode website](https://code.visualstudio.com/download).
> **If on Windows you will also need to install the OpenSSH Client:**
> Open <kbd>Settings</kbd>, select <kbd>Apps</kbd>, then select <kbd>Optional Features</kbd>
> At the top of the page select <kbd>Add a feature</kbd>, then find `OpenSSH Client`, and select <kbd>Install</kbd> {.is-info}

<a id="F14" href="#F14">F14.</a> Install the free "Remote - SSH" extension from the [VSCode Marketpace Page](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh), ensuring you click the blue <kbd>Install</kbd> button.

![image.png](/setup/always-free-oracle/image.png)

<a id="F15" href="#F15">F15.</a> Press Ctrl + Shift + P to open the command palette and search for `Open SSH Configuration File`, selecting the one for your user account.

![sshconfig1.png](/setup/always-free-oracle/sshconfig1.png)

<a id="F16" href="#F16">F16.</a> Fill in the config file with a name for your Server (Host), the public IP address of your server (HostName), your username (Ubuntu by default). Add a new line with an `IdentityFile` followed by the path to your ssh key file. 
> Make sure to save the config file either with Ctrl + S or through a menu. {.is-warning}

![sshconfig2.png](/setup/always-free-oracle/sshconfig2.png)

<a id="F17" href="#F17">F17.</a> For convenience we will store the key in the same folder as the configuration file, get there by right clicking on the config file name and selecting "Reveal in File Explorer" / "Reveal in Finder".

![sshkey.png](/setup/always-free-oracle/sshkey.png)

<a id="F18" href="#F18">F18.</a> Now that the connection is setup, go to the <kbd>Remote Explorer tab</kbd> in the sidebar and click the folder icon next to your server. If you get a popup asking you to chose the Operating system of the server choose <kbd>Linux</kbd>.

![connect.png](/setup/always-free-oracle/connect.png)

![osselect.png](/setup/always-free-oracle/osselect.png)

<a id="F19" href="#F19">F19.</a> A new window will open with the name of your server in the bottom left hand corner. Use the <kbd>Open Folder</kbd> button to open your `foundrydata` folder. Congratulations, you now have access to a file editor, a file tree you can drag and drop files into and out of, and a terminal connected to your server.

# G. Optional: Budget and Cost Analysis
## Objective
At the end of this section you will confirm that your instance will not incur any charges and optionally set up a warning if charges do occur. 

## Costs
All resources mentioned and configured in this guide are `Always Free` and should never incur any costs over the lifetime of the account. If all steps were followed carefully, the Cost Analysis below should always show no costs projected. 

In the case that the Cost Analysis shows some cost projected either a non-free resource was created, or the A1 instance was created above the Always Free tier limits, or there is some kind of error on Oracle's end. 

If you see a projected cost from the Cost Analysis and are within 30 days of account creation then contact Oracle Support by clicking the Help button at the top-right of the screen, and starting a chat or creating a support request. 

## View Cost Analysis
<a id="G1" href="#G1">G1.</a> Open the Navigation Menu and click **Governane & Administration** -> **Cost Management** -> **Cost Analysis**

<a id="G2" href="#G2">G2.</a> On the Cost Analysis page, click to check the :ballot_box_with_check:`Show Forecast` option. This enables displaying future costs.

>Accounts must be at least 10 days old to access the `Show Forecast` feature and may only forecast into the future the age of the account. For example, an account that is 11 days old can only forecast 11 days into the future. 
>
>You may still conduct a Cost Analysis to confirm no costs have been incured in the recent past witout forecasting into the future. {.is-info}

<a id="G3" href="#G3">G3.</a> Choose the `Granularity`, either **Daily** or **Monthly**.

<a id="G4" href="#G4">G4.</a> If forecasting, set a date many days or months into the future under `End Forecast Date (UTC)`. 

![Cost Analysis Page Setup](/images/oracle/cost-analysis-1.webp)

<a id="G5" href="#G5">G5.</a> Click **Apply**.

>Note that it may take a few moments for the Cost Analysis to generate. {.is-info}

<a id="G6" href="#G6">G6.</a> Once it has finished generating, review the Cost Details. You should see a completely empty chart and a table with only zeroes, indicating no costs incured at any point including in the projected future.

![Cost Analysis Chart](/images/oracle/cost-analysis-2.webp)

>If you see any projected costs here, please review all the services you've set up to ensure they have an `always-free` tag. If you are within 30 days of your account creation, please contact Oracle Support for further assistance. 
>
>All steps in this guide have been carefully written to ensure that only `always-free` tagged resources are used and should not have any costs associated with them. {.is-warning}

## Set Budget Warning
<a id="G7" href="#G7">G7.</a> On the same Cost Management page as above, click **Budets**.

<a id="G8" href="#G8">G8.</a> Click **Create Budget**.

<a id="G9" href="#G9">G9.</a> Enter `foundry_budget` in the **Name** field.

<a id="G10" href="#G10">G10.</a> Enter `Budget warning for Foundry cost forecast` in the **Description** field.

<a id="G11" href="#G11">G11.</a> Under **Target Compartment** choose your `root` compartment.

<a id="G12" href="#G12">G12.</a> Under **Monthly Budget Amount** enter `1`. 

<a id="G13" href="#G13">G13.</a> Under **Budget Alert Rule (Optional)** -> **Threshold Metric** choose `Forecast Spend` to warn on upcoming charges. 

<a id="G14" href="#G14">G14.</a> Under **Threshold %** enter `1` to warn on minimum forecast charges. 

![Budget Warning Setup](/images/oracle/cost-analysis-4.webp)

<a id="G15" href="#G15">G15.</a> Enter any **Email Recipients** where you would like to receive these warnings. 

<a id="G16" href="#G16">G16.</a> Click **Create**.

>You now have a warning that will send you an email on the first of the month if there are any projected costs whatsoever. {.is-info}


# H. Optional: Updating NodeJS Version
## Objective
If you have a message from Foundry telling you that you must updated your NodeJS version, this section will walk you through how to do that. At the end of this section you'll have the latest NodeJS version installed and Foundry up and running. 

## Updating NodeJS

<a id="H1" href="#H1">H1.</a> Reconnect to your Oracle instance, as in step [D2](#D2).

<a id="H2" href="#H2">H2.</a> Once successfully connected, follow the the updating steps as outlined in the [Recommended Linux Install Guide](https://foundryvtt.wiki/en/setup/linux-installation#optional-f-updating-nodejs-on-debianubuntu).

Once completed, you are good to go!

# I. Optional: S3 Storage Setup
## Objective
At the end of this section, you will have a functional S3 storage bucket that Foundry can access to store assets under the “Amazon S3” tab in the file picker. This allows you to have extra storage beyond that provided by the instance volume and serve large assets more efficiently. 

>S3 storage can provide a way for large assets to be served to players in a more efficient way than from the instance created earlier in this guide. In most cases, Foundry scenes should load quickly without using S3. Setting up an S3 store is only recommended in cases where faster loading speeds or more storage space is required. {.is-info}

>This section has been removed since reports of it not playing well within Foundry. Once a fix has been found, it will be modified and updated. If an S3 store is desired/needed, please look into setting up an [AWS S3 store](https://foundryvtt.wiki/en/setup/hosting/Self-Hosting-on-AWS#h-5-simple-storage-service-s3). {.is-danger}

# J. Restore Disabled A1 Instance
## Objective
This section helps users who have had their A1 Ampere instances disabled at the end of the trial account period. These instances are still `Always Free` and can continue to be used. Once restored as described below, the instances should remain running without issue or fear of being disabled. 

To see why the instance has been disabled, check step B4 in the <a href="#archived-information">archived information</a>.

## Restore Instance Including All Data
  To create a new instance that retains all data up to the point it was disabled:
  
<a id="J1" href="#J1">J1.</a> To be safe, create a backup of your Boot Volume by going to **Compute** -> **Instances** -> `<Instance Name>`

<a id="J2" href="#J2">J2.</a> Scroll down to **Boot Volume** -> Click the `<Boot Volume Name>`

<a id="J3" href="#J1">J3.</a> Scroll down to **Boot Volume Backups** and click `Create Boot Volume Backup`. Enter a name for the backup, and click `Create Boot Volume Backup`. This backup is not technically needed, but is good to have just in case. 

<a id="J4" href="#J4">J4.</a> Go to **Compute** -> **Instances** and click on the `Foundry` instance. Click **More Actions** and choose `Terminate`. Make ***absolutely*** sure that `Permanently delete the attached boot volume` is ***NOT CHECKED***.

<a id="J5" href="#J5">J5.</a> Wait for the instance status to change from `Terminating` to `Terminated`. You may need to refresh the page. 

<a id="J6" href="#J6">J6.</a> Scroll down to **Boot Volume** and click the `<Boot Volume Name>`. 

<a id="J7" href="#J7">J7.</a> At the top of the page, click `Create Instance`. 

<a id="J8" href="#J8">J8.</a> Give the instance a name, such as `Foundry`. Scroll down to verify the **Shape** indicates `VM.Standard.A1.Flex` with 1 core OCPU and 6 GB memory. The **Image** should be the `Boot Volume Name`.

>Optionally, you can edit the **shape** to be up to 4 core OCPU and 24GB memory. As part of the `Always Free` services, you get up to 4 core OCPU and 24GB of member spread over up to 4 A1 instances. Please allocate accordingly.
>
>Do **not modify the image**. {.is-info}

<a id="J9" href="#J9">J9.</a> Scroll down to **Add SSH Keys** and click `Save Private Key`. Save this key in a safe location, preferrably the same location as you previously saved the SSH key. You will need this key if you ever want to connect to your instance via SSH in the future. 

<a id="J10" href="#J10">J10.</a> Click `Create`. You now have a new `Always Free` A1 instance running that should not be disabled again. 

<a id="J11" href="#J11">J11.</a> If you set up a domain name, you will need to point the domain's A record to the instances `<Public IP address>`. If you did not set up a domain name, you can now connect to Foundry directly, using a link like this: `http://<Public IP Address>:30000`

# K. Perform Clean Update/Reinstall
## Purpose
This section contains the instructions needed to remove the current installation, and replace it with a cleanly installed new version. It does not touch your userdata in any way. 

## Clean Install

<a id="K1" href="#K1">K1.</a> Reconnect to your Oracle instance, as in step [D2](#D2).

<a id="K2" href="#K2">K2.</a> Once successfully connected, follow the the updating steps as outlined in the [Recommended Linux Install Guide](https://foundryvtt.wiki/en/setup/linux-installation#optional-g-performing-a-clean-reinstallupdate).

# L. Archived: A1 Disabling and Upgrading to Pay As You Go (Reference Only)
## Purpose

This section contains outdated information, left here for reference only. This information is outdated and should only be used to reference previous instructions for informational purposes. 

## Archived Information

>The section below is reproduced as it was written before January 20th, 2022. {.is-warning}

<a>B4.</a> **Added September 2021**: Due to changes in the way trial accounts work, accounts must be upgraded to **Pay As You Go** accounts to retain services long-term without interruption. This guide is specifically written to use only `Always Free` services that will not incur any charges on a Pay As You Go account. 

<details><summary>Why do I need to upgrade my account to Pay As You Go? ▼</summary>

  As of September 2021 (possibly earlier), Oracle terminates the `Always Free` A1 Ampere instances used in this guide after roughly 60 days unless an account has been upgraded to Pay As You Go. This is confirmed in their FAQ under the [*What happens when my Free Trial expires or my credits are used up?*](https://www.oracle.com/cloud/free/faq.html#:~:text=What%20happens%20when%20my%20Free%20Trial%20expires%20or%20my%20credits%20are%20used%20up%3F) heading:
  
  >Resources identified as Always Free will not be reclaimed. After your Free Trial expires, you'll continue to be able to use and manage your existing Always Free resources, and can create new Always Free resources according to tenancy limits.
  >However, Ampere A1 Compute instances are disabled when your trial ends and then deleted (terminated) after 30 days, unless you upgrade to a paid account. To continue using Arm-based compute instances as an Always Free user, you must delete your existing Ampere A1 Compute instances and create new Ampere A1 Compute instances.
  
  (Text quoted 2021-09-14)
  
  In order to avoid data loss and service interruption, the upgrade to Pay As You Go must be done before the A1 instance is disabled.

</details>

<details><summary>Won't I be charged money if I upgrade my account to Pay As You Go? ▼</summary>

  If you use only resources tagged with the `Always Free` tag, you will never be charged for them regardless if you have a trial account or a Pay As You Go account. This guide has been written to only use those specific resources. 
  
  See [What are Always Free services?](https://www.oracle.com/cloud/free/faq.html#:~:text=What%20are%20Always%20Free%20services%3F) and [Always Free Resources](https://docs.oracle.com/en-us/iaas/Content/FreeTier/freetier_topic-Always_Free_Resources.htm#:~:text=All%20Oracle%20Cloud%20Infrastructure%20accounts%20(whether%20free%20or%20paid)%20have%20a%20set%20of%20resources%20that%20are%20free%20of%20charge%20for%20the%20life%20of%20the%20account.%20These%20resources%20display%20the%20Always%20Free%20label%20in%20the%20Console%20(for%20Ampere%20A1%20Compute%20shapes%2C%20see%20Compute).).
  
  When upgrading your account to Pay As You Go, you may see temporary authorization charges on your credit card (depending on region). These charges should be released within a few days once the authorization is complete. 
  
  We recommend conducting a [Cost Analysis](#g-optional-budget-and-cost-analysis) after completing this guide for peace of mind. The account must be a minimum of 10 days old to conduct a cost analysis forecast. 
  
  If you see any other charges on the credit card, contact Oracle support.

</details>

<details><summary>Can I continue without upgrading to Pay As You Go / What happens if I don't upgrade to Pay As You Go? ▼</summary>

  You can continue to use this guide and set up all the services without upgrading to Pay As You Go. As per the Oracle Always Free FAQ, your A1 instance (the cloud server you set up that runs Foundry) may be disabled and eventually deleted if you don't upgrade to Pay As You Go. See [*What happens when my Free Trial expires or my credits are used up?*](https://www.oracle.com/cloud/free/faq.html#:~:text=What%20happens%20when%20my%20Free%20Trial%20expires%20or%20my%20credits%20are%20used%20up%3F)
  
  You can set everything up in this guide without upgrading to Pay As You Go immediately, however if you don't do so before your A1 instance is disabled you risk losing data and would need to re-set up your server. 
  
  We recommend upgrading your account now so that you do not risk having services disabled, however you are free to choose when/if you upgrade your account.
  
  We ***highly*** recommend setting up backups in [Section E Backup Policy Setup](#e-optional-backup-policy-setup) to minimize potential data loss. 

</details>

<details><summary>I choose to  <b>&nbsp;NOT&nbsp;</b>  upgrade to Pay As You Go ▼</summary>

  You can continue to follow this guide without upgrading to Pay As You Go. All the services will work without issue for at least the first 30 days since your account creation. 
  
  After the first 30 days, your A1 instance will be disabled. This instance is what Foundry is running on. Your Foundry will become inaccessible and it will look like your Foundry installation is down. 
  
  After an additional 30 days, the Foundry A1 instance will be deleted (terminated) including any data that is not backed up. 
  
  After that point, you must delete the existing A1 instance, and recreate a new A1 instance (pending availability in your region) and attach it to your existing Boot Volume (or a volume restored from backup). 
  
  Instructions on how to restore from backup can be found in the [Oracle Documentation](https://docs.oracle.com/en-us/iaas/Content/Block/Tasks/restoringavolumefromabackup.htm).
  
  If you'd like to continue without setting up a Pay As You Go account, skip ahead to [Compute and Networking Setup](#c-compute-and-networking-setup).

</details>

<details><summary>My instance was <b>&nbsp;disabled</b>! ▼</summary>

  If your instance has been disabled, you must create a new A1 instance to continue using the Always Free Tier. You can **keep the same boot volume and all data on it**. 
  
  Please follow the steps in the <a href="#i-restore-disabled-a1-instance">Restore Disabled A1 Instance</a> section. 
  
</details>

<a>B5.</a> Open the navigation menu and click **Governance & Administration** -> **Cost Management** -> **Payment Method**.

<a>B6.</a> Under Account Type, select **Pay As You Go**.

>If you get a message that `this account is manage by enterprise agreement` and `upgrade operation is not available` then your account has not finished provisioning. Please wait a few minutes until your account has finished provisioning.
>
>If your account does not complete provisioning after 30 minutes, please contact Oracle support {.is-warning}

<a>B7.</a> If present, **Edit** your current credit card information or **Add** a new credit card. Click **Finish** when completed.

<a>B8.</a> Read the terms and conditions and :ballot_box_with_check:`check` the check box to indicate your agreement.

>Read carefully. Starting a Pay As You Go account may require temporary authorization charges to be made against your credit card. {.is-warning}

<a>B9.</a> Click **Start Paid Account**.

<a>B10.</a> Once you complete the remainder of this guide, be sure to review a [Cost Analysis](#g-optional-budget-and-cost-analysis) to ensure that no costs will be incured. The account must be at least 10 days old to forecast costs into the future, but you may view past costs before that date as confirmation. 






> The text above this point is archived text from before January 20, 2022. {.is-warning}


