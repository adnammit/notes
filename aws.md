# Amazon Web Services (AWS)

## Notes on AWS Config for amandaryman.com
* domain is registered with DomainCheap, hosting through AWS
* aws login:
	* use Root user email address to log in
* the simple core website is a static S3 bucket
* your front end apps (cowpoke lounge, upnext) are on amplify and map to subdomains of your site (i.e. upnext.amandaryman.com)
	* these are actually pretty easy to manage/create a new one
	* go to amplify -> create a new app
	* it will walk you through adding a repo/deployment branch
	* once you're done, you can add a custom domain through amplify: Overview > Custom domains > Add domain
* traffic is configured through [Route 53](https://us-east-1.console.aws.amazon.com/route53/v2/hostedzones?region=us-east-1#ListRecordSets/Z021739422TSA26V29ZT2)
* uses CloudFront CDN
* RDS database instances to support your apps


## Reference
* [tutorial](https://docs.aws.amazon.com/AmazonS3/latest/userguide/HostingWebsiteOnS3Setup.html) for configuring a simple static website on S3
* [configuring a website with a custom domain](https://docs.aws.amazon.com/AmazonS3/latest/userguide/website-hosting-custom-domain-walkthrough.html)
* [Cognito Identity Provider](https://docs.aws.amazon.com/cognito/latest/developerguide/what-is-amazon-cognito.html)
* [EventBridge Scheduler](https://docs.aws.amazon.com/eventbridge/latest/userguide/using-eventbridge-scheduler.html)
* [Pricing Calculator](https://calculator.aws/#/)

## What is AWS?
* like azure and google cloud, the AWS ecosystem provides many options for IaaS and PaaS
* Compute (EC2, Lambda, ECS, Batch)
* Storage (S3, EBS, EFS)
* Database (RDS, Redshift)
* Networking (VPC, CloudFront, Route 53)

# AWS Service Overview

## Simple Storage Service (S3)
* object storage service that includes static websites, data lakes, mobile apps, backups

## Elastic Container Services (ECS)
* **Elastic Container Services** are used to run and manage docker containers
* an ECS is a logical grouping of **EC2 compute instances**: the ECS is a service to manage your EC2 virtual instances
* **Lambda** is a serverless compute service that runs code in response to events and automatically manages the compute resources for you: you write code that reacts to events (such as a change in a S3 bucket or DynamoDB table) and AWS runs your Lambda for you. your code can call other Lambdas or call other AWS services.

## Database 
* **Relational Database Service** (RDS) supports many flavors including MySQL, psql, MariaDB, Oracle, and SQL Server
* **ElasticCache** is a managed Redis or Memcached service

## Identity and Access Management (IAM)
* **Cognito** is an identity platform: a user directory that supports authentication, OAuth token services, and provides AWS credentials using built-in user directory, your own enterprise directory, or consumer identity providers like Google and Facebook
* **Certificate Manager** to help manage eminently expiring certs

## Media Services
* AWS includes services to stream video, serve video playback, process video streams, and media transcoding

## Analytics
* Athena: run queries on S3 data
* ETL Services

## EventBridge
* EventBridge is a serverless event bus that makes it easy to connect applications together using data from your own applications, integrated SaaS applications, and AWS services
* there are various products within EventBridge
* **EventBridge**: event bus
* **EventBridge Scheduler**: set cron jobs

## SNS
* [aws sns](https://aws.amazon.com/sns/) is a pub/sub service for A2A (app to app) and A2P (app to person) notifications


## Management Tools
* **Cloud Watch** provides application monitoring and logging
* **Cloud Formation** provides scripted infrastructure deployment
* **Cloud Trail**: logging of everything a user does in AWS, held for one week
* **Code Deploy** automatically deploys your services onto an EC2 or Lambda
* **EBL** Elastic Load Balancer
* **Cloud Front** is a CDN
* **Route 53** is a DNS service


# Concepts

## Availability Zones and Regions
* AWS is divided into 16 **regions**: geographic areas
* AWS has 44 **availability zones** which are data centers within a region that are engineered to be isolated from failures in other zones
* ideally you design your application to be across more than one AZ so if one goes down, you have another 

# EC2

## Options
* **EC2** virtual machines may by purchased with different pricing models:
	* **on-demand** paid by the hour
	* **reserved** for a period of time, instances are always available
	* **scheduled** instances are available at specific times for a contract amount
	* **dedicated**: your very own physical host
	* there are also **spot** instances which are available when the price is below a certain threshold (used for big data computation that is not time-sensitive)
* **EC2 types**: 
	* there are a billion different types of EC2 instances, each with different specs and pricing including dense storage, memory optimized, compute optimized, GPU, etc but some to know:
		* **M4**: general purpose, application servers
		* **T2**: web servers, small databases
	* you can change the type of a EC2 instance if your requirements change
	* the [Compute Optimizer](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-recommendations.html) can show you which type is recommended for your existing workload


## EC2 Configuration
* Load Balancers direct traffic across your instances. all AWS Load Balancers have their own DNS name
* if you have high performance requirements within your ecosystem, you can be methodical in your EC2 placement group -- grouping instances together within a single AZ can enable low-latency, 10Gbps connections between instances

## Simple EC2 Setup
* [set up a node app on EC2](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/setting-up-node-on-ec2-instance.html)
```bash
# after ssh into the instance

# install node
sudo apt-get install curl
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
. ~/.nvm/nvm.sh
nvm install --lts
node -e "console.log('Running Node.js ' + process.version)"

# clone repo - use https link
git clone https://github.com/adnammit/media-service.git
cd {cloned folder}

# install dependencies
npm i
# if you're using typescript you'll need to transpile
npm run build
# run the thing
node dist/index.js

```




# IAM
* There are two general entities you'll need to manage for your services in aws: **roles** and **policies** (also **users**?)
* **roles** are a set of permissions that can be assumed by a user or service
* **policies** may be **inline** (attached to a single user) or **managed** (can be attached to multiple users)
* policies may need to be managed rather than inline in certain conditions, such as a policy that allows a user to assume a role


# RDS

## Management
* when working with relational dbs managed by aws, you can use special `rdsadmin` procs to manage the db such as [bringing it back online](https://stackoverflow.com/a/12200909)



# AWS Development and Infrastructure (CDK, SAM, CloudFormation)
* these are all tools for managing AWS infrastructure as code
* AWS CLI is the command line interface for managing AWS services
* **CDK** is the Cloud Development Kit, a framework for defining cloud infrastructure using programming languages
* **CloudFormation** is a service that allows you to define your AWS infrastructure as code
* **SAM** is the Serverless Application Model, a framework for building serverless applications

## AWS CDK
* using the CDK, you can define your AWS infrastructure using programming languages like TypeScript, Python, Java, and C#, so you can write your code as classes and methods which are used to generate a CloudFormation template - this is typically higher level and easier to use than writing raw CloudFormation templates

## SAM
* [Hello World with SAM](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-getting-started-hello-world.html)
* like the CDK, SAM uses CloudFormation under the hood (a SAM template is very similar to a CloudFormation template), but it is higher level and easier to use


### SAM Setup with .NET
* install .NET 8 SDK and runtime
* [install aws-cli](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)
* [configure aws sso](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-sso.html#cli-configure-sso-configure)
	* running the `aws configure sso` wizard will create a profile in your `~/.aws/config` file ([more about config files](https://docs.aws.amazon.com/cli/v1/userguide/cli-configure-files.html))
    * the credentials requested in the `aws configure sso` wizard can be found in the AWS Access Portal
	* the new profile should be called "root" - this will match the profile value in `aws-lambda-tools-defaults.json`
* [install AWS SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/install-sam-cli.html)
* [install .NET Lambda Global CLI](https://docs.aws.amazon.com/lambda/latest/dg/csharp-package-cli.html) (installing `Amazon.Lambda.Templates` is not needed)
* add the VS Code AWS Toolkit Extension

### Quickstart
```sh
	# login - using profile flag if different from default
	aws sso login --profile root

	# build and deploy with params
	sam build && sam deploy --profile root \ 
        --parameter-overrides DatabaseSecretArn={db_secret_arn}

	# check a secret value
	aws secretsmanager get-secret-value --secret-id {secret_arn}
```

# CodeDeploy
* you can use CodeDeploy to automate deployments into EC2, Lambda, Fargate, or on-prem servers
* for example, you'd set up a EC2 instance with CodeDeploy agent installed

## Install CodeDeploy Agent on Ubuntu
[Documentation](https://docs.aws.amazon.com/codedeploy/latest/userguide/codedeploy-agent-operations-install-ubuntu.html)

```bash
# get ready
sudo apt update
sudo apt install ruby-full
sudo apt install wget
cd /home/ubuntu
# get install script for the appropriate region
wget https://aws-codedeploy-us-east-1.s3.us-east-1.amazonaws.com/latest/install
chmod +x ./install
# install latest version of CodeDeploy
sudo ./install auto
# check service 
sudo service codedeploy-agent status
# it should be running, but if it's not:
sudo service codedeploy-agent start
```
