# VPC: Virtual Private Cloud

Within a region, you’re able to create VPCs. Each VPC contain subnets (networks). Each subnet must be mapped to an AZ. It’s common to have a public ip and private ip subnet. It’s common to have many subnets per AZ.

* Subnets allow you to partition your network inside your vpc (AZ resource)
* Public subnet is a subnet that is accessible frpm the internet
* A privagte subnets is a subnets that is not accessible from the internet
* to define access to the internet and between sbuenets we use route tables.

## Intenet Gatway & NAT Gateways
* Intenet gateways helps out vpc instances connect with the internet.
* Public subnets whave a route to the internet gateway.
* NAT Gateways (AWS-managed) & NAT Instances (self-managed) allow your instances in your Private subnets to access the internet while remaining private.

## Network ACL (NACL) & Security Groups

NACL -
* A firewall which controls traffic from and to subnet
* Can have ALLOW and DENY rules
* Are attached at the subnet level.
* Rules only include IP addresses
* First mechanism of defence in our public subnet
* Default VPC has Default NACL, allows everything in and everything out.

Security Groups
* A firewall that controlls traffic to and from an ENI/ EC2 instance.
* Can have any ALLOW rules
* Rules include IP addresses and other security groups.

VPC Flow Logs
* Capture information about IP traffic going into your interfaces:
    * VPC FLow logs
    * Subnet Flow logs
    * Elastic Network Interface FLow Logs
* Helps to monitor & troubleshoot connectivity issues. Esample:
    * subnets to intenet
    * Subnets to subnets
    * intent to subnets.
#### Public Subnets usually contain:
* Load Balancers
* Static Websites
* Files
* Public Authentication Layers

Private Subnets usually contain:
* Web application servers
* Databases

Public and Private subnets can communicate if they’re in the same VPC

## VPC peering

* Connect two VPC, privately using AWS's network.
* Make them behave as if they were in the same network. 
* We need to connect VPCs using VPC peering (must not have overlapping CIDR)
* VPC Peering connection is not transitive (must be established for each VPC that need to communicate with one another)

## VPC endpoints
* Endspoints allow you to connect to AWS services using a private network instad of the public www network.
* This gives you enhanved security and lower latency to access AWS services.
VPC endpoint gateway: S3 and Dynamo DB
VPC Endpoint interface" the rest

Site to Site VPN & DIrect Connect  
* Site to site vpn
    * Connect to On prem to AWS
    * The cononection is automatically encrypted
    * goes over the public internet
* Direct Connect
    * Establish a phusical connection between on prem and AWS
    * The connection is private, secure and fast
    * goes over a private network
    * takes at a month to establish

# Three tier Architecture

#### AWS VPC Summary
* VPC & Regions aren’t much asked at the developer associate exam
* All new accounts come with a default VPC
* It’s possible to use a VPN to connect to a VPC
* VPC flow logs allow you to monitor the traffic within, in and out of your VPC (useful for security, performance, audit)
* VPC are per Account per Region
* Subnets are per VPC per AZ
* Some AWS resources can be deployed in VPC while others can’t
* You can peer VPC (within or across accounts) to make it look like they’re part of the same network