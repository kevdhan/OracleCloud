# DBCS (Oracle Database Cloud Service Connection to OAC)
This document goes over how to create a connection between an OAC and DBCS instance.

## Pre-Requesites:
* Before beginning, you need to collect some information about your Oracle Analytics Instance and Oracle Database Instance.
  * Here are some basic facts about the OAC and DBCS environment:
    * OAC - a Public Oracle Analytics Instance
    * DBCS - a Public Oracle Database 19c running on a VM (Shape - VM.Standard2.1)
### OAC:
1. Collect Gateway IP Address: 
 ![alt text](https://github.com/kevdhan/OracleCloud/blob/main/Platform/Oracle%20Analytics%20Cloud%20(OAC)/Connections/Database%20Cloud%20Service%20(DBCS)/Images/OAC_IPAddress.png)
### DBCS:
1. Collect Host, Port, Service Name, Username and Password
   1. Host - Public IP Address
   2. Port - 1521 (Default for Oracle Databases)
   3. Service Name - 'Database Unique Name'.'Host Domain Name'
   4. Username and Password - In relation to user created/existing in the Oracle Database
2. Create a Security Rule in the Security List for the Subnet that the Oracle Database is residing in to allow traffic from the OAC instance to the Oracle Database
   1. Go to the Security List for the Subnet/VCN that the Oracle Database is residing in:
   2. Create a Stateful TCP Ingress Rule, where Source is the OAC CIDR Range/IP Address and Destination Port is 1521 for the Database


## Actual Steps:
* Putting it all together...


Documents:
* https://docs.oracle.com/en/cloud/paas/analytics-cloud/acsom/administer-services-analytics-cloud.html#GUID-07F4764C-8111-42E7-8138-4FD3C19BA5CA 
* https://docs.oracle.com/en/cloud/paas/analytics-cloud/acoci/manage-service-access-and-security.html#GUID-E66175DD-774D-43D0-9D53-374DAFBB0B69
