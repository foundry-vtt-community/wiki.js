---
title: Always Free Oracle Cloud Hosting Guide for Foundry
description: This guide provides easy to follow steps for a relatively simple installation of Foundry plus a reverse proxy using Caddy at the end of which you will have a functional cloud-hosted Foundry installation using Oracle Cloud.
published: true
date: 2021-09-14T18:45:57.319Z
tags: 
editor: markdown
dateCreated: 2021-04-21T17:55:20.522Z
---

# Always Free Oracle Cloud Hosting Guide for Foundry
# A. Overview
## Objective 

This guide provides easy to follow steps for a relatively simple installation of Foundry plus a reverse proxy using Caddy at the end of which you will have a functional cloud-hosted Foundry installation using [Oracle Cloud](https://www.oracle.com/cloud/free/) with optional backups and S3 integration at no cost with no time limit. 

* You will end up with:

  1. A VM with 1 cpu core (optionally increased to 4 cores) and 6GB of RAM (optionally increased to 24GB).
  2. Foundry running 24/7, including after restarts.
  3. A domain name and an automatically managed encrypted connection to your Foundry instance.
  4. Roughly 40GB (optionally increased to roughly 190GB) available storage in the [User Data folder](https://foundryvtt.com/article/configuration/#where-user-data).
  5. Outbound data transfer of 10TB per month, more than enough for hosting Foundry even with daily sessions.
  6. A backup policy that automatically keeps 5 backups in case of emergencies.
  


## Important Information and Requirements
This guide assumes that you are not an existing customer with [Oracle Cloud](https://www.oracle.com/cloud/free/) and that the services set up fall within the Oracle Always Free Tier resulting in no monthly charges. Potential pitfalls or notes to be aware of when using the Always Free Tier will be highlighted wherever appropriate. 

The following is required to complete this guide:
1.  Basic understanding of using a terminal that includes the ssh utility, such as:
a.  Powershell in Windows.
b.  Terminal in Linux and MacOS.
2.  Notepad or other text editor.
3. Valid domain name (for secure connection with HTTPS only), such as:
a.  A purchased domain name.
b.  A free subdomain from a service like [Duck DNS](http://duckdns.org). 
4. A valid credit card is required to sign up for the Always Free Tier services on Oracle Cloud. Authorization charges may appear temporarily. 


## Disclaimer 
While this guide is written to target the Always Free Tier and should not result in any charges if followed correctly, you are fully responsible for ensuring that no costs will be incurred. It is recommended to conduct a [Cost Analysis](https://cloud.oracle.com/account-management/cost-analysis) and/or set a budget after this guide is complete to ensure that all services are Always Free. 

All information in this guide is accurate as of the date it was written. 

# B. Account Setup
## Objective
At the end of this section, you will have a registered account with Oracle Cloud that has access to the Always Free Tier services. 

## Steps
> A valid credit card is required to sign up for an account with Oracle Cloud in order to access the Always Free Tier services. 
{.is-warning}


<a id="B1" href="#B1">B1.</a> Review the availability of [Always Free services in your preferred region](https://www.oracle.com/cloud/data-regions.html). At minimum, this guide targets the Compute VM, Block Storage, and optionally the Object Storage services. Ensure that they are available in the region you want to use. 

<a id="B2" href="#B2">B2.</a> Sign up for an account at [Oracle Cloud](https://www.oracle.com/cloud/free/), entering your personal information as well as credit card information when prompted. Ensure that you select the proper region to set as your Home Region. Once this is selected, it cannot be changed. 

>When choosing your home region, pay special attention to any notices regarding availability of `Arm Ampere A1 Compute` capacity. Be very sure to choose a region with availability otherwise you will not be able to create the instance mentioned in this guide and access the Always Free services. {.is-warning}

<details><summary>What if I can't choose a region that has Arm Ampere A1 availability? ▼</summary>
	
  The older `VM.Standard.E2.1.Micro` instances should still be available in all regions. That instance type is limited to 1 throttled OCPU, 1GB RAM, and 50mbps external network speeds. While lower in resources than the new A1 instances, it is still enough to meet the **minimum** requirements to host Foundry.
  
  You can choose the `VM.Standard.E2.1.Micro` shape in step [C24](#C24) instead of the `VM.Standard.A1.Flex` shape mentioned in the step, and then continue the guide.
</details>

<a id="B3" href="#B3">B3.</a> Once your account is confirmed, a “Get Started” email will be sent to the registered email address providing access to the Oracle Cloud account.

>  Some users in certain regions may require manual account verification which could take a few days of extra time.{.is-info}

<a id="B4" href="#B4">B4.</a> **Added September 2021**: Due to changes in the A1 free tier, accounts must be upgraded to **Pay As You Go** accounts to retain services long-term. This guide is specifically written to use only `Always Free` services that will not incur any charges on a Pay As You Go account. 

<details><summary>Why do I need to upgrade my account to Pay As You Go? ▼</summary>

  As of September 2021 (possibly earlier), Oracle disables the always-free A1 Ampere instances used in this guide after roughly 60 days unless an account has been upgraded to Pay As You Go. This is confirmed in their FAQ under the [*What happens when my Free Trial expires or my credits are used up?*](https://www.oracle.com/cloud/free/faq.html#:~:text=What%20happens%20when%20my%20Free%20Trial%20expires%20or%20my%20credits%20are%20used%20up%3F) heading:
  
  >Resources identified as Always Free will not be reclaimed. After your Free Trial expires, you'll continue to be able to use and manage your existing Always Free resources, and can create new Always Free resources according to tenancy limits.

	>However, Ampere A1 Compute instances are disabled when your trial ends and then deleted (terminated) after 30 days, unless you upgrade to a paid account. To continue using Arm-based compute instances as an Always Free user, you must delete your existing Ampere A1 Compute instances and create new Ampere A1 Compute instances.
  >(Text quoted 2021-09-14)

</details>


> If the services described in the next section are not listed, you may need to wait a few more minutes for the account to be fully provisioned. A status notification appears at the top of the page when logged in to Oracle Cloud if the account has not yet been fully provisioned. {.is-info}

 

# C. Compute and Networking Setup
## Objective
At the end of this section, you will have set up a Compute VM (Virtual Machine) with Ubuntu 20.04 as well as a VCN (Virtual Cloud Network) required to host Foundry. 

## Create a VCN (Virtual Cloud Network) and Security Policy
 

<a id="C1" href="#C1">C1.</a> From the [**Get Started**](https://cloud.oracle.com/) page, click on **Set up a network with a wizard**. 

![Get Start Page](/images/oracle/image12.webp)

<a id="C2" href="#C2">C2.</a> Choose **VCN with Internet Connectivity** and click **Start VCN Wizard**. 

![Start VCN Wizard](/images/oracle/image5.webp)

<a id="C3" href="#C3">C3.</a> Enter a **VCN Name**, such as `foundry`. 

<a id="C4" href="#C4">C4.</a> Ensure that **USE DNS HOSTNAMES IN THIS VCN** is :white_medium_square:`unchecked`.

![Create VCN](/images/oracle/image19.webp)

<a id="C5" href="#C5">C5.</a> Click **Next** to proceed to the Review page.

<a id="C6" href="#C6">C6.</a> Click **Create** to create the VCN. 

<a id="C7" href="#C7">C7.</a> Once all steps here are marked "done" with a green checkmark :white_check_mark:, click **View Virtual Cloud Network**.

<a id="C8" href="#C8">C8.</a>  In this next section, we will create a security policy to allow external connections to the VCN. This is required to make Foundry accessible to the internet. To start, click on the **Public Subnet-foundry** link. 

![VCN Created](/images/oracle/image20.webp)

<a id="C9" href="#C9">C9.</a> Click **Default Security List** for foundry.

<a id="C10" href="#C10">C10.</a>  Click **Add Ingress Rules**.

<a id="C11" href="#C11">C11.</a>  Ensure that **Stateless** is :ballot_box_with_check:`checked`.

<a id="C12" href="#C12">C12.</a>  Enter `0.0.0.0/0` into the **SOURCE CIDR** field.

<a id="C13" href="#C13">C13.</a>  Enter `80,443,30000` into the **DESTINATION PORT RANGE** field. 

![Ingress Rules](/images/oracle/image25.webp)


>Ports 80 and 443 are required for HTTP and HTTPS, and 30000 is required for Foundry. Once a reverse proxy is set up (step [D38](#D38) in this guide) you may optionally remove that port as it will no longer be needed.{.is-info}

<a id="C14" href="#C14">C14.</a>  Click **Add Ingress Rules**.

>This completes the creation of a VCN with the correct security rules to allow connections to the Foundry host. {.is-info}

<a id="C15" href="#C15">C15.</a>  Click **ORACLE Cloud** at the top left to return to the **Get Started** page to continue.



## Create a Compute VM (Virtual Machine) 

<a id="C16" href="#C16">C16.</a>  From the **Get Started** page, click **Create a VM Instance**.

![Create a VM Instance](/images/oracle/image23.webp)

<a id="C17" href="#C17">C17.</a>  On the next screen, enter a name for the VM instance. The name `foundry` is used for this guide. 

![Compute Instance Name](/images/oracle/image14.webp)

<a id="C18" href="#C18">C18.</a>  Leave the default **Availability Domain**. 

![Change Image](/images/oracle/image4.webp)

<a id="C19" href="#C19">C19.</a>  Click the **Change Image** button to select a new OS image.

<a id="C20" href="#C20">C20.</a>  From the pop-out list, :ballot_box_with_check:`check` **Canonical Ubuntu**. Ensure that the OS Version is **20.04** and **not** 20.04 Minimal.

![Select non-Minimal Ubuntu 20.04](/images/oracle/ubuntu-2004.webp)

<a id="C21" href="#C21">C21.</a>  Click **Select Image**.

<a id="C22" href="#C22">C22.</a>  To change the type of VM to an Always Free Tier VM, click **Change Shape**.

![Change Shape](/images/oracle/ubuntu-ampere.webp)

<a id="C23" href="#C23">C23.</a>  In the pop-out window, click **Ampere**. 

<a id="C24" href="#C24">C24.</a>  Then, :ballot_box_with_check:`check` the **VM.Standard.A1.Flex shape**.

<details><summary>How many CPU cores and how much RAM should I use? ▼</summary>

  As of June 7, 2021 Oracle offers [significant and flexible resources](https://docs.oracle.com/en-us/iaas/Content/FreeTier/freetier_topic-Always_Free_Resources.htm#compute) for their Always Free Tier instances. You can now have up to 4 Ampere cores and 24GB of RAM spread over up to 4 instances, for free. This guide leaves the default selection of 1 core and 6GB of RAM as that is more than enough to run Foundry. 
  
  An additional core may help increase performance of Foundry slightly and a few more gigs of RAM could help in the most extreme cases for resource-intensive modules. You can flexibly edit the shape after creation if you want to adjust the resources after creation. 
  
  In the vast majority of cases, 1 core and 6GB is recommended for Foundry. 

</details>

<details><summary>What if I can't choose the VM.Standard.A1.Flex shape? ▼</summary>
	
  First, try switching to another Availability Domain (checking to ensure it is tagged `always-free`) to see if there are A1 instances in other Availability Domains within your home region. The number of Availability Domains differs in each reagion, with some regions only having 1.
  
  The older `VM.Standard.E2.1.Micro` instances should still be available in all regions. That instance type is limited to 1 throttled OCPU, 1GB RAM, and 50mbps external network speeds. While lower in resources than the new A1 instances, it is still enough to meet the **minimum** requirements to host Foundry.
  
  You can choose the `VM.Standard.E2.1.Micro` shape in step [C24](#C24) above, under the `Legacy and Specialty` section instead of the `VM.Standard.A1.Flex` shape mentioned in the step, and then continue the guide.
</details>

![Select Shape](/images/oracle/ampere-shape.webp)


>If this shape is not available to select, your account may still be provisioning. Wait until the provisioning banner at the top of the page disappears and try again. This may take a few hours in some cases. If your account is provisioned but you do not see the VM.Standard.A1.Flex shape, then you will have to contact Oracle Support to resolve the issue. Choosing other shapes may incur charges. {.is-info}

<a id="C25" href="#C25">C25.</a>  Click **Select Shape**.

<a id="C26" href="#C26">C26.</a>  Scroll down to **Configure networking**.

<a id="C27" href="#C27">C27.</a>  Ensure that **foundry** is selected for Virtual cloud network in `<compartment name>`. This should appear by default.

<a id="C28" href="#C28">C28.</a>  Ensure that **Public Subnet-foundry (Regional)** is selected for Subnet in `<compartment name>`. This should appear by default. 

![Configure Networking](/images/oracle/image28.webp)

<a id="C29" href="#C29">C29.</a>  Scroll down to **Add SSH keys**.

<a id="C30" href="#C30">C30.</a>  Click on **Save Private Key**. Save this key file in an easily accessible and memorable folder. Rename this key to `foundry.key`. 

![Save Private Key](/images/oracle/image17.webp)

>Due to security restrictions, in Windows it is recommended to save the foundry.key file in the Documents folder or similar user-specific folder. 
>
>In Linux, you may need to `chmod 600` the keyfile if ssh reports an error due to the keyfile being too open.
{.is-info}

> You must download this key and keep it safe. This key is required to connect to your instance, and it will not be shown again. 
{.is-danger}

<a id="C31" href="#C31">C31.</a> Scroll down to **Boot Volume**.

<a id="C32" href="#C32">C32.</a> Optionally, :ballot_box_with_check:`check` the **Specify a custom boot volume size** option and enter up to `200` in the **Boot volume size (GB)** box that appears.
>Leaving the default boot volume size will provide you with roughly 40GB of storage for Foundry, which is usually more than enough for even the most asset-heavy campaigns. {.is-info}

>By specifying a larger Boot Volume you will be able to have up to roughly 190GB of storage for Foundry assets. However, doing so will prevent the use of multiple Compute VM instances that are provided with the Always Free Tier (not used in this guide). Think carefully about your desired future usage for the Always Free Tier before making this change. {.is-warning}

<a id="C33" href="#C33">C33.</a>  All other options should be left at default. Click **Create**. 
>If you receive an `Out of capacity for shape VM.Standard.A1.Flex in availability domain` error, then there are no more Always Free Tier instances available at the moment. You can try selecting a different Availability Domain above (double check that it is always-free tagged). Otherwise, according to the [FAQ](https://www.oracle.com/cloud/free/faq.html), more should become available within a few days. Check back regularly to see when they become available. {.is-info}

<a id="C34" href="#C34">C34.</a>  Once the large yellow square changes from <span style="color:goldenrod">**Provisioning**</span> to <span style="color:green">**Running**</span>, the instance is ready to be used. 

<a id="C35" href="#C35">C35.</a>  Copy the **Public IP Address** and save it somewhere for future reference. This IP address is needed to connect to your VM. 

>This page contains a lot of very useful information about your Computer VM, and is a central place where adjustments can be made later on if needed. {.is-info}


# D. Server Setup and Installation
## Objective
At the end of this section you will have a functional installation of Foundry using HTTPS and Caddy as a reverse proxy. Foundry will be set to restart any time the Compute VM is restarted, managed by pm2.

## Connect to Compute VM Instance and Update
>In this section, terminal is used to refer to whichever command line interface you may be using, either Windows Powershell or Linux/MacOS terminal. On Windows cmd will not work, it must be Powershell. {.is-info}

<a id="D1" href="#D1">D1.</a> In your terminal, navigate to the folder where you saved `foundry.key`.

> In Windows, you can navigate to the folder in File Explorer. Then, hold the shift key and right click within the folder and choose Open Powershell window here.{.is-info}

<a id="D2" href="#D2">D2.</a> Run the ssh command where `<public ip address>` is the Public IP Address (do not include the `<>` brackets) of your Compute VM as noted in step [C33](#C33) above. 

```
ssh -i foundry.key ubuntu@<public ip address>
```

>If you receive a `connection refused` error, the instance is probably still starting up and needs another minute or two before it can accept connections. Wait two minutes and try again. {.is-info}

<a id="D3" href="#D3">D3.</a> You will likely be prompted to accept the ECDSA key. Type `yes` and hit <kbd>enter</kbd> to accept and continue.

<a id="D4" href="#D4">D4.</a> You should see a prompt showing **ubuntu@foundry:~$**. 

![Ubuntu Prompt](/images/oracle/image9.webp)

>All commands going forward are to be entered when you see the **ubuntu@foundry:~$** prompt only. If you do not see the prompt then make sure the previous step is not still running. Some commands may take a minute or two to process. {.is-warning}

<a id="D5" href="#D5">D5.</a> First, update the system by running the following commands (this may take a few minutes to complete):
```
sudo apt-get update
sudo apt-get upgrade -y
```
<a id="D6" href="#D6">D6.</a> Once complete, the instance is ready to continue. 

> All further commands in this section should continue to be entered into this terminal in the order written. Do not close the terminal until the end of the guide. {.is-info}


## Open Ports in iptables

> The Ubuntu image from Oracle has blocked network traffic and requires adding a rule to iptables to allow HTTP, HTTPS, and Foundry traffic. {.is-warning}

<a id="D7" href="#D7">D7.</a> We need to use iptables to open the ports within Ubuntu to allow network traffic on the needed ports. We can do that by:
```
sudo iptables -I INPUT 6 -m state --state NEW -p tcp --match multiport --dports 80,443,30000 -j ACCEPT
sudo netfilter-persistent save
```
<a id="D8" href="#D8">D8.</a> The instance is now ready to accept connections.


## Install nodejs, pm2, nano, and unzip
> Nodejs is required to launch and run Foundry, and pm2 will be used to manage starting and stopping Foundry. {.is-info}

<a id="D9" href="#D9">D9.</a> Run the following commands to install nodejs:
```
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
sudo apt-get install -y nodejs
```
<a id="D10" href="#D10">D10.</a>  Check that node was installed correctly by verifying these commands return versions:
```
node --version
npm --version
```
<a id="D11" href="#D11">D11.</a>  Then install pm2:
```
sudo npm install pm2 -g
```
<a id="D12" href="#D12">D12.</a>  Check that pm2 is installed correctly by running:
```
pm2 --version
```
<a id="D13" href="#D13">D13.</a>  Allow pm2 to start and stop Foundry when the instance restarts:
```
pm2 startup
sudo env PATH=$PATH:/usr/bin /usr/lib/node_modules/pm2/bin/pm2 startup systemd -u ubuntu --hp /home/ubuntu
```
<a id="D14" href="#D14">D14.</a>  Run the following command to install unzip:
```
sudo apt-get install nano unzip -y
```
<a id="D15" href="#D15">D15.</a>  The software needed to launch and manage Foundry is now successfully installed.

## Install and launch Foundry
<a id="D16" href="#D16">D16.</a>  Login to [FoundrVTT](https://foundryvtt.com) and navigate to the **Purchased Licenses** page. 

<a id="D17" href="#D17">D17.</a>  Select the recommended version and Linux in the downloads options. Click on the :link:`Timed URL` button to copy a download url. 

> Be sure to click the :link:`Timed URL` and not the :download:`Download` button to copy and authenticated temporary download link. This link will expire in 5 minutes, after which it will need to be copied again from the gear. {.is-info}

<a id="D18" href="#D18">D18.</a>  Run the following commands, pasting the download url where you see `<download url>`. In most terminals, you can right click to paste the copied url.
```
mkdir ~/foundry
wget --output-document ~/foundry/foundryvtt.zip "<download url>"
```
> Make sure to include the quote symbols before and after the `<download url>` or the file may not download properly. {.is-info}

<a id="D19" href="#D19">D19.</a>  Once downloaded, extract Foundry and cleanup the zip file:
```
unzip ~/foundry/foundryvtt.zip -d ~/foundry/
rm ~/foundry/foundryvtt.zip
```
<a id="D20" href="#D20">D20.</a>  Create the User Data folder for Foundry to store data:
```
mkdir -p ~/foundryuserdata
```
<a id="D21" href="#D21">D21.</a>  Test that Foundry runs successfully by running the following command:
```
node /home/ubuntu/foundry/resources/app/main.js --dataPath=/home/ubuntu/foundryuserdata
```
<a id="D22" href="#D22">D22.</a>  You should see these <span style="color:green">info</span> lines at the end of the output, indicating that Foundry is successfully running. 

![Foundry Launched](/images/oracle/image29.webp)

<a id="D23" href="#D23">D23.</a>  Test the connection to Foundry by opening `http://<public IP address>:30000` in a new browser tab, where `<public IP address>` is the Public IP Address noted earlier in the guide.

>You should see a Foundry screen asking for a license key at this point. If you do not see a Foundry screen at this point likely the steps taken in [Create a VCN and Security Policy](#create-a-vcn-virtual-cloud-network-and-security-policy), or [Open Ports in iptables](#open-ports-in-iptables) were incorrect, or an incorrect IP address was used. Review the steps to ensure that no errors were made and try again. {.is-info}

<a id="D24" href="#D24">D24.</a>  In the terminal window, press <kbd>ctrl</kbd>-<kbd>c</kbd> to stop the Foundry test. You should see the last few lines as below, and a blinking cursor at **ubuntu@foundry:~/foundry$**.

![Stop Foundry](/images/oracle/image24.webp)

<a id="D25" href="#D25">D25.</a>  We will now set Foundry to be managed by pm2 so that Foundry will always be running, even in the case where the instance has been restarted. To do so, run the following command:
```
pm2 start "node /home/ubuntu/foundry/resources/app/main.js --dataPath=/home/ubuntu/foundryuserdata" --name foundry
```
<a id="D26" href="#D26">D26.</a>  Double check pm2 has launched Foundry correctly:
```
pm2 list
```
![pm2 List](/images/oracle/image1.webp)
>If the **status** column does not show <span style="color:green">online</span> then review step [D25](#D25) above before continuing. {.is-warning}

<a id="D27" href="#D27">D27.</a>  Check to see if Foundry is running correctly by again connecting a browser tab to `http://<public IP address>:30000`. 

> If you don’t see a Foundry screen in the browser at this point, carefully check the pm2 command in step 25. You can use commands like `pm2 stop`, `pm2 list`, and `pm2 delete` to remove any extra entries in the list. {.is-info}

<a id="D28" href="#D28">D28.</a>  If the connection to Foundry is successful, use the following command to save Foundry so that it will start automatically:
```
pm2 save
```
<a id="D29" href="#D29">D29.</a>  Foundry is now launched and is fully functional. You can test the interface and functions within Foundry at the `http://<public IP address>:30000` address as you like.

> If you do not wish to set up a domain or reverse proxy, you can stop here and continue to use Foundry in this way. {.is-info}


## Reverse Proxy and HTTPS Configuration with Caddy

> This section assumes that you have a valid domain name with an A record pointing to `<public IP address>`. If you do not have a domain name. you can use a service like [Duck DNS](http://duckdns.org) to get a free domain and point it to `<public IP address>`. Having a domain name is required for this section. {.is-info}


<a id="D30" href="#D30">D30.</a>  Install Caddy to use as a reverse proxy by running the following commands:
```
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo apt-key add -
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee -a /etc/apt/sources.list.d/caddy-stable.list
sudo apt update
sudo apt install caddy
```
<a id="D31" href="#D31">D31.</a>  Now that Caddy is installed and running, modify the Caddyfile to include a reverse proxy to Foundry. Open the Caddyfile:
```
sudo nano /etc/caddy/Caddyfile
```
<a id="D32" href="#D32">D32.</a>  Delete all the text, and replace it with (making sure to replace the `your.hostname.com` portion with your actual domain name):
```
# This replaces the existing content in /etc/caddy/Caddyfile

# A CONFIG SECTION FOR YOUR HOSTNAME
your.hostname.com {
    # PROXY ALL REQUEST TO PORT 30000
    reverse_proxy localhost:30000
    encode zstd gzip
}

# Refer to the Caddy docs for more information:
# https://caddyserver.com/docs/caddyfile
```
<a id="D33" href="#D33">D33.</a>  Press <kbd>ctrl</kbd>-<kbd>x</kbd> and then <kbd>y</kbd>, and then <kbd>enter</kbd> to save the changes to the file. 

<a id="D34" href="#D34">D34.</a>  Restart Caddy to pick up the new settings by:
```
sudo service caddy restart
```
>Caddy handles all forwarding to HTTPS as well as the encryption certificates. No further configuration is needed to get those working. {.is-info}

<a id="D35" href="#D35">D35.</a> Tell Foundry that we are running behind a reverse proxy by changing the `options.json` file. Open the file for editing by:
```
nano /home/ubuntu/foundryuserdata/Config/options.json
```
<a id="D36" href="#D36">D36.</a> Find the `proxySSL` and `proxyPort` parameters, and change them as below. Leave all other options as they are.
```
...
"proxyPort": 443,
...
"proxySSL": true,
...
```
>Make sure to not delete any commas or other JSON elements while editing this file. Change ONLY the values afer the `:` {.is-warning}

<a id="D37" href="#D37">D37.</a> Press <kbd>ctrl</kbd>-<kbd>x</kbd> then <kbd>y</kbd> and <kbd>enter</kbd> to save your changes.

<a id="D38" href="#D38">D38.</a>  Test your site by opening a new browser tab to your domain name. If everything is working, you will see Foundry load and the site will have the encrypted lock icon. It is now ready for use and no further configuration is needed. 

>Sometimes DNS records can take a few minutes and up to a couple hours to be recognized across the internet. If you receive an error along the lines of `server IP address could not be found` or `having trouble finding that site` then the DNS records may just need more time. Wait a few minutes and try again. {.is-warning}

> This concludes the portion of the guide that sets Foundry up and running. You may now continue using Foundry this way without issue going forward. However, if you want to set up backups or configure the S3 storage you can continue below. {.is-info}


# E. Optional: Backup Policy Setup
## Objective
At the end of this section you will have a policy that automatically retains 5 rotating backups, allowing you to seamlessly restore from backup should something ever go wrong.  

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

![Add Schedule](/images/oracle/image27.webp)

> This schedule will set a rotating backup of 5 weeks, with the oldest week being deleted as a new one is created after 5 weeks. You are able to set your own schedule as desired, however the Always Free Tier only includes 5 backups. If you exceed 5 backups, the oldest will be deleted regardless of the retention period. {.is-info}

<a id="E7" href="#E7">E7.</a> Click **Add Schedule**.

<a id="E8" href="#E8">E8.</a> Assign the backup policy to the existing volume on our Compute VM by clicking the menu button, and choosing **Compute** -> **Instances**.

<a id="E9" href="#E9">E9.</a> Click the **Foundry** instance.

<a id="E10" href="#E10">E10.</a>  Scroll down the page, and click **Boot Volume** unde the Resources menu on the left side. 

<a id="E11" href="#E11">E11.</a>  Click the **instance-xxxxxxxxx (Boot Volume)** link under the Boot Volume Name heading.

<a id="E12" href="#E12">E12.</a>  Click the **Edit** button near the top of the page, just under the instance-xxxxxx (Boot Volume) title.

<a id="E13" href="#E13">E13.</a>  Scroll down the pop-out pane to Backup Policies, and select the **foundry-backup** policy from the dropdown. Click **Save Changes**.

<a id="E14" href="#E14">E14.</a>  You have now given your instance a backup policy that will run automatically. 

> Restoring from backup is beyond the scope of this guide. More information can be found in the [Oracle Docs](https://docs.oracle.com/en-us/iaas/Content/Block/Tasks/restoringavolumefromabackup.htm) should you need to restore from backup. {.is-info} 

# F. Optional: Accessing Userdata Files
## Objective
If you would like to access the files in your userdata directory directly to move, delete, manage, or bulk upload, then we will need to set up Cyberduck to do so. 

## Set Up Cyberduck
You may follow the steps in the Linux Installation Guide to [set up Cyberduck](https://foundryvtt.wiki/en/setup/linux-installation#optional-d-accessing-userdata-files-with-cyberduck) to access your userdata files. Be sure to use the SSH Private Key method using the key file saved in a safe place. 

## Nemo or Nautilas
Linux users won't be able to install CyberDuck, but they don't need to, because they can simply connect to their server within their File Browser.

<a id="F1" href="#F1">F1.</a> Move the `foundry.key` file that you created earlier in this guide to your `~/.ssh` directory, then open a terminal there.

<a id="F2" href="#F2">F2.</a> Create a new `.pem` file from it:
```
openssl rsa -in foundry.key -text > foundry.pem
```

<a id="F3" href="#F3">F3.</a> Edit your `.ssh` config:
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

<a id="F4" href="#F4">F4.</a> Update the permissions of the new `.pem` file and the `.ssh` folder:
```
sudo chmod 600 foundry.pem
sudo chmod 755 ~/.ssh
```

<a id="F5" href="#F5">F5.</a> Open your File Browser.
### Nautilas
Go to "Other Locations" and enter `ssh://foundry` in the the text field on the bottom.
![nautilas.webp](/images/oracle/nautilas.webp)

### Nemo
Go to **File -> Connect to Server...** and then enter `foundry` where it says `server`:
![nemo.webp](/images/oracle/nemo.webp)

# G. Optional: S3 Storage Setup
## Objective
At the end of this section, you will have a functional S3 storage bucket that Foundry can access to store assets under the “Amazon S3” tab in the file picker. This allows you to have extra storage beyond that provided by the instance volume and serve large assets more efficiently. 

>S3 storage can provide a way for large assets to be served to players in a more efficient way than from the instance created earlier in this guide. In most cases, Foundry scenes should load quickly without using S3. Setting up an S3 store is only recommended in cases where faster loading speeds or more storage space is required. {.is-info}

>This section has been removed since reports of it not playing well within Foundry. Once a fix has been found, it will be modified and updated. If an S3 store is desired/needed, please look into setting up an [AWS S3 store](https://foundryvtt.wiki/en/setup/hosting/Self-Hosting-on-AWS#h-5-simple-storage-service-s3). {.is-danger}