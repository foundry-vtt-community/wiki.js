---
title: AWS hosted foundry scripted
description: An automated deployment of a server on aws using AWS Cloudformation script
published: true
date: 2021-04-26T08:37:46.754Z
tags: template, aws
editor: markdown
dateCreated: 2021-04-23T09:43:25.124Z
---

# Create a foundry server on AWS

###Currently fixing a bug that on restart of ec2 it does not work properly. No data is lost and if you want to have your ec2 running 24/7 then for now ignore this. but fixing it currently###

Based on the two excellent tutorials on this wiki: [Self-Hosting-on-AWS](/en/setup/hosting/Self-Hosting-on-AWS) and [Ubuntu-VM](/en/setup/hosting/Ubuntu-VM) I scripted their solutions using cloudformation. You only need to get a few values to fill (will point out where to find them) in the script:

  - EC2 AMI
  - EC2 instancetype
  - Hosted Zone ID
  - Domain name 
  - Email (for the certificate) 
  - The foundry installation link
  - VPC Id
  
 If you know how and where to get these skip to the deployment step.
 Here is the script (link at the bottom):
```
Transform: AWS::Serverless-2016-10-31
Parameters:
  AMI:
    Type: String
    Default: ami-0932440befd74cdba
  InstanceType:
    Type: String
    Default: t2.micro
  HostedZone:
    Type: String
    Default: Z0........
  DomainName:
    Type: String
    Default: foundry........
  Email:
    Type: String
    Default: your@email.com
  DownloadURL:
    Type: String
    Default: nodejs-url
  VpcId:
    Type: String
Resources:
  TemporaryS3:
    DeletionPolicy: Retain
    Type: AWS::S3::Bucket
  Lambda:
    Type: AWS::Serverless::Function
    Properties:
      Handler: index.lambda_handler
      Runtime: python3.8
      Policies:
        Version: 2012-10-17
        Statement:
          - Effect: Allow
            Action: s3:*
            Resource:
              - !Sub arn:aws:s3:::${TemporaryS3}
              - !Sub arn:aws:s3:::${TemporaryS3}/*
      Environment:
        Variables:
          s3: !Ref TemporaryS3
          url: !Ref DownloadURL
      Timeout: 60
      MemorySize: 1024
      InlineCode: |
        import os
        import boto3
        import json
        import cfnresponse
        import http.client
        file_name = "foundryvtt.zip"
        def lambda_handler(event, context):
          try:
            print(json.dumps(event))
            s3bucket = os.environ.get("s3")
            s3client = boto3.client("s3")

            if event['RequestType'] == "Create":
              url = os.environ.get("url")
              conn = http.client.HTTPSConnection(url.split("https://")[1].split("/releases")[0])
              conn.request("GET", url.split('.com')[1])
              res = conn.getresponse()
              data = res.read()
              print(f"if this is shorter than 10000 the download was not completed {len(data)}")
              if len(data) < 10000:
                cfnresponse.send(event, context, cfnresponse.FAILED, {}, "CustomResourcePhysicalID")
              else:
                s3client.put_object(
                  Bucket=s3bucket,
                  Body=data,
                  Key=file_name
                  )
                print("it acutally succeeded...")
                responseData = {}
                responseData['Location'] = f"Content is in s3 bucket {s3bucket} with name 'foundryvtt.zip' "
                cfnresponse.send(event, context, cfnresponse.SUCCESS, responseData, "CustomResourcePhysicalID")
            elif event['RequestType'] == "Delete":
              s3client.delete_object(
                Bucket=s3bucket,
                Key=file_name
              )
              s3client.delete_bucket(
                Bucket=s3bucket
              )
              cfnresponse.send(event, context, cfnresponse.SUCCESS, {}, "CustomResourcePhysicalID")
            else:
              cfnresponse.send(event, context, cfnresponse.SUCCESS, {}, "CustomResourcePhysicalID")
          except Exception as e:
            cfnresponse.send(event, context, cfnresponse.SUCCESS, {"Error": f"{e}"}, "CustomResourcePhysicalID")

  CustomResource:
    Type: Custom::DownloadFile
    Properties:
      ServiceToken: !GetAtt Lambda.Arn
  Ec2Role:
    Type: AWS::IAM::Role
    Properties:
      AssumeRolePolicyDocument:
        Version: "2012-10-17"
        Statement:
          - Effect: Allow
            Principal:
              Service:
                - ec2.amazonaws.com
            Action:
              - 'sts:AssumeRole'
      ManagedPolicyArns:
        - arn:aws:iam::aws:policy/AmazonSSMManagedInstanceCore
        - arn:aws:iam::aws:policy/CloudWatchAgentServerPolicy
      Policies:
        - PolicyDocument:
            Version: 2012-10-17
            Statement:
              - Effect: Allow
                Action: 
                  - s3:List*
                  - s3:Get*
                Resource: !Sub arn:aws:s3:::${TemporaryS3}/*
          PolicyName: !Sub "GetFoundryZip-${AWS::StackName}"
  InstanceProfile:
    Type: AWS::IAM::InstanceProfile
    Properties:
      InstanceProfileName: !Sub ec2-profile-${AWS::StackName}
      Roles:
        - !Ref Ec2Role    
  IAMUser:
    Type: AWS::IAM::User
    Properties:
      UserName: !Sub foundry-s3-user-${AWS::StackName}

  S3Bucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketEncryption:
        ServerSideEncryptionConfiguration:
          - ServerSideEncryptionByDefault:
              SSEAlgorithm: AES256
      PublicAccessBlockConfiguration:
        BlockPublicAcls: True
        BlockPublicPolicy: False
        RestrictPublicBuckets: False
        IgnorePublicAcls: True
  S3BucketPolicy:
    Type: AWS::S3::BucketPolicy
    Properties:
      Bucket: !Ref S3Bucket
      PolicyDocument:
        Version: 2012-10-17
        Statement:
          - Sid: AllowWhitelistedIPs
            Effect: Allow
            Principal: "*"
            Action: s3:GetObject
            Resource: !Sub "arn:aws:s3:::${S3Bucket}/*"
            Condition:
              IpAddress:
                aws:SourceIp: [
                  "0.0.0.0/0"
                ]
  IAMPolicy:
    Type: AWS::IAM::Policy
    Properties:
      Users:
        - !Ref IAMUser
      PolicyName: !Sub foundry-s3-policy-${AWS::StackName}
      PolicyDocument:
        Version: 2012-10-17
        Statement:
          - Sid: AllowSpecificS3Access
            Effect: Allow
            Action:
              - s3:PutObject
              - s3:GetObject
              - s3:ListObject
              - s3:DeleteObject
              - s3:PutObjectAcl
            Resource:
              - !Sub "arn:aws:s3:::${S3Bucket}/*"
              - !Sub "arn:aws:s3:::${S3Bucket}"
          - Sid: ListAllBuckets
            Effect: Allow
            Action: s3:ListAllMyBuckets
            Resource: "*"
  Credentials:
    Type: AWS::IAM::AccessKey
    Properties: 
      UserName: !Ref IAMUser
  EC2:
    Type: AWS::EC2::Instance
    Properties:
      ImageId: !Ref AMI
      SecurityGroupIds:
        - !Ref SG
      UserData:
        Fn::Base64: !Sub |
          #!/bin/bash -xe
          mkdir /tmp/ssm
          cd /tmp/ssm
          export HOME=/home/ssm-user
          exec > >(tee /var/log/initial-log.log|logger -t user-data -s 2>/dev/console) 2>&1
            curl "https://s3.amazonaws.com/session-manager-downloads/plugin/latest/ubuntu_64bit/session-manager-plugin.deb" -o "session-manager-plugin.deb"
            sudo dpkg -i session-manager-plugin.deb
            curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
            apt-get install -y nodejs 
            apt-get install nginx unzip -y
            rm /etc/nginx/sites-enabled/default
            service nginx restart      
            mkdir -p /home/ssm-user/foundry
            cd /home/ssm-user/foundry
            sudo apt-get install awscli -y
            aws s3 cp s3://${TemporaryS3}/foundryvtt.zip ./foundryvtt.zip
            unzip foundryvtt.zip
            rm foundryvtt.zip
            npm install pm2 -g
          exec > >(tee /var/log/start.log|logger -t user-data -s 2>/dev/console) 2>&1
            mkdir -p /home/ssm-user/.local/share/FoundryVTT
            pm2 start "node /home/ssm-user/foundry/resources/app/main.js --port=8080" --name "foundry"
            sudo env PATH=$PATH:/usr/bin /usr/lib/node_modules/pm2/bin/pm2 startup systemd -u ssm-user --hp /home/ssm-user
            pm2 save
            sudo chmod a+rwx /home/ssm-user -R
            echo "{
              \"accessKeyId\": \"${Credentials}\", 
              \"secretAccessKey\": \"${Credentials.SecretAccessKey}\",
              \"region\": \"${AWS::Region}\"
              }" | tee /home/ssm-user/.local/share/FoundryVTT/Config/aws.json > /dev/null
            mkdir /var/log/nginx/foundry
            echo "
            # Most sites won't have configured favicon or robots.txt
            # and since its always grabbed, turn it off in access log
            # and turn off it's not-found error in the error log
            location = /favicon.ico { access_log off; log_not_found off; }
            location = /robots.txt { access_log off; log_not_found off; }
            location = /apple-touch-icon.png { access_log off; log_not_found off; }
            location = /apple-touch-icon-precomposed.png { access_log off; log_not_found off; }

            # Rather than just denying .ht* in the config, why not deny
            # access to all .invisible files
            location ~ /\. { deny  all; access_log off; log_not_found off; }
            " | tee /etc/nginx/conf.d/drop > /dev/null

          echo "server {
            listen 80;

            # Adjust this to your the FQDN you chose!
            server_name                 ${DomainName};

            access_log                  /var/log/nginx/foundry/access.log;
            error_log                   /var/log/nginx/foundry/error.log;
            include conf.d/drop;

            location ^~ /.well-known/acme-challenge {
                allow all;
                root /var/www/letsencrypt;
                auth_basic off;
            }

            location / {
                proxy_set_header        Host \$host;
                proxy_set_header        X-Real-IP \$remote_addr;
                proxy_set_header        X-Forwarded-For \$proxy_add_x_forwarded_for;
                proxy_set_header        X-Forwarded-Proto \$scheme;

                # Adjust the port number you chose!
                proxy_pass              http://127.0.0.1:8080;

                proxy_http_version      1.1;
                proxy_set_header        Upgrade \$http_upgrade;
                proxy_set_header        Connection \"Upgrade\";
                proxy_read_timeout      90;

                # Again, adjust both your FQDN and your port number here!
                proxy_redirect          http://127.0.0.1:8080 http://${DomainName};
            }
          }" | tee /etc/nginx/sites-available/foundry > /dev/null
          ln -s /etc/nginx/sites-available/foundry /etc/nginx/sites-enabled/foundry
          sudo service nginx restart
          exec > >(tee /var/log/user-data.log|logger -t user-data -s 2>/dev/console) 2>&1
            snap install core; snap refresh core
            snap install --classic certbot
            ln -s /snap/bin/certbot /usr/bin/certbot
            sudo certbot --nginx -n --agree-tos -m ${Email} -d ${DomainName}
      InstanceType: !Ref InstanceType
      IamInstanceProfile: !Ref InstanceProfile
      Tags:
        - Key: Name
          Value: !Ref DomainName
  FoundryDNS:
    Type: AWS::Route53::RecordSet
    Properties: 
      Comment: DNS Entry for Foundry
      HostedZoneId: !Ref HostedZone
      Name: !Ref DomainName
      ResourceRecords: 
        - !GetAtt EC2.PublicIp
      TTL: 300
      Type: A
  SG:
    Type: AWS::EC2::SecurityGroup
    Properties: 
      GroupDescription: public
      GroupName: !Sub public-access-${AWS::StackName}
      SecurityGroupEgress: 
        - CidrIp: "0.0.0.0/0"
          Description: Anonymous access
          IpProtocol: -1
      SecurityGroupIngress: 
        - CidrIp: "0.0.0.0/0"
          Description: Anonymous access
          FromPort: 443
          IpProtocol: TCP
          ToPort: 443
        - CidrIp: "0.0.0.0/0"
          Description: Anonymous access
          FromPort: 80
          IpProtocol: TCP
          ToPort: 80
      VpcId: !Ref VpcId
```
Link to the template: [template.yml](/development/scripted-aws-foundry-setup/template.yml)

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