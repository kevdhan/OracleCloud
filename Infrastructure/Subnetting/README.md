# Subnetting
This tutorial will go over basic information on IP Ranges in networks/subnets

**VCN - Virtual Cloud Network**
According to Oracle, a VCN is: "A virtual, private network that you set up in Oracle data centers. It closely resembles a traditional network, with firewall rules and specific types of communication gateways that you can choose to use. A VCN resides in a single Oracle Cloud Infrastructure region and covers one or more CIDR blocks of your choice"

**Subnet**
According to Oracle, a Subnet is: "Subdivisions you define in a VCN (for example, 10.0.0.0/24 and 10.0.1.0/24). Subnets contain virtual network interface cards (VNICs), which attach to instances. Each subnet consists of a contiguous range of IP addresses that do not overlap with other subnets in the VCN. You can designate a subnet to exist either in a single availability domain  or across an entire region (regional subnets are recommended). Subnets act as a unit of configuration within the VCN: All VNICs in a given subnet use the same route table, security lists, and DHCP options (see the definitions that follow). You can designate a subnet as either public or private when you create it. Private means VNICs in the subnet can't have public IP addresses. Public means VNICs in the subnet can have public IP addresses at your discretion"

Here is an example Oracle VCN/Subnet Architecture:

![Example VCN/Subnet Architecture](https://github.com/kevdhan/OracleCloud/blob/main/Infrastructure/Subnetting/Images/VCN_Subnet_Example.png)

## IP Address Block + Masking
In this section, we will go over how to calculate the network address, broadcast address, gateway addreses, and usable IP Addresses of a CIDR Block (tied to a network/subnet). This [Networking Overview](https://docs.oracle.com/en-us/iaas/Content/Network/Concepts/overview.htm#Reserved) documentation by Oracle further discusses this.

Given CIDR Block: 10.1.0.255/16
IP Address = 10.1.0.255 = 0000 1010.0000 0001.0000 0000.1111 1111
Network Mask = /16 = 255.255.0.0 = 1111 1111.1111 1111.0000 0000.0000 0000

**(1) Calculate Network Address**
Do a logical AND of the IP Address and Network Mask:
IP Address   =  0000 1010.0000 0001.0000 0000.1111 1111
Network Mask =  1111 1111.1111 1111.0000 0000.0000 0000
AND Result   =  0000 1010.0000 0001.0000 0000.0000 0000  =  **10.1.0.0**

**(2) Calculate Broadcast Address**
Do a "Force Host Bit" of the IP Address with the inverse of the Network Mask (aka Host Bit Mask):
IP Address     =  0000 1010.0000 0001.0000 0000.1111 1111
Host Bit Mask  =  0000 0000.0000 0000.1111 1111.1111 1111
Results        =  0000 1010.0000 0001.1111 1111.1111 1111  =  **10.1.255.255**

**(3) Calculate Default Gateway Address**
Oracle uses the first available usable Host IP Address. So in this example, it would be 10.1.0.0+1 = **10.1.0.1**

**(4) Calculate Usable Host IP Range**
Now that we've found the Network and Broadcast Address, the Usable Host IP Range is (Network Address + 1) - (Broadcast Address - 1).
However, because Oracle reserves the first available usable Host IP Address for the Default Gateweay Address, it is actually +2.

So, for this example: (10.1.0.0+2) - (10.1.255.255-1) = **10.1.0.2 - 10.1.255.254**

**Documentation**

Here's a documentation from Azure ["Understand TCP/IP addressing and subnetting basics"](https://docs.microsoft.com/en-us/troubleshoot/windows-client/networking/tcpip-addressing-and-subnetting#default-gateways) that further discusses IP addresses, subnet mask, and default gateway.

Here's a documentation from Stack Exchange: [Calculating Addresses from IP Address and Mask](https://networkengineering.stackexchange.com/questions/7106/how-do-you-calculate-the-prefix-network-subnet-and-host-numbers)






