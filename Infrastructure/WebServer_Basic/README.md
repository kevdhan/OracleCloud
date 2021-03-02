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
Utilize the “Start VCN Wizard” to create a bundled VCN with Internet Connectivity (includes Public/Private Subnet, Internet/NAT/Service Gateway, etc.). This option allows you to create a network with a few clicks versus having to build out everything by scratch.
![NetworkVCNWizard](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/Network/NetworkVCNWizard.png)
1. Create VCN using VCN Wizard

![NetworkVCNWizard_Preview](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/Network/NetworkVCNWizard_Preview.png)
2. Give Basic Information like VCN Name and VCN/Subnet CIDR Blocks. Then click “Create”

## (2) Setup Subnets + Security Lists + Route Tables


