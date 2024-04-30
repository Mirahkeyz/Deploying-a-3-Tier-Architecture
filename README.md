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






























































