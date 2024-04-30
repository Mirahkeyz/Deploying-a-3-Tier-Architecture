# Deploying-a-3-Tier-Architecture

Scenario: A small e-commerce company wants a scalable and reliable e-commerce platform and decide to deploy a 3-tier architecture at the advice of their engineers. The web tier would be responsible for serving the static content to end-users and handling incoming requests. By using an Auto Scaling Group, the platform can handle sudden spikes in traffic without any manual intervention. The public subnets would allow incoming traffic from the internet to reach the EC2 instances running in the Auto Scaling Group. The application tier would handle the business logic of the platform. This tier would be responsible for processing transactions, handling payments, and interacting with the database. By placing the EC2 instances in private subnets and only allowing inbound traffic from the Web Server Security Group, the application tier can be protected from direct access from the internet. The database tier would store all of the platform’s data, including customer information, product data, and transaction records.

Project Overview: This project has 3 tiers of difficulty. Foundational, Advanced and Complex. I will attempting all 3 tiers and documenting the steps to accomplish them.

Before starting, I mapped out all of the IP’s for the VPC:

VPC CIDR: 10.0.0.0/16

Web Tier (2) Public IP’s: 10.0.1.0/24 & 10.0.2.0/24

Application Tier: (2) Private IP’s: 10.0.3.0/24 & 10.0.4.0/24

Data Tier: (2) Private IP’s: 10.0.5.0/24 & 10.0.6.0/24

# TIER 1: FOUNDATIONAL

Web Tier:

— 2 public subnets

— Minimum of 2 EC2 instances with an OS of your choice (free tier) in an Auto Scaling Group.

— EC2 Web Server Security Group allowing inbound permission from the internet.

— Boot strap static web page or create a custom AMI that already includes the static web page.

— Create a public route table and associate the 2 public subnets.

The first thing we need to do is create a new VPC. I created a new default one that has an IPV4 CIDR of 10.0.0.0/16. Let’s create our 2 Public IP’s. From the VPC console, click on Subnets, Create subnet and choose the VPC.

![Snipe 1](https://github.com/Mirahkeyz/Deploying-a-3-Tier-Architecture/assets/134533695/5c8869c3-aaba-4365-a06e-36058b04160c)

Both Public IP’s have been created. We need to click on the checkbox of each one individually then click on Actions, Edit subnet settings and Enabling auto-assign public IPv4 address then save.

![Snipe 2](https://github.com/Mirahkeyz/Deploying-a-3-Tier-Architecture/assets/134533695/3bbe3954-7439-4dc0-be55-5451677dcec9)

# Launch an Instance

Navigate to the EC2 console, click “Instances,” then “Launch instances.” Give the instance a name, select an AMI, select an instance type, and key pair.

![Snipe 3](https://github.com/Mirahkeyz/Deploying-a-3-Tier-Architecture/assets/134533695/717337f4-796b-42c4-8edf-3b7e55c5b098)

Under the Network settings you will see that the default VPC is selected. Click “Edit” and select the VPC you just created. Then select one of the public subnets you just created. For “Auto-assign public IP,” choose “Enable.”

![Snipe 4](https://github.com/Mirahkeyz/Deploying-a-3-Tier-Architecture/assets/134533695/37083359-2f73-4c74-8150-a988510e8479)

Under the Firewall settings we want to create a web tier security group that allows inbound internet traffic. Select “Create security group,” give it a name and description, and the select “HTTP” for type and “Anywhere” for source type. This will allow all inbound traffic on port 80. I also like to allow HTTPS (port 443) and SSH (port 22).

![Snipe 5](https://github.com/Mirahkeyz/Deploying-a-3-Tier-Architecture/assets/134533695/17d25986-c884-46ed-b06a-2eeda2e2aaff)

Then put all the details of your website inside this EC2 instance created i.e deploying any static website. Then we can create a new AMI on this EC2 instance.

# Create a Launch Template and Auto Scaling Group

The next step is to launch EC2 instance using an Auto Scaling Group. Click on Create auto scaling group. I will be calling mine Web Tier ASG. before we can proceed we will need to create the launch template.

![Snipe 6](https://github.com/Mirahkeyz/Deploying-a-3-Tier-Architecture/assets/134533695/9b6cd6cc-a171-45db-9c57-485ee18f4beb)

For the OS I will be using my custom Amazon Linux AMI which already has a website baked into it. Choose the instance type and key pair. Create a new web security group that allows SSH and HTTP access.

In Advanced network configuration, Enable Auto-assign public IP. Now click on create launch template.

Head back to ASG and select the LT we just created and click Next. In the next screen choose both public subnets under Availability Zones and subnets and click Next.

We will not be adding a load balancer just yet. Leave everything default and click Next. For Desired capacity, Min and Max, I will set at 0 for now. For Scaling policies select Target tracking scaling policy and leave everything else default and click Next. Continue to click next until you can create the auto scaling group.

The last step in the web tier is to create the public route table and associate the 2 public subnets.

Head over to Route table, Create route table. I will be naming it Web Tier RT 1. Choose the VPC and click Create route table.

After it is created click on the Subnet associations tab, Edit subnet associations, select both subnets then click on Save associations.

![Snipe 7](https://github.com/Mirahkeyz/Deploying-a-3-Tier-Architecture/assets/134533695/52e149e3-d256-427f-b0e4-67ec1339c4e6)

We finished with the Web Tier for now. Lets continue on to the Application Tier.

Application Tier:

— 2 private subnets

— Minimum of 2 EC2 instances with an OS of your choice (free tier) in an Auto Scaling Group.

— EC2 Application Server Security Group allowing inbound permission from the Web Server Security Group.

— Associate with private route table.

Let’s start by creating the 2 private subnets. I will be naming them Private App Tier 1 (

![Snipe 8](https://github.com/Mirahkeyz/Deploying-a-3-Tier-Architecture/assets/134533695/7c2973d8-ec20-4a2b-8441-ddc24f286de4)

Next create the private route table and associate the 2 private subnets.

![Snipe 9](https://github.com/Mirahkeyz/Deploying-a-3-Tier-Architecture/assets/134533695/5213c6e2-7311-4e14-9913-5cb5d1a00373)

Let’s create the App Tier security group. It needs to allow SSH & ICMP access from the Web Tier only.

![Snipe 10](https://github.com/Mirahkeyz/Deploying-a-3-Tier-Architecture/assets/134533695/13f3cff0-eb64-42ff-93cb-e19f63a03d34)

Head over to auto scaling groups to create one for the App Tier.







































































