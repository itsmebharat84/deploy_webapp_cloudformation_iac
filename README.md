# Overview
Deploy a High-Availability Web App using CloudFormation. 

# Problem Statement
Provision infrastructure in AWS using cloudformation to deploy Web Application named Udagram. 
Ensure IAC to be fully automated so that infrstructure can be discarded post validation. 

# Network Solution Design 

The below network design depicts all the required infrastructure and their connectivity with each component. 
Application will be hosted inside the EC2 instance that is managed through ASG Launch Configuration. 


![img.png](img.png)

# Pre-requisites 
1. AWS Account
2. Create S3 Bucket, to host the application code e.g. HTML
3. Copy index.html file inside the s3 bucket. 
4. Key-Pair, to connect to Ec2 instances through Bastion Host  (OPTIONAL)

# Deployment Process 
The application infrastructure consists of deploying following stacks:
1. Network Stack (Required)
   This includes one VPC, four subnets ( 2 public and 2 private) in each zone, an internet Gateway NAT Gateways and Routing tables and their associations with public and private subnets.  
2. AppServer Stack  (Required)
   This includes ASG, Load Balancers, Security Groups, launch configurations, Instance profile and Target Group. 

# Create Infrastructure 
To create the infrastructure stack run the following commands in the same order as below:

      ./create-stack.sh network network.yaml network-parameters.json
      ./create-stack.sh appserver server.yml server-parameters.json  

# Validate deployment
In order to validate web application is running, navigate to server stack in cloud formation. Under the Outputs section you will fine the DNS name or Public accesible URL (WebAppLBDNSName).

![](screenshots/App Running Screenshot.png)
# Update infrastructure
To update the already existing infrastructure stack run one (or all) the following commands:

      ./update-stack.sh network network.yaml network-parameters.json 
      ./update-stack.sh appserver server.yml server-parameters.json  

# (Optional) Bastion Host Along with Application Server 
   In addition to the server stack, I have added code for bastion host. So this will build a bastion host along with server stack resources. The purpose of this bastion host is to connect to instances hosted in private subnet.
   Please note, you need to execute only one script either sever script or server plus bastion script.
   ### Create Stack 
    ./create-stack.sh appserver appserver_plus_bastion.yml appserver_plus_bastion.json 
   ### Update Stack
    ./update-stack.sh appserver appserver_plus_bastion.yml appserver_plus_bastion.json 


# Destroy infrastructure
To delete the infrastructure stack run the following commands in the same order as below:

      ./delete-stack.sh appserver
      ./delete-stack.sh network