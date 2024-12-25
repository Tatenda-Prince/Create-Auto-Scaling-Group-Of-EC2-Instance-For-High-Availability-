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


![image]()






