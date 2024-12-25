# Create-Auto-Scaling-Group-Of-EC2-Instance-For-High-Availability-
"Scaling  The Cloud"

![image alt](https://github.com/Tatenda-Prince/Create-Auto-Scaling-Group-Of-EC2-Instance-For-High-Availability-/blob/fae73dd8e74ecebd45353dee9a642f4fc6a551b9/Images/ASG%20image.png)

# Intro 

Together, we will “Scale the Clouds” by following the steps to creating an Auto Scaling Group (ASG) of Amazon EC2 Instances for high availability. We will literally be building a cloud infrastructure from ground up, so expect the use of many different AWS resources.

# Background

# Virtual Private Cloud (VPC)

Virtual Private Cloud is a service on AWS that offers a virtual private network, logically isolated from other virtual networks on the platform.

# Subnet

Subnets are a set of IP address ranges in your VPC.

# Internet Gateway

Internet Gateways enable AWS resources in public subnets with public IP addresses, to be able to connect to the internet.

# Elastic Load Balancers

Elastic Load Balancers automatically distribute incoming traffic across multiple targets such as Amazon EC2 Instances.

# Route Table

A Route Table contains a set of rules, called routes, that determine the direction of network traffic from subnets or an internet gateway.

# Launch Template

Launch Templates contain pre-configuration information used to launch resources such as Amazon EC2 Instance.

# High Availability

High availability refers to the elimination of single points of failure to enable infrastructures to continue operating, even if a component it depends on fails.

# Use Case

You are a Cloud Engineer for UP THE CHELS TECH, tasked to build a highly available Apache Web Servers cloud infrastructure. The infrastructure needs to have a minimum of 2 servers running at all times. Additionally, it must have the capability to scale out and increase to a maximum of 5 servers when the average CPU utilization increases to 50%.


# Step 1: Setup VPC and subnets

# Create VPC

Navigate to VPC dashboard and click “Create VPC”. Continue by naming your VPC, then add the IPv4 CIDR block. We will be using 10.10.0.0/16, as seen below. You can now “Create VPC”.


![image alt](https://github.com/Tatenda-Prince/Create-Auto-Scaling-Group-Of-EC2-Instance-For-High-Availability-/blob/b43a747ce06afa9c6b11029ad65a6ef82cc5c0bc/Images/Screenshot%202024-12-23%20200843.png)


# Create Subnets

Navigate to “Subnets” in the left pane of the VPC dashboard. Click “Create subnets”, then select our newly created VPC from the list. Name your subnet, then choose the Availability Zone (AZ) and assigned IPv4 CIDR block. For the first subnet, we will use AZ us-east-1a and CIDR block 10.10.1.0/24. For the second subnet, use AZ us-east-1b and CIDR block 10.10.2.0/24. Lastly, for subnet three, use AZ us-east-1c and CIDR block 10.10.3.0/24.


![image](https://github.com/Tatenda-Prince/Create-Auto-Scaling-Group-Of-EC2-Instance-For-High-Availability-/blob/6243431bea3fed2b535b5b5eff8e88eebb5618af/Images/Screenshot%202024-12-23%20201543.png)


After configuring all three subnets, click “Create subnets”. You should be able to see all 3 subnets created and “Available”, as seen below.

![image alt](https://github.com/Tatenda-Prince/Create-Auto-Scaling-Group-Of-EC2-Instance-For-High-Availability-/blob/642d84c40f05590e01105566699d5131a4316cb3/Images/Screenshot%202024-12-25%20114630.png)


We need to enable each subnet to automatically receive a public IP address so we can send and receive traffic over the internet. To accomplish this, select one of your subnets, then navigate to “Actions” and click “Edit subnet settings”. Enable “Auto-assign public address”, then save the changes. Make sure to do this with all 3 subnets.


![image alt](https://github.com/Tatenda-Prince/Create-Auto-Scaling-Group-Of-EC2-Instance-For-High-Availability-/blob/a2d8ef6c1a8f7ccfbac713d1e39a8df5b2eb6e34/Images/Screenshot%202024-12-23%20201743.png)


Now that we’ve created and configured our VPC and 3 subnets, we can proceed to Step 2 — Internet Gateway and Route Tables!


# Step 2: launch Internet gateway and configure route tables

# Create Internet Gateway and attach to VPC

Navigate to your VPC dashboard, select “Internet Gateway”, then “Create a new gateway”. Name the Internet Gateway and proceed to “Create internet gateway”.


![image alt](https://github.com/Tatenda-Prince/Create-Auto-Scaling-Group-Of-EC2-Instance-For-High-Availability-/blob/853f8f7d1d5799cfe79a0a6dd0603962bee424c1/Images/Screenshot%202024-12-23%20201923.png)

On the Internet Gateway dashboard, select your Internet Gateway, click “Actions”, then “Attach to VPC”. Select your VPC, then attach the Internet Gateway, a shown below.

![image alt](https://github.com/Tatenda-Prince/Create-Auto-Scaling-Group-Of-EC2-Instance-For-High-Availability-/blob/ec11836c0a66e08fbb133760cf04f6dd2d3ba229/Images/Screenshot%202024-12-23%20202017.png)

Your Internet Gateway’s state should now read “Attached” with the VPC ID of our previously created VPC, as shown below.


![image alt](https://github.com/Tatenda-Prince/Create-Auto-Scaling-Group-Of-EC2-Instance-For-High-Availability-/blob/79a8408f3eb29870dee6d72f7078b8637cfb84ee/Images/Screenshot%202024-12-23%20202056.png)


# Configure route tables

Navigate to “Route tables” and “Create route table”. Name the route table, choose the VPC, then “Create route table”.

![image alt](https://github.com/Tatenda-Prince/Create-Auto-Scaling-Group-Of-EC2-Instance-For-High-Availability-/blob/436d24d44ebaa792a731527c8f3c83bbc6065437/Images/Screenshot%202024-12-23%20202257.png)

In your Route table dashboard, select the “Subnet associations” tab, then edit the explicit subnet associations —


![image alt](https://github.com/Tatenda-Prince/Create-Auto-Scaling-Group-Of-EC2-Instance-For-High-Availability-/blob/e45bb2491c2c2c3013718714e9ec8f8c2bbf2b9b/Images/Screenshot%202024-12-25%20120646.png)

Add all three subnets to the associations, then save the associations.


![image alt](https://github.com/Tatenda-Prince/Create-Auto-Scaling-Group-Of-EC2-Instance-For-High-Availability-/blob/c8f38c00c1927864a472e9eb75cb8a8985fe0ae6/Images/Screenshot%202024-12-23%20202535.png)

Let’s now edit the route table to have access to the internet though our Internet Gateway. Click on the “Routes” tab, “Edit routes”, then “Add route”. We will add a route that will direct all other addresses destined outside our network, to our newly created Internet Gateway through to the internet, as seen below.


![image alt](https://github.com/Tatenda-Prince/Create-Auto-Scaling-Group-Of-EC2-Instance-For-High-Availability-/blob/b72415f2f717ccfc16ee3f7a5d537f4cb67cf43b/Images/Screenshot%202024-12-23%20202753.png)

We now have an Internet Gateway and a configured Route Table. Let’s proceed to Step 3— Application Load Balancer!

# Step 3: Launch a configure application load balancer

# Configure network mappings

Head over to your EC2 dashboard, navigate to the left side, then scroll down. Select “Load Balancers”, then “Create load balancer”.

We will choose to create an Application Load Balancer. Name the Load Balancer and keep the rest of the default basic configurations. Continue on to Network mappings, choose your VPC then select the three AZ’s with your three subnets, as shown below.

![image alt](https://github.com/Tatenda-Prince/Create-Auto-Scaling-Group-Of-EC2-Instance-For-High-Availability-/blob/e869c452c76452ec12f750c531d839333bd9e260/Images/Screenshot%202024-12-23%20203259.png)


# Create security group

Proceed to the Security group setting, then create a new security group. Name your security group and make sure your previously created VPC is selected. Add an inbound rule to allow HTTP traffic from Anywhere (0.0.0.0/0), as seem below.


![image alt](https://github.com/Tatenda-Prince/Create-Auto-Scaling-Group-Of-EC2-Instance-For-High-Availability-/blob/6140669c31f0c035049ba36d481af88257369a2e/Images/Screenshot%202024-12-23%20203511.png)



![image alt](https://github.com/Tatenda-Prince/Create-Auto-Scaling-Group-Of-EC2-Instance-For-High-Availability-/blob/efa9f696cd124e85897b2fc6dcb039ba674006ab/Images/Screenshot%202024-12-23%20203543.png)


Additionally, add another rule of type “SSH” with source from “Anywhere”. Note — this is security risk, however for this article we will allow it for demonstration sake.

We can now proceed to “Create security group”.

Select the newly created security group from the list, as seen below.

![image alt](https://github.com/Tatenda-Prince/Create-Auto-Scaling-Group-Of-EC2-Instance-For-High-Availability-/blob/a872abdb4b85d954aa708978a0bae98ab7b918b7/Images/Screenshot%202024-12-23%20203818.png)


Continue to “Listeners and routing”. Make sure the listener Protocol is set to HTTP and the Port to 80. We will now “Create target group” to be configured for our EC2 Instances. Choose “Instances” for a target type and name your target group. Choose HTTP for the protocol and 80 for the Port. Select our new VPC, click “Next”, then “Create target group”.


![image alt](https://github.com/Tatenda-Prince/Create-Auto-Scaling-Group-Of-EC2-Instance-For-High-Availability-/blob/c63b3fe385d5b29bbf97178cf10dc8dedc4aac78/Images/Screenshot%202024-12-23%20204015.png)


We can now select our newly created target group, as shown below.


![image alt](https://github.com/Tatenda-Prince/Create-Auto-Scaling-Group-Of-EC2-Instance-For-High-Availability-/blob/a56947144f42821c6da51ac076d1d2c8bcbddd0f/Images/Screenshot%202024-12-23%20204101.png)

Scroll to the bottom, review the summary then “Create load balancer”. Now let’s continue to Step 4 — Launch Template!


# Step 4: Create Launch Template

# Configure Launch Template 

Navigate to your EC2 dashboard, on the left, click “Launch Templates”, then “Create launch template”.

Name the launch template, select the Amazon Linux AMI that’s part of AWS free tier by clicking the “Quick Start” tab, then choosing the AMI. Choose the “t2.micro” instance type, as shown below.


![image alt](https://github.com/Tatenda-Prince/Create-Auto-Scaling-Group-Of-EC2-Instance-For-High-Availability-/blob/66f71dc78fc9f2f8619ea61fc8234987bd843f9c/Images/Screenshot%202024-12-23%20204427.png)


![image alt](https://github.com/Tatenda-Prince/Create-Auto-Scaling-Group-Of-EC2-Instance-For-High-Availability-/blob/dc895e911c63bbeafe6793da9804df4e525b7a1d/Images/Screenshot%202024-12-23%20204442.png)


Proceed to create a new key pair to enable you to authenticate via SSH into our EC2 Instances. Save the key pair “.pm” file to your local machine in a safe location. Remember, you can also use a previously created key pair.

Continue to “Network settings” and select our previously created load balancer security group, as shown below.

![image alt](https://github.com/Tatenda-Prince/Create-Auto-Scaling-Group-Of-EC2-Instance-For-High-Availability-/blob/7765a8ba39ba9bfd00e2c0d999dbb5eea59898d6/Images/Screenshot%202024-12-23%20204529.png)


# Bootstrap EC2 Instance to launch with Apache Web Server

Continue to the “Advanced details”. Expand the settings, then scroll down to add the user data to install, start and enable the Apache Web Server. We will also install other software to enable us to stress test an EC2 Instance.

Copy the bash script below, paste it in the user data field, then “create launch template” —

#!/bin/bash

#Update all yum package repositories
yum update -y

#Install Apache Web Server
yum install -y httpd.x86_64

#Start and Enable Apache Web Server
systemctl start httpd.service
systemctl enable httpd.service

#Install epel for easy access to install packages for commonly used software
amazon-linux-extras install epel -y

#Install stress to enable us to test EC2 Instance under stress work loads
yum install stress -y



# Step 5: Create an Autoscaling Group (ASG)

# Configure new ASG launch options

After creating the launch template, in the following window, click “Create Auto Scaling group”, as show below. Scroll down, select “Auto scaling groups” in the left pane, then “Create auto scaling group”.

![image alt](https://github.com/Tatenda-Prince/Create-Auto-Scaling-Group-Of-EC2-Instance-For-High-Availability-/blob/3a21a3e480bc65fc1af5367898fc11a619c7dd73/Images/Screenshot%202024-12-23%20204656.png) 


Name the Auto scaling group, select the launch template previously created, then click “Next”.

Select our previously created VPC, add the three of our subnets, as seen below, then click “Next”.


![image alt](https://github.com/Tatenda-Prince/Create-Auto-Scaling-Group-Of-EC2-Instance-For-High-Availability-/blob/73ead94ea986f79696511127a0f825d58c6df5dd/Images/Screenshot%202024-12-23%20205105.png) 


# Configure load balancing options

Select “Attached to and exiting load balancer”, then choose our previously created load balancer’s target group, as shown below.

![imagee alt](https://github.com/Tatenda-Prince/Create-Auto-Scaling-Group-Of-EC2-Instance-For-High-Availability-/blob/e35eeec0ee7d7a624d15ba1a47a02401bab9e0b2/Images/Screenshot%202024-12-23%20205208.png)

Proceed to health checks and change the grace period to 120 seconds (2 minutes), as seen below. With health checks enabled, the load balancer is able to periodically send request to check the status of our EC2 Instances and determine whether an instance is capable of performing work successfully.

In additional settings, “enable group metric collection within CloudWatch”.


![image alt](https://github.com/Tatenda-Prince/Create-Auto-Scaling-Group-Of-EC2-Instance-For-High-Availability-/blob/032f5b00360ca4b14dfc81bbd949fff1969b6823/Images/Screenshot%202024-12-23%20205301.png) 


# Configure ASG Group size and CloudWatch Monitoring

As our use case states, we will set our desired and minimum capacity to 2. Our maximum capacity will be set to 5. Select “Target scaling policy” and make sure the metric type is “Average CPU utilization” and “Target value” is set to 50, then click “Next”.


![image alt](https://github.com/Tatenda-Prince/Create-Auto-Scaling-Group-Of-EC2-Instance-For-High-Availability-/blob/cd16153029c5507ba7d31263730ce842e9d3e1c4/Images/Screenshot%202024-12-23%20205408.png)



![image alt](https://github.com/Tatenda-Prince/Create-Auto-Scaling-Group-Of-EC2-Instance-For-High-Availability-/blob/7d8a2ee44fcd6a8f052484208587ce054e3b684e/Images/Screenshot%202024-12-23%20205430.png) 



Continue until you get to the review page. Review the final configurations, then “Create auto scaling group”. You should be able to see your ASG with a status of “updating capacity..” as it launches your EC2 Instances according to the pre-set configurations.

Navigate to your EC2 dashboard and verify that there are two running EC2 Instances created by your ASG.

![image alt](https://github.com/Tatenda-Prince/Create-Auto-Scaling-Group-Of-EC2-Instance-For-High-Availability-/blob/cd4533ed1b69656ea0e4640abe1a128ca35588a5/Images/Screenshot%202024-12-23%20205547.png)


# Step 6: Connect to Servers running Apache Web Server

Retrieve the public IP Address of each of the EC2 Instance from the Amazon EC2 dashboard “Networking” tab, copy and paste it in the address bar of your preferred browser. Your browser should display the Apache Web Server default Webpage, as seen below.


![image alt](https://github.com/Tatenda-Prince/Create-Auto-Scaling-Group-Of-EC2-Instance-For-High-Availability-/blob/7dcae7bfeeeefc54eba7893ffcd0183c4181d577/Images/Screenshot%202024-12-23%20205747.png)


You should also be able to connect to one of your instances through the application load balancer’s domain name. You can retrieve this by navigating to your previously created load balancer, copying the DNS name, then pasting it in your browser’s address bar, as seen below.


![image alt]()


# YAY Congratulations!

You’ve just successfully completed “Scaling The Clouds”. You’ve created an Auto Scaling Group of EC2 Web Servers for high availability!










































