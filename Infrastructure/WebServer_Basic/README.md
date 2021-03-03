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
### (1a) Goal: 
Utilize the “Start VCN Wizard” to create a bundled VCN with Internet Connectivity (includes Public/Private Subnet, Internet/NAT/Service Gateway, etc.). This option allows you to create a network with a few clicks versus having to build out everything by scratch.

### (1b) Steps:
![NetworkVCNWizard](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/Network/NetworkVCNWizard.png)

**(1b.1) Create VCN using VCN Wizard**

![NetworkVCNWizard_Preview](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/Network/NetworkVCNWizard_Preview.png)

**(1b.2) Give Basic Information like VCN Name and VCN/Subnet CIDR Blocks. Then click “Create”**

## (2) Setup Subnets + Security Lists + Route Tables
### (2a) Goal: 
In total we will need **3 Regional Subnets** (2 Public, 1 Private), **3 Security Lists** (2 Public, 1 Private), and **2 Route Tables** (1 Public, 1 Private). 

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

### (2b) Steps:
**(2b.1 Create Security List (For Bastion Host Public Subnet):**

![SecurityList_CreateSecurityList](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/Network/SecurityList_CreateSecurityList.png)

1. Click Create Security List
2. Choose a name for the Security List (e.g. “Security List for Public Subnet – Bastion Host”)
3. Leave rules for Ingress/Egress empty for now
4. Click Create

**(2b.2) Create Public Subnet (For Bastion Host):**

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

**(2b.3) Create Resources in Subnets: Web Server, Database, Load Balancer, Bastion Host
Now that we have created the required network resources, we will start creating our Web Server, Load Balancer, and Bastion Host.**

1. Web Server (Under Compute --> Instances)
   
   ![WebServerVM_Preview](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/WebServer/WebServerVM_Preview.png)
   
   1. Choose a name for the VM Instance
   2. Select the compartment
   3. Select the Availability Domain
   4. Select the Image – for this example, Oracle Linux 7.9
   5. Select the Shape – for this example, VM.Standard.E3.Flex (1 OCPU, 16 GB RAM)
   6. Select Networking – choose the VCN/Private Subnet created for the Web Server
   7. Add SSH Keys – Either create your own private/public SSH Keys and upload them or save the private/public SSH Keys Oracle Cloud generates for you.
   8. Optional – Add a custom boot volume. For this example, we will leave this blank as this is the first time we are creating this Web Server/VM.
   9. Click Create

 2. Load Balancer (Under Networking --> Load Balancer)
   **Add Details**
   
   ![Image: AddDetails_1](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/LoadBalancer/AddDetails_1.png)
   
   1. Choose a name for the Load Balancer
   2. Select Public Load Balancer
   3.	Select Ephemeral IP Address (Reserved IP Address if you have an existing IP Address)
   4.	Selected the Bandwidth (Shape) – for this example, Dynamic Shape: Small / 100 Mbps
   
   ![Image: AddDetails_2](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/LoadBalancer/AddDetails_2.png)
   
   5.	Select Networking – choose the VCN/Public Subnet created for the Load Balancer
  
  **Choose Backends**
  When adding the Web Server as the backend, the Load Balancer Wizard will automatically add the necessary Security Rules to the Load Balancer Security List and Web Server Security List.
   
   ![Image: ChooseBackends](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/LoadBalancer/ChooseBackends.png)
   
   1.	Select Load Balancing Policy – for this example, Weighted Round Robin
   2.	Click Add Backends and select the Web Server VM Instance.
   3.	Select Health Check Policy – for this example, keep the default (i.e. HTTP, Port 80, etc.)

   **Configure Listener**
   
   ![Image: ConfigureListener](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/LoadBalancer/ConfigureListener.png)

   1.	Choose a name for the Listener
   2.	Select Traffic Type – for this example, HTTP / Port 80 will be used.
   3.	Click Submit

3. Bastion Host – similar process to creating Web Server VM
   
   ![Image: BastionHost_VMPreview](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/BastionHost/BastionHost_VMPreview.png)
   
   1. Choose a name for the VM Instance
   2.	Select the compartment
   3. Select the Availability Domain
   4. Select the Image – for this example, Oracle Linux 7.9
   5.	Select the Shape – for this example, VM.Standard.E2.1.Micro
   6.	Select Networking – choose the VCN/Public Subnet created for the Bastion Host
   7.	Add SSH Keys – Either create your own private/public SSH Keys and upload them or save the private/public SSH Keys Oracle Cloud generates for you.
   8.	Optional – Add a custom boot volume. For this example, we will leave this blank as this is the first time we are creating this Bastion Host.
   9. Click Create

**(2b.4)	Update Security Lists with Security Rules
Now that the resources (i.e. Web Server, Bastion Host, Load Balancer) are created, it’s time to go back and update the Security Lists**
1. Security List for Public Subnet – Bastion Host

   ![Image: BastionHost_IngressRule](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/Network/BastionHost/BastionHost_IngressRule.png)
   
   1. Add Ingress Rule to allow for SSH to the Bastion Host
   
   ![Image: BastionHost_EgressRule](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/Network/BastionHost/BastionHost_EgressRule.png)
   
   2. Add Egress Rule to allow traffic from Bastion Host to Web Server

2. Security List for Private Subnet – Web Server

   ![Image: WebServer_IngressRule](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/Network/WebServer/WebServer_IngressRule.png)
   
   1. Add Ingress Rule to allow for SSH from Bastion Host to Web Server
   2. Automatically added by Load Balancer Wizard: Ingress Rule to allow HTTP Traffic from Load Balancer to Web Server 

   ![Image: WebServer_EgressRule](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/Network/WebServer/WebServer_EgressRule.png)
   
   3.	Add Egress Rule to allow all types of traffic to all IP’s

3. Security List for Public Subnet – Load Balancer
   
   ![Image: LoadBalancer_IngressRule](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/Network/LoadBalancer/LoadBalancer_IngressRule.png)
   
   1.	Automatically added by Load Balancer Wizard: Ingress Rule to allow HTTP Traffic to Load Balancer
   
   ![Image: LoadBalancer_EgressRule](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/Network/LoadBalancer/LoadBalancer_EgressRule.png)
   
   2. Add Egress Rule to allow HTTP traffic from Load Balancer to Web Server

**(2b.5)	SSH into Bastion Host then SSH into Web Server to Install and Configure Apache and PHP
In this Section, we will SSH into the Bastion Host, then from there SSH into the Web Server. Although the Web Server is in a private subnet, because of the NAT gateway and Route Table, the Web Server can still initiate traffic to the internet and receive traffic back.**
1. Pre-Reqs:
   
   ![Image: BastionHost_SSH_chmod400](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/BastionHost/SSH/BastionHost_SSH_chmod400.png)
   
   **Permissions for Web Server and Bastion Private Keys:**
   1. Locate Bastion Host and Web Server Private Key and for both private keys
   2. Enter the command: “chmod 400 (private key)”
   3. This sets the file permission so that only you can read the file
  
   **SSH-Agent:**
   
   Use ssh-agent to store the Web Server Private Key. This will allow best practice security, by storing the Web Server key on your local machine (not on your Bastion Host). 
   
   ![Image: WebServer_KeyAgent](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/WebServer/SSH/WebServer_KeyAgent.png)
   
   1.	To enable loading keys in the agent, locate your ssh config file in: “~/.ssh/config” and add: “AddKeysToAgent yes”
   
   ![Image: WebServer_SSH_AddAgent](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/WebServer/SSH/WebServer_SSH_AddAgent.png)
   
   2.	To add keys to the agent, Enter the command: “ssh-add [path of Web Server Private Key]”
 
2. SSH into Bastion Host:
   
   ![Image: BastionHost_PublicIPAddress](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/BastionHost/SSH/BastionHost_PublicIPAddress.png)
   
   1. Locate the public IP Address of your Bastion Host

   ![Image: BastionHost_SSH](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/BastionHost/SSH/BastionHost_SSH.png)

   2. Enter the command: “ssh -A -i (private key) (username)@(public IP Address)"
   
   Username = “opc” (this is the admin user and used because this is the first time we are logging into the Bastion)

3. From Bastion Host, SSH into the Web Server
   
   ![Image: WebServer_PrivateIPAddress](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/WebServer/SSH/WebServer_PrivateIPAddress.png)
   
   1. Locate the private IP Address of your Web Server

   ![Image: WebServer_SSH_BastiontoWeb](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/WebServer/SSH/WebServer_SSH_BastiontoWeb.png)

   2. Enter the command: “ssh -A (username)@(private IP Address)
   
   Username = “opc” (similar reason given in Bastion Host Section)
  
   3. You are now logged into the private Web Server VM Instance.

4. Install and Configure Apache
In this step, now that we are inside the Web Server VM, we will install and configure Apache. The VCN Wizard in the beginning has created a NAT Gateway and added Route Rules to the Private Route Tables 

   ![Image: WebServer_InstallApache](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/WebServer/Apache/WebServer_InstallApache.png)
   
   1. Install Apache Server, Enter the command: “sudo yum install -y httpd”

   ![Image: WebServer_EnableStartApache](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/WebServer/Apache/WebServer_EnableStartApache.png)

   2. Enable Apache, Enter the command: “sudo systemctl enable httpd”
   3. Start Apache, Enter the command: “sudo systemctl restart httpd”
   4. Enable HTTP Connection through port 80, Enter the command: “sudo firewall-cmd –add-service=http --permanent”
   5. Reload the firewall, Enter the Command: “sudo firewall-cmd --reload”

   ![Image: WebServer_AddIndexHTML](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/WebServer/Apache/WebServer_AddIndexHTML.png)
   
   6. Add an index.html file, Enter the Commands:
   
   “sudo su”
   
   “echo ‘WebServer Demo’ >/var/www/html/index.html”

   ![Image: WebServer_ActualSite](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/WebServer/Apache/WebServer_ActualSite.png)
   
   7. Test the web server by:
   
   Enter the Command: “curl localhost”
   
   Or connect to the public IP Address of the Load Balancer. Go to your browser of choice and enter 'http://(Public IP of Load Balancer)'

   8. Apache has successfully been installed

5. Install and Configure PHP

   ![Image: WebServer_PHP1](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/WebServer/PHP/WebServer_PHP1.png)

   1. Configure Oracle Linux package repos to use PHP 7, Enter the command: “sudo yum install -y oracle-php-release-el7”

   ![Image: WebServer_PHP2](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/WebServer/PHP/WebServer_PHP2.png)

   2. Install PHP 7, Enter the command: “sudo yum install -y php”
 
   ![Image: WebServer_PHP3](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/WebServer/PHP/WebServer_PHP3.png)
   
   3.	Restart Apache, Enter the command: “sudo systemctl restart httpd”
   4. Verify Installation, Enter the command: “php -v”
   5. Add a PHP test file to your instance, Enter the command: “sudo vi /var/www/html/info.php”

   Edit the file, Enter the following text:
   
   " <?php
   
    phpinfo();
    
   ?> "
   
   ![Image: WebServer_PHP4](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/WebServer_Basic/Images/WebServer/PHP/WebServer_PHP4.png)

   6. Test the connection by going to your browser of choice and connecting to: http://"Public IP of Load Balancer"/info.php

**(2b.6)	End – we have successfully created a network that hosts a private Web Server/Application that is accessed through a Public Load Balancer.**
Future extensions to this tutorial will include adding a second Web Application to fully see the capabilities of the load balancer, a private backend database for the Application, and more!


# Additional Info: Q&As, Documentations, References, etc.
Why utilize Regional Subnets?
* Regional Subnets (Private/Public) will be created for best practice (Flexible Deployments/HA). [Documentation](https://docs.oracle.com/en-us/iaas/Content/Network/Tasks/managingVCNs_topic-Overview_of_VCNs_and_Subnets.htm#Overview)
* Public Load Balancers specifically require a regional subnet or 2 AD Specific Subnets because of High Availability reasons. [Documentation](https://docs.oracle.com/en-us/iaas/Content/Balance/Concepts/balanceoverview.htm)

Chmod commands:
* What does “chmod 400” do?
   * It changes the file permission so that only you can read it
* What does “chmod 700” do?
   * Sets the permission to a file so that only the user/owner can read, write, and execute. 
* What does “chmod 600” do?
   * Sets permissions so that user/owner can read and write, but cannot execute

Bastion Host:
* [Documentation](https://docs.oracle.com/en-us/iaas/Content/Resources/Assets/whitepapers/bastion-hosts.pdf) – Covers what a Bastion Host, Oracle’s View on Them and why they support it, short tutorial, and more.

What are Health Check Policies and what does each Status mean?
* [Working with Oracle Load Balancer Health Check Policies](https://docs.oracle.com/en-us/iaas/Content/Balance/Tasks/editinghealthcheck.htm#healthstatus)
