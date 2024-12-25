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


![image alt]()

















