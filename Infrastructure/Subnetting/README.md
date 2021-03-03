# Subnetting
This tutorial will go over basic information on IP Ranges in networks/subnets

**VCN - Virtual Cloud Network**
According to Oracle, a VCN is: "A virtual, private network that you set up in Oracle data centers. It closely resembles a traditional network, with firewall rules and specific types of communication gateways that you can choose to use. A VCN resides in a single Oracle Cloud Infrastructure region and covers one or more CIDR blocks of your choice"

**Subnet**
According to Oracle, a Subnet is: "Subdivisions you define in a VCN (for example, 10.0.0.0/24 and 10.0.1.0/24). Subnets contain virtual network interface cards (VNICs), which attach to instances. Each subnet consists of a contiguous range of IP addresses that do not overlap with other subnets in the VCN. You can designate a subnet to exist either in a single availability domain  or across an entire region (regional subnets are recommended). Subnets act as a unit of configuration within the VCN: All VNICs in a given subnet use the same route table, security lists, and DHCP options (see the definitions that follow). You can designate a subnet as either public or private when you create it. Private means VNICs in the subnet can't have public IP addresses. Public means VNICs in the subnet can have public IP addresses at your discretion"

Here is an example

In each 
