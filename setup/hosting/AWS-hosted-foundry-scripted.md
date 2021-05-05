---
title: AWS hosted foundry scripted
description: An automated deployment of a server on aws using AWS Cloudformation script
published: true
date: 2021-05-05T17:42:34.207Z
tags: template, aws
editor: markdown
dateCreated: 2021-04-23T09:43:25.124Z
---

# Create a foundry server on AWS


Based on the two excellent tutorials on this wiki: [Self-Hosting-on-AWS](/en/setup/hosting/Self-Hosting-on-AWS) and [Ubuntu-VM](/en/setup/hosting/Ubuntu-VM) I scripted their solutions using cloudformation. You only need to get a few values to fill (will point out where to find them) in the script:

  - EC2 AMI
  - EC2 instancetype
  - Hosted Zone ID
  - Domain name 
  - Email (for the certificate) 
  - The foundry installation link
  - VPC Id
  
 If you know how and where to get these skip to the deployment step.
 Here is the script: [template.yml](/development/scripted-aws-foundry-setup/template.yml)

## AMI and Instancetype

##### InstanceType
If you are new to AWS you'll be eligible for free tier. Free tier allows you 750 hour of ec2 usage for a specific type per month for the first year using AWS. for most regions that is t2.micro (so that is the default), exception being Stockholm and Bahrain where you should use t3.micro. If you are not eligable for free tier you may want to use another type, for me t3a.micro is cheaper and better. 

##### AMI
Take the ami for ubuntu, go to ec2 and click on launch instances retrieve the ami ubuntu 20.04 ![get-ami.png](/development/scripted-aws-foundry-setup/get-ami.png) for 


## Hosted Zone and DomainName
![hostedzoneid.png](/development/hostedzoneid.png)
If you don't have a domain in route 53 yet, you should get one now (or move it here, cost for dns record is 50 cent per month. So if you have a Domain named example.com you should input foundry.example.com in the parameters. It will add that record in the hosted zone as an alias (A) record. 

!!IMPORTANT!!
If you stop and start and ec2 you will get a new public ip address, you can update the A record in the hosted zone with the new IP address

## Email Address
The email address is used to request a certificate on your machine during initial provisioning. The certificate is  self signed with certbot and is valid for 3 months (it is free) and you need to renew it after that period (will show how)

## The foundry installation link
The link is only valid for 5 minutes, get this parameter last. You'll find it in your foundry account under purchased licenses. click on the link.
![foundrylink.png](/development/foundrylink.png)

## VPC ID
Every AWS account has a VPC in it to start, look it up under the VPC Service.
![vpcid.png](/development/vpcid.png)

## Deploying the script
Go to the cloudformation service. In the top right corner click on Create Stack and select the option "with new resources". Then click the button "Choose file" and upload the template. ![upload-file.png](/development/upload-file.png)

Following page enter all the parameters you've gathered and decide on a name for your cloudformation stack. And click next and check the checkboxes at the end and see your foundry instance get created.

![parameters.png](/development/parameters.png)

## Troubleshooting.

Even after the stack is completed it will take a few minutes before everything in the script has finished. So be patient for a few minutes. The most likely issue to encounter is that the link for nodejs foundry is not downloaded properly. The stack should then fail. Simply make it again using a fresh DownloadURL.


If everything has succeeded and it is not working check the logs on the ec2. You connect to the ec2 as follows. You click on the connect button.

![connect.png](/development/scripted-aws-foundry-setup/connect.png)

![connect2.png](/development/scripted-aws-foundry-setup/connect2.png)

Once connected you can look at the logs generated during the setup, they are split up for readability's sake.
```
nano /var/log/initial-log.log
nano /var/log/start.log
nano /var/log/user-data.log
```

If you encounter an error that cannot be fixed by running it again please leave a comment with your error on my github: https://github.com/dirkvandooren/AWS_FoundryVTT/issues 

To renew the certificate.
```
certbot renew
```

small bug to be fixed:
if you restart the server your user "ssm-user" does not have the proper permissions to start foundry. if you login using the connect button and run these three lines your setup will be fixed and it will not happen again:
```
sudo chown ssm-user /home/ssm-user -R
pm2 start "node /home/ssm-user/foundry/resources/app/main.js --port=8080" --name "foundry"
pm2 save
```