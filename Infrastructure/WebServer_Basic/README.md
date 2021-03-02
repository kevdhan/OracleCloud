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
###(1a) Goal: Utilize the “Start VCN Wizard” to create a bundled VCN with Internet Connectivity (includes Public/Private Subnet, Internet/NAT/Service Gateway, etc.). This option allows you to create a network with a few clicks versus having to build out everything by scratch.

###(1b) Steps:
![NetworkVCNWizard](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/Network/NetworkVCNWizard.png)

**(1b.1) Create VCN using VCN Wizard**

![NetworkVCNWizard_Preview](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/Network/NetworkVCNWizard_Preview.png)

**(1b.2) Give Basic Information like VCN Name and VCN/Subnet CIDR Blocks. Then click “Create”**

## (2) Setup Subnets + Security Lists + Route Tables
###(2a) Goal: In total we will need **3 Regional Subnets** (2 Public, 1 Private), **3 Security Lists** (2 Public, 1 Private), and **2 Route Tables** (1 Public, 1 Private). 

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

**(2b) Steps:**
1. Create Security List (For Bastion Host Public Subnet):

   ![SecurityList_CreateSecurityList](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/Network/SecurityList_CreateSecurityList.png)
   1. Click Create Security List
   2. Choose a name for the Security List (e.g. “Security List for Public Subnet – Bastion Host”)
   3. Leave rules for Ingress/Egress empty for now
   4. Click Create

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

3. Create Resources in Subnets: Web Server, Database, Load Balancer, Bastion Host
Now that we have created the required network resources, we will start creating our Web Server, Load Balancer, and Bastion Host.

   **Web Server (Under Compute --> Instances)**
   
   ![WebServerVM_Preview](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/WebServer/WebServerVM_Preview.png)
   
   1. Choose a name for the VM Instance
   2. Select the compartment
   3. Select the Availability Domain
   4. Select the Image – for this example, Oracle Linux 7.9
   5. Select the Shape – for this example, VM.Standard.E3.Flex (1 OCPU, 16 GB RAM)
   6. Select Networking – choose the VCN/Private Subnet created for the Web Server
   7. Add SSH Keys – Either create your own private/public SSH Keys and upload them or save the private/public SSH Keys Oracle Cloud generates for you.
viii.	Optional – Add a custom boot volume. For this example, we will leave this blank as this is the first time we are creating this Web Server/VM.
   8. Click Create

   **Load Balancer (Under Networking --> Load Balancer)**
   1. Add Details
   
   ![Image: AddDetails_1]
   
1.	Choose a name for the Load Balancer
2.	Select Public Load Balancer
3.	Select Ephemeral IP Address (Reserved IP Address if you have an existing IP Address)
4.	Selected the Bandwidth (Shape) – for this example, Dynamic Shape: Small / 100 Mbps
[Image: AddDetails_2]
5.	Select Networking – choose the VCN/Public Subnet created for the Load Balancer
ii.	Choose Backends
When adding the Web Server as the backend, the Load Balancer Wizard will automatically add the necessary Security Rules to the Load Balancer Security List and Web Server Security List.
[Image: ChooseBackends]
1.	Select Load Balancing Policy – for this example, Weighted Round Robin
2.	Click Add Backends and select the Web Server VM Instance.
3.	Select Health Check Policy – for this example, keep the default (i.e. HTTP, Port 80, etc.)
iii.	Configure Listener
[Image: ConfigureListener]
1.	Choose a name for the Listener
2.	Select Traffic Type – for this example, HTTP / Port 80 will be used.
3.	Click Submit
c.	Bastion Host – similar process to creating Web Server VM
[Image: BastionHost_VMPreview]
i.	Choose a name for the VM Instance
ii.	Select the compartment
iii.	Select the Availability Domain
iv.	Select the Image – for this example, Oracle Linux 7.9
v.	Select the Shape – for this example, VM.Standard.E2.1.Micro
vi.	Select Networking – choose the VCN/Public Subnet created for the Bastion Host
vii.	Add SSH Keys – Either create your own private/public SSH Keys and upload them or save the private/public SSH Keys Oracle Cloud generates for you.
viii.	Optional – Add a custom boot volume. For this example, we will leave this blank as this is the first time we are creating this Bastion Host.
ix.	Click Create







