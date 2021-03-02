# Web Server Basic Tutorial

**This tutorial will follow/touch on these other tutorials/documentations:**
* [Free Tier: Install Apache and PHP on an Oracle Linux Instance](https://docs.oracle.com/en-us/iaas/developer-tutorials/tutorials/apache-on-oracle-linux/01-summary.htm)
* [Getting Started with Load Balancing](https://docs.oracle.com/en-us/iaas/Content/GSG/Tasks/loadbalancing.htm#Getting_Started_with_Load_Balancing)
* [Bastion Host White Paper](https://docs.oracle.com/en-us/iaas/Content/Resources/Assets/whitepapers/bastion-hosts.pdf)
* [Connecting to Your Linux Instance using SSH](https://docs.oracle.com/en-us/iaas/Content/GSG/Tasks/testingconnection.htm)
* [Connecting to a Database System (DBCS)](https://docs.oracle.com/en-us/iaas/Content/Database/Tasks/connectingDB.htm)

**Background:**
In this tutorial, we will create a basic web server using Apache and PHP on an Oracle Linux VM Instance.

The Web Server will be hosted in a private subnet to provide best security. A Load Balancer will be created in a public subnet, giving clients a way to access the Web Server and providing best practice, security, and HA for the Web Server. A Bastion Host (aka Proxy Server) in a public subnet will be used to give Admin(s) and Developer(s) access to the Web Server.

It will go over the following topics/services:
* Networking
* Computes (Virtual Machines)
* Load Balancing

High Level Architecture of Tutorial:
Coming soon… 

# Steps...
## (1) Create Virtual Cloud Network
**Goal:** Utilize the “Start VCN Wizard” to create a bundled VCN with Internet Connectivity (includes Public/Private Subnet, Internet/NAT/Service Gateway, etc.). This option allows you to create a network with a few clicks versus having to build out everything by scratch.

**Steps:**
![NetworkVCNWizard](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/Network/NetworkVCNWizard.png)

1. Create VCN using VCN Wizard

![NetworkVCNWizard_Preview](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/Network/NetworkVCNWizard_Preview.png)

2. Give Basic Information like VCN Name and VCN/Subnet CIDR Blocks. Then click “Create”

## (2) Setup Subnets + Security Lists + Route Tables
**Goal:** In total we will need **3 Regional Subnets** (2 Public, 1 Private), **3 Security Lists** (2 Public, 1 Private), and **2 Route Tables** (1 Public, 1 Private). 

By Default, the VCN Wizard creates 1 Public and 1 Private Subnet, each with its own Security List and Route Table. Because the VCN Wizard creates 1 Public and Private Subnet and Security List, we will only need to create 1 additional Public Subnet and Security List.

What we will do first (in this step), is create the skeleton. For example, we will create the security list, but not add security rules yet. We will come back to Security Lists and add the appropriate Security Rules once we’ve added our resources (i.e. Load Balancer, Web Server, Bastion Host) to our subnets.

By default, the VCN Wizard creates 1 Private and 1 Public Route Table. The Private Route Table allows use of the NAT Gateway and Service Gateway, and the Public Route Table allows use of the Internet Gateway. For this tutorial, these route tables and rules are fine and so we will not create new ones.

Overall Subnet + Security List + Route Table List:
![Subnets_OverallView](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/Network/Subnets_OverallView.png)

![SecurityListOverview](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/Network/SecurityListOverview.png)

![Image: RouteTables](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/Network/RouteTableOverview.png)

* 2 Public Subnets:
   * Default Public Subnet + Default Public Security List + Default Public Route Table for LOAD BALANCER
   * Create New Public Subnet + New Public Security List + Default Public Route Table for BASTION HOST
* 1 Private Subnets:
   * Default Private Subnet + Default Private Security List + Default Private Route Table for WEB SERVER

**Steps:**
1. Create Security List (For Bastion Host Public Subnet):

   ![SecurityList_CreateSecurityList](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/Network/SecurityList_CreateSecurityList.png)

   1. i.	Click Create Security List
   2. ii.	Choose a name for the Security List (e.g. “Security List for Public Subnet – Bastion Host”)
   3. iii.	Leave rules for Ingress/Egress empty for now
   4. iv.	Click Create
2. Create Public Subnet (For Bastion Host):
   ![Subnets_CreateSubnet](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/Network/Subnets_CreateSubnet.png)
   1. Click Create Subnet
   ![Subnets_CreateSubnet2](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/Network/Subnets_CreateSubnet2.png)
   2. Choose a name
   3. Choose the compartment
   4. Select Subnet Type: Regional
   5. Choose the CIDR Block (Make sure its within your VCN CIDR Block and NOT overlapping with other subnets)
   6. Select Corresponding Route Table (Default Public Route Table if Public Subnet)
   7. Select Public Subnet
   ![Subnets_CreateSubnet3](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/Network/Subnets_CreateSubnet3.png)
   8. Leave default DNS Options
   9. Select Default DHCP Options
   10. Select Corresponding Security List (Select newly created Security List)
   11. Click Create

3. 






