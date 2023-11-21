# Project-02-Deploy-a-high-availability-web-app-using-CloudFormation
Project 02 - Deploy a high-availability web app using CloudFormation

## Project Requirements
Each of these categories details the specific requirements for each component of the project. Complete the checklist at the bottom of the page to ensure your project is ready for submission.

### Infrastructure Diagram
You'll need to create an infrastructure diagram in a tool of your choice, with all the AWS resources you need for your solution. Resources we expect to see in the diagram:
Network resources: VPC, subnets, Internet Gateway, NAT Gateways
EC2 resources: Autoscaling group with EC2 instances, Load Balancer, Security Groups
Static Content: S3 bucket.
Use arrows to include any relevant relations between resources.
Feel free to omit name labels for any resource icons you deem appropriate, as long as the diagram is still clear and unambiguous.

### Network and Servers Configuration
You can deploy to any region.
You'll need to create the networking infrastructure for your solution, including a new VPC and four subnets: two public and two private, following high availability best practices.
Use a parameters JSON file to pass CIDR blocks for your VPC and subnets.
You'll need to attach Internet and NAT gateways for internet access.
You'll need to use Launch Templates to create an Autoscaling Group for your application servers in order to deploy four servers, two located in each of your private subnets.
Your CPU and RAM requirements will be covered with t2.micro instances, so use this instance type. The Operating System to be used is Ubuntu 22.
The application must be exposed to the internet using an Application Load Balancer.

###  Static Content
You'll need to create an S3 bucket with CloudFormation to store all static content. This bucket should have public-read access.
Your servers IAM Role should provide read and write permissions to this bucket.

###  Security Groups
Udagram communicates on the default HTTP Port: 80, so your servers will need this inbound port open since you will use it with the Load Balancer and the Load Balancer Health Check. As for outbound, the servers will need unrestricted internet access to be able to download and update their software.
The load balancer should allow all public traffic (0.0.0.0/0) on port 80 inbound, which is the default HTTP port.

###  CloudFormation Templates
Considering that a network team will be in charge of the networking resources, you'll need to deliver two separate templates: one for networking resources and another one for your application specific resources (servers, load balancer, bucket).
Your application template should use outputs from your networking template to identify the hosting VPC and subnets.
One of the output exports of the CloudFormation application stack should be the public URL of the LoadBalancer. Bonus points if you add http:// in front of the load balancer DNS Name in the output, for convenience.
You should be able to create and destroy the entire infrastructure using scripts (no UI interactions). You can use any language you like (bash or python, for example), but you must be using the CloudFormation CLI or libraries built on top of it (boto3, for example).

###  BONUS (Optional features)
Create a Cloudfront distribution to serve your static content.
Set up a bastion host (jump box) to allow you to SSH into your private subnet servers. This bastion host would be on a Public Subnet with port 22 open only to your home IP address, and it would need to have the private key that you use to access the other servers.

## Project setup: 

1) Install AWS CLI
2) Create an IAM user with Administrator permissions
3) Configure the AWS CLI

aws configure 

Login to AWS by AWS CLI. 

Step 1: 

- create network

aws cloudformation create-stack  --stack-name tutai92-udagram-network  --template-body file://network.yml  --parameters file://network-parameters.json --capabilities "CAPABILITY_NAMED_IAM" --region us-east-1 

- create stack S3

aws cloudformation create-stack  --stack-name tutai92-udagram-s3  --template-body file://s3.yml  --parameters file://s3-parameters.json --capabilities "CAPABILITY_NAMED_IAM" --region us-east-1 

- create stack IAM

aws cloudformation create-stack  --stack-name tutai92-udagram-iam  --template-body file://iam.yml  --parameters file://iam-parameters.json --capabilities "CAPABILITY_NAMED_IAM" --region us-east-1 

- create servers

aws cloudformation create-stack  --stack-name tutai92-udagram-servers  --template-body file://servers.yml  --parameters file://servers-parameters.json --capabilities "CAPABILITY_NAMED_IAM" --region us-east-1 

aws cloudformation update-stack  --stack-name tutai92-udagram-servers  --template-body file://servers.yml  --parameters file://servers-parameters.json --capabilities "CAPABILITY_NAMED_IAM" --region us-east-1 

Completed setup network, S3, servers and completed required of Project 02. 

Step 2: Delete stacks:

- Delete network: 

aws cloudformation delete-stack --stack-name tutai92-udagram-network --region us-east-1 

- Delete S3 (mark sure empty all files in S3)

aws cloudformation delete-stack --stack-name tutai92-udagram-s3 --region us-east-1 

- Delete IAM

aws cloudformation delete-stack --stack-name tutai92-udagram-iam --region us-east-1 

- Delete servers configuration

aws cloudformation delete-stack --stack-name tutai92-udagram-servers --region us-east-1 


## Results: 

1) Picture folders

2) URL: http://tutai92-project02-udagram-alb-648604683.us-east-1.elb.amazonaws.com/index.html