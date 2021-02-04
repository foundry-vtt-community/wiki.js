---
title: Always Free Oracle Cloud Hosting Guide for Foundry
description: A guide to set up cloud-hosted Foundry installation using Oracle Cloud with optional backups and S3 integration at no cost with no time limit.
published: true
date: 2021-02-04T20:28:21.567Z
tags: 
editor: markdown
dateCreated: 2021-02-04T18:31:17.191Z
---

# Always Free Oracle Cloud Hosting Guide for Foundry

## A. Overview
### Objective 
At the end of this guide, you will have a functional cloud-hosted Foundry installation using [Oracle Cloud](https://www.oracle.com/cloud/free/) with optional backups and S3 integration at no cost with no time limit. This guide provides easy to follow steps for a relatively simple installation of Foundry plus a reverse proxy using Caddy. 
&nbsp;
### Important Information and Requirements
This guide assumes that you are not an existing customer with [Oracle Cloud](https://www.oracle.com/cloud/free/) and that the services set up fall within the Oracle Always Free Tier resulting in no monthly charges. Potential pitfalls or notes to be aware of when using the Always Free Tier will be highlighted wherever appropriate. 

The following is required to complete this guide:
1.	Basic understanding of using a terminal that includes the ssh utility, such as:
a.	Powershell in Windows.
b.	Terminal in Linux and MacOS.
2.	Notepad or other text editor.
3.	Valid domain name (for secure connection with HTTPS only), such as:
a.	A purchased domain name.
b.	A free subdomain from a service like [Duck DNS](http://duckdns.org). 

A valid credit card is required to sign up for the Always Free Tier services on Oracle Cloud. 

&nbsp;
### Disclaimer
While this guide is written to target the Always Free Tier and should not result in any charges if followed correctly, the responsibility to confirm that no charges will be incurred is on you. It is recommended to conduct a [Cost Analysis](https://cloud.oracle.com/account-management/cost-analysis) and/or set a budget after this guide is complete to ensure that all services are Always Free. 

All information in this guide is accurate as of the date it was written. 
&nbsp;
## B. Account Setup
### Objective
At the end of this section, you will have a registered account with Oracle Cloud that has access to the Always Free Tier services. 
&nbsp;
### Steps
> A valid credit card is required to sign up for an account with Oracle Cloud in order to access the Always Free Tier services. 
{.is-warning}


1.	Review the availability of [Always Free services in your preferred region](https://www.oracle.com/cloud/data-regions.html). At minimum, this guide targets the Compute VM, Block Storage, and optionally the Object Storage services. Ensure that they are available in the region you want to use. 
2.	Sign up for an account at [Oracle Cloud](https://www.oracle.com/cloud/free/), entering your personal information as well as credit card information when prompted. Ensure that you select the proper region to set as your Home Region. Once this is selected, it cannot be changed. 
3.	Once your account is confirmed, a “Get Started” email will be sent to the registered email address providing access to the Oracle Cloud account.

>  Some users in certain regions may require manual account verification which could take a few days of extra time.{.is-info}


> If the services described in the next section are not listed, you may need to wait a few more minutes for the account to be fully provisioned. A status notification appears at the top of the page when logged in to Oracle Cloud if the account has not yet been fully provisioned. {.is-info}

 
&nbsp;
## C. Compute and Networking Setup
### Objective
At the end of this section, you will have set up a Compute VM (Virtual Machine) with Ubuntu 20.04 as well as a VCN (Virtual Cloud Network) required to host Foundry. 
&nbsp;
### Create a VCN (Virtual Cloud Network) and Security Policy
 

1.	From the [**Get Started**](https://cloud.oracle.com/) page, click on **Set up a network with a wizard**. 
&nbsp;
![Get Start Page](/images/oracle/image12.webp)
&nbsp;
2.	Choose **VCN with Internet Connectivity** and click **Start VCN Wizard**. 
&nbsp;
![Start VCN Wizard](/images/oracle/image5.webp)
&nbsp;
3.	Enter a **VCN Name**, such as `foundry`. 
4.	Ensure that **USE DNS HOSTNAMES IN THIS VCN** is :white_medium_square:`unchecked`.
&nbsp;
![Create VCN](/images/oracle/image19.webp)
&nbsp;
5.	Click **Next** to proceed to the Review page.
6.	Click **Create** to create the VCN. 
7.	Once all steps here are marked "done" with a green checkmark :white_check_mark:, click **View Virtual Cloud Network**.
8.	 In this next section, we will create a security policy to allow external connections to the VCN. This is required to make Foundry accessible to the internet. To start, click on the **Public Subnet-foundry** link. 
&nbsp;
![VCN Created](/images/oracle/image20.webp)
&nbsp;
9.	Click **Default Security List** for foundry.
10.	Click **Add Ingress Rules**.
11.	Ensure that **Stateless** is :ballot_box_with_check:`checked`.
12.	Enter `0.0.0.0/0` into the **SOURCE CIDR** field.
13.	Enter `80,443,30000` into the **DESTINATION PORT RANGE** field. 
&nbsp;
![Ingress Rules](/images/oracle/image25.webp)
&nbsp;

>Ports 80 and 443 are required for HTTP and HTTPS, and 30000 is required for Foundry. Once a reverse proxy is set up (step [D.38](#Reverse-Proxy-and-HTTPS-Configuration-with-Caddy) in this guide) you may optionally remove that port as it will no longer be needed.{.is-info}

14.	Click **Add Ingress Rules**.

>This completes the creation of a VCN with the correct security rules to allow connections to the Foundry host. {.is-info}

15.	Click **ORACLE Cloud** at the top left to return to the **Get Started** page to continue.

&nbsp;

### Create a Compute VM (Virtual Machine) 

16.	From the **Get Started** page, click **Create a VM Instance**.
&nbsp;
![Create a VM Instance](/images/oracle/image23.webp)
&nbsp;
17.	On the next screen, enter a name for the VM instance. The name `foundry` is used for this guide. 
&nbsp;
![Compute Instance Name](/images/oracle/image14.webp)
&nbsp;
18.	Leave the default **Availability Domain**. 
&nbsp;
![Change Image](/images/oracle/image4.webp)
&nbsp;
19.	Click the **Change Image** button to select a new OS image.
20.	From the pop-out list, choose **Canonical Ubuntu 20.04**.
21.	Click **Select Image**.
22.	To change the type of VM to an Always Free Tier VM, click **Change Shape**.
&nbsp;
![Change Shape](/images/oracle/image15.webp)
&nbsp;
23.	In the pop-out window, click **Specialty and Legacy**. 
24.	Then, :ballot_box_with_check:`check` the **VM.Standard.E2.1.Micro shape**.
&nbsp;
![Select Shape](/images/oracle/image11.webp)
&nbsp;

>If this shape is not available to select, your account may still be provisioning. Wait until the provisioning banner at the top of the page disappears and try again. This may take a few hours in some cases. If your account is provisioned but you do not see the VM.Standard.E2.1.Micro shape, then you will have to contact Oracle Support to resolve the issue. Choosing any other shape will incur charges. {.is-info}

25.	Click **Select Shape**.
26.	Scroll down to **Configure networking**.
27.	Ensure that **foundry** is selected for Virtual cloud network in `<compartment name>`. This should appear by default.
28.	Ensure that **Public Subnet-foundry (Regional)** is selected for Subnet in `<compartment name>`. This should appear by default. 
&nbsp;
![Configure Networking](/images/oracle/image28.webp)
&nbsp;
29.	Scroll down to **Add SSH keys**.
30.	Click on **Save Private Key**. Save this key file in an easily accessible and memorable folder. Rename this key to `foundry.key`. 
&nbsp;
![Save Private Key](/images/oracle/image17.webp)

>Due to security restrictions, in Windows it is recommended to save the foundry.key file in the Documents folder or similar user-specific folder. {.is-info}

> You must download this key and keep it safe. This key is required to connect to your instance, and it will not be shown again. 
{.is-danger}

31.	All other options should be left at default. Click **Create**. 
32.	Once the large yellow square changes from yellow **Provisioning** to a green **Running**, the instance is ready to be used. 
33.	Copy the **Public IP Address** and save it somewhere for future reference. This IP address is needed to connect to your VM. 

>This page contains a lot of very useful information about your Computer VM, and is a central place where adjustments can be made later on if needed. {.is-info}

&nbsp;
## D. Server Setup and Installation
### Objective
At the end of this section you will have a functional installation of Foundry using HTTPS and Caddy as a reverse proxy. Foundry will be set to restart any time the Compute VM is restarted, managed by pm2.
&nbsp;
### Connect to Compute VM Instance and Update
>In this section, terminal is used to refer to whichever command line interface you may be using, either Windows Powershell or Linux/MacOS terminal. On Windows cmd will not work, it must be Powershell. {.is-info}

1.	In your terminal, navigate to the folder where you saved `foundry.key`.

> In Windows, you can navigate to the folder in File Explorer. Then, hold the shift key and right click within the folder and choose Open Powershell window here.{.is-info}

2.	Run the ssh command where `<public ip address>` is the Public IP Address of your Compute VM as noted in step C.33 above. 

```
ssh -i foundry.key ubuntu@<public ip address>
```

3.	You will likely be prompted to accept the ECDSA key. Type `yes` and hit <kbd>enter</kbd> to accept and continue.
4.	You should see a prompt showing **ubuntu@foundry:~$**. 
&nbsp;
![Ubuntu Prompt](/images/oracle/image9.webp)
&nbsp;
5.	First, update the system by running the following commands (this may take a few minutes to complete):
```
sudo apt-get update
sudo apt-get upgrade -y
```
6.	Once complete, the instance is ready to continue. 

> All further commands in this section should continue to be entered into this terminal in the order written. Do not close the terminal until the end of the guide. {.is-info}

&nbsp;
### Open Ports in iptables

> The Ubuntu image from Oracle has blocked network traffic and requires adding a rule to iptables to allow HTTP, HTTPS, and Foundry traffic. {.is-warning}

7.	We need to use iptables to open the ports within Ubuntu to allow network traffic on the needed ports. We can do that by:
```
sudo iptables -I INPUT 6 -m state --state NEW -p tcp --match multiport --dports 80,443,30000 -j ACCEPT
sudo netfilter-persistent save
```
8.	The instance is now ready to accept connections.

&nbsp;
### Install nodejs, pm2, and unzip
> Nodejs is required to launch and run Foundry, and pm2 will be used to manage starting and stopping Foundry. {.is-info}

9.	Run the following commands to install nodejs:
```
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
sudo apt-get install -y nodejs
```
10.	Check that node was installed correctly by verifying these commands return versions:
```
node --version
npm --version
```
11.	Then install pm2:
```
sudo npm install pm2 -g
```
12.	Check that pm2 is installed correctly by running:
```
pm2 --version
```
13.	Allow pm2 to start and stop Foundry when the instance restarts:
```
pm2 startup
sudo env PATH=$PATH:/usr/bin /usr/lib/node_modules/pm2/bin/pm2 startup systemd -u ubuntu --hp /home/ubuntu
```
14.	Run the following command to install unzip:
```
sudo apt-get install unzip -y
```
15.	The software needed to launch and manage Foundry is now successfully installed.
&nbsp;
### Install and launch Foundry
16.	Login to [FoundrVTT](https://foundryvtt.com) and navigate to the **Purchased Licenses** page. 
17.	Click on the gear icon (:link:) next to the **Node.js** latest version to copy a download url. 

> Be sure to click the gear icon (:link:) and not the link itself to copy and authenticated temporary download link. This link will expire in 5 minutes, after which it will need to be copied again from the gear. {.is-info}

18.	Run the following commands, pasting the download url where you see `<download url>`
```
mkdir ~/foundry
cd ~/foundry
wget -O foundryvtt.zip “<download url>”
```
> Make sure to include the quote symbols before and after the `<download url>` or the file may not download properly. {.is-info}

19.	Once downloaded, extract Foundry:
```
unzip foundryvtt.zip
rm foundryvtt.zip
```
20.	Create the User Data folder for Foundry to store data:
```
mkdir -p ~/.local/share/FoundryVTT
```
21.	Test that Foundry runs successfully by running the following command:
```
node /home/ubuntu/foundry/resources/app/main.js
```
22.	You should see the above lines at the end of the output, indicating that Foundry is successfully running. 
&nbsp;
![Foundry Launched](/images/oracle/image29.webp)
&nbsp;
23.	Test the connection to Foundry by opening `http://<public IP address>:30000` in a new browser tab, where `<public IP address>` is the Public IP Address noted earlier in the guide.

>You should see a Foundry screen asking for a license key at this point. If you do not see a Foundry screen at this point likely the steps taken in [Create a VCN and Security Policy](#create-a-vcn-virtual-cloud-network-and-security-policy), or [Open Ports in iptables](#open-ports-in-iptables) were incorrect, or an incorrect IP address was used. Review the steps to ensure that no errors were made and try again. {.is-info}

24.	In the terminal window, press <kbd>ctrl</kbd>-<kbd>c</kbd> to stop the Foundry test. You should see the last few lines as above, and a blinking cursor at **ubuntu@foundry:~/foundry$**.
&nbsp;
![Stop Foundry](/images/oracle/image24.webp)
&nbsp;
25.	We will now set Foundry to be managed by pm2 so that Foundry will always be running, even in the case where the instance has been restarted. To do so, run the following command:
```
pm2 start “node /home/ubuntu/foundry/resources/app/main.js” --name foundry
```
26.	Double check pm2 has launched Foundry correctly:
```
pm2 list
```
![pm2 List](/images/oracle/image1.webp)
&nbsp;
27.	Check to see if Foundry is running correctly by again connecting a browser tab to `http://<public IP address>:30000`. 

> If you don’t see a Foundry screen in the browser at this point, carefully check the pm2 command in step 25. You can use commands like `pm2 stop`, `pm2 list`, and `pm2 delete` to remove any extra entries in the list. {.is-info}

28.	If the connection to Foundry is successful, use the following command to save Foundry so that it will start automatically:
```
pm2 save
```
29.	Foundry is now launched and is fully Functional. You can test the interface and functions within Foundry at the `http://<public IP address>:30000` link as you like.

> If you do not wish to set up a domain or reverse proxy, you can stop here and continue to use Foundry in this way. {.is-info}

&nbsp;
### Reverse Proxy and HTTPS Configuration with Caddy

> This section assumes that you have a valid domain name with an A record pointing to `<public IP address>`. If you do not have a domain name. you can use a service like [Duck DNS](http://duckdns.org) to get a free domain and point it to `<public IP address>`. Having a domain name is required for this section. {.is-info}


30.	Install Caddy to use as a reverse proxy by running the following commands:
```
echo "deb [trusted=yes] https://apt.fury.io/caddy/ /" | sudo tee -a /etc/apt/sources.list.d/caddy-fury.list
sudo apt-get update
sudo apt-get install caddy 
```
31.	Now that Caddy is installed and running, modify the Caddyfile to include a reverse proxy to Foundry. Open the Caddyfile:
```
sudo nano /etc/caddy/Caddyfile
```
32.	Delete all the text, and replace it with (making sure to replace the `your.hostname.com` portion with your actual domain name):
```
# This replaces the existing content in /etc/caddy/Caddyfile

# A CONFIG SECTION FOR YOUR HOSTNAME
your.hostname.com {
    # PROXY ALL REQUEST TO PORT 30000
    reverse_proxy localhost:30000
}

# Refer to the Caddy docs for more information:
# https://caddyserver.com/docs/caddyfile
```
33.	Press <kbd>ctrl</kbd>-<kbd>x</kbd> and then <kbd>y</kbd>, and then <kbd>enter</kbd> to save the changes to the file. 
34.	Restart Caddy to pick up the new settings by:
```
sudo service caddy restart
```
>Caddy handles all forwarding to HTTPS as well as the encryption certificates. No further configuration is needed to get those working. {.is-info}

35. Tell Foundry that we are running behind a reverse proxy by changing the `options.json` file. Open the file for editing by:
```
nano /home/ubuntu/.local/share/Foundry-VTT/Config/options.json
```
36. Find the `proxySSL` and `proxyPort` parameters, and change them as below. Leave all other options as they are.
```
...
"proxyPort": 443,
...
"proxySSL": true,
...
```
>Make sure to not delete any commas or other JSON elements while editing this file. Change ONLY the values afer the `:` {.is-warning}
37. Press <kbd>ctrl</kbd>-<kbd>x</kbd> then <kbd>y</kbd> and <kbd>enter</kbd> to save your changes.
38.	Test your site by opening a new browser tab to your domain name. If everything is working, you will see Foundry load and the site will have the encrypted lock icon. It is now ready for use and no further configuration is needed. 

> This concludes the portion of the guide that sets Foundry up and running. You may now continue using Foundry this way without issue going forward. However, if you want to set up backups or configure the S3 storage you can continue below. {.is-info}

&nbsp;
## E. Optional: Backup Policy Setup
### Objective
At the end of this section you will have a policy that automatically retains 5 rotating backups, allowing you to seamlessly restore from backup should something ever go wrong.  
&nbsp;
### Set Backup Policy and Attach to Volume

1.	Sign into your Oracle Cloud account, and from the **Get Started** page click on the three bars on the top left to open the menu. Click on **Block Storage** -> **Backup Policies**.
&nbsp;
![Backup Policies Menu](/images/oracle/image18.webp)
&nbsp;
2.	Click **Create Backup Policy**.
3.	Enter a name such as `foundry-backup`. Leave all other options default.
4.	Click **Create Backup Policy**.
&nbsp;
![Create Backup Policy](/images/oracle/image13.webp)
&nbsp;
5.	Click **Add Schedule**.
6.	Enter the following options (or change as you see fit):
a.	Schedule Type: `Weekly`
b.	Day of the Week: `Monday`
c.	Hour of the day: `03:00`
d.	Retention Time in Weeks: `5`
e.	Backup Type: `Incremental`
f.	Timezone: `Regional Data Center Time`
&nbsp;
![Add Schedule](/images/oracle/image27.webp)

> This schedule will set a rotating backup of 5 weeks, with the oldest week being deleted as a new one is created after 5 weeks. You are able to set your own schedule as desired, however the Always Free Tier only includes 5 backups. If you exceed 5 backups, the oldest will be deleted regardless of the retention period. {.is-info}

7.	Click **Add Schedule**.
8.	Assign the backup policy to the existing volume on our Compute VM by clicking the menu button, and choosing **Block Storage** -> **Volume Groups**.
&nbsp;
![Volume Groups Menu](/images/oracle/image21.webp)
&nbsp;
9.	Click **Create Volume Group**.
10.	Enter a name for the volume group, such as `foundry-volume-group`. 
11.	Select the `foundry-backup` backup policy.
12.	Select the `foundry (Boot Volume)` volume.
13.	Click **Create**.
&nbsp;
![Create Volume Group](/images/oracle/image26.webp)
&nbsp;
14.	You have now given your instance a backup policy that will run automatically. 

> Restoring from backup is beyond the scope of this guide. More information can be found in the [Oracle Docs](https://docs.oracle.com/en-us/iaas/Content/Block/Tasks/restoringavolumefromabackup.htm) should you need to restore from backup. {.is-info} 

&nbsp;
## F. Optional: S3 Storage Setup
### Objective
At the end of this section, you will have a functional S3 storage bucket that Foundry can access to store assets under the “Amazon S3” tab in the file picker. This allows you to have extra storage beyond that provided by the instance volume. 
&nbsp;
### Create S3 Storage Bucket and Connect to Foundry

> The Always Free Tier includes 10GiB of Object Storage (S3 Storage). If that limit is exceeded, the data will simply be lost. Keep an eye on the amount of storage used in the bucket created below. {.is-warning}

1.	Click on the menu button, then **Object Storage** -> **Object Storage**. 
&nbsp;
![Object Storage Menu](/images/oracle/image10.webp)
&nbsp;
2.	Click **Create Bucket**.
3.	Give it a name, such as `foundry-bucket`. 
4.	Click **Create**.
&nbsp;
![Create Bucket](/images/oracle/image16.webp)
&nbsp;
5.	Click on `foundry-bucket`.
6.	Click on **Edit Visibility**.
&nbsp;
![Edit Visibility](/images/oracle/image7.webp)
&nbsp;
7.	Set the visibility to `public`. Leave the default of **Allow users to list objects from this bucket** as :ballot_box_with_check:`checked`.
8.	Copy the **Namespace** (blanked in this guide for security, will look like a random series of letters) and paste it somewhere to hold on to for now. 
&nbsp;
![Bucket Information](/images/oracle/image31.webp)
&nbsp;
9.	We now need to generate an accessKeyID and a secretAccessKey. Click on your profile button, then click on your email address.
&nbsp;
![Profile](/images/oracle/image3.webp)
&nbsp;
10.	Scroll down and click on **Customer Secret Keys**, then **Generate Secret Key**.
&nbsp;
![Customer Secret Keys](/images/oracle/image32.webp)
&nbsp;
11.	Enter a name such as `foundry-bucket`.
12.	Click **Generate Secret Key**.
13.	In the pop-up window, click **Copy** to copy the secretAccessKey. Paste it somewhere temporarily to hold on to. 
&nbsp;
![Copy Generated Key](/images/oracle/image22.webp)

> This window only appears once, and when you close it you will not be able to see or copy the generated key again. Ensure that you have safely copied the key before closing the window {.is-danger} 
 
14.	Beside the `foundry-bucket` key, hover your mouse over the last few characters under **Access Key**. Click **copy** to copy your accessKey. Hold on to this as well.
&nbsp;
![Copy Access Key](/images/oracle/image33.webp)
&nbsp;
15.	Click on your home region button, and then click **Manage Regions**. (Actual region name will reflect what was chosen when signing up). 
&nbsp;
![Manage Regions](/images/oracle/image8.webp)
&nbsp;
16.	Copy the **Region Identifier** of your home region (Actual region name will reflect what was chosen when signing up). Hold on to this value as well. 
&nbsp;
![Copy Region Identifier](/images/oracle/image30.webp)
&nbsp;
17.	Connect to your instance using your terminal and ssh as instructed in steps [D.1-4](#connect-to-compute-vm-instance-and-update).
18.	Run the following command to create an s3.json file that will contain the relevant information needed for Foundry to connect to the S3 storage bucket that was just created:
```
nano /home/ubuntu/.local/share/FoundryVTT/Config/s3.json
```
19.	In the nano window, paste the following making sure to replace the values that were copied earlier:
```
{
  "accessKeyId": "<copied accessKeyId>",
  "secretAccessKey": "<copied secretAccessKey>",
  "region": "<copied region identifier>",
  "endpoint": "https://<copied namespace>.compat.objectstorage.<copied region identifier>.oraclecloud.com",
  "s3ForcePathStyle":true,
  "signatureVersion":"v4"
}
```

> Be extra careful to fully replace all the placeholders marked with <> tags, including those in the endpoint url. The url should have both the namespace and the region identifier in it. Pay special attention to the format of the json file and avoid removing any quotation marks, brackets, or commas. Do not introduce any extra spaces into the values. {.is-warning}

20.	Press <kbd>ctrl</kbd>-<kbd>x</kbd> then <kbd>y</kbd>, and <kbd>enter</kbd> to save the file. 
21.	Open Foundry in your browser, then navigate to the **Configuration** page.
22.	Enter `/home/ubuntu/.local/share/FoundryVTT/Config/s3.json` into the **AWS Configuration Path**. 
23.	Click Save Changes to commit the changes and allow Foundry to restart. Press <kbd>F5</kbd> if the screen does not reappear after a few seconds. 
24.	You now have an additional tab in the File Picker within Foundry to access “Amazon S3.” Files can be uploaded and used there. 



