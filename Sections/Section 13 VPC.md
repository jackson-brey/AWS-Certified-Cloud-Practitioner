# Section 13: VPC

# VPC - Crash Course

- ************************************************************************************************************************************************************VPC is something you should know in-depth for the AWS Certified Solutions Architect Associate & AWS Certified SysOps Administrator exams************************************************************************************************************************************************************
- At the AWS Certified Cloud Practitioner Level, you should know about:
    - VPC, Subnets, Internet Gateways & NAT Gateways
    - Security Groups, Network ACL (NACL), VPC Flow Logs
    - VPC Peering, VPC Endpoints
    - Site to Site VPN & Direct Connect
    - Transit Gateway

# IP Addresses in AWS

- IPv4 - Internet Protocol version 4 (4.3 billion addresses)
    - Public IPv4 - can be used on the Internet
    - EC2 instances get a new public IP address every time you stop and then start it (default)
    - Private IPv4 - can be used on private networks (LAN) such as internal AWS networking (e.g., 192.168.1.1)
    - Private IPv4 is fixed for EC2 Instances even if you start/stop them
- Elastic IP - allows you to attach a fixed public IPv4 address to an EC2 instance
    - Note: has an ongoing cost if not attached to the EC2 instance or if the EC2 instance is stopped
- IPv6 - Internet Protocol version 6 (3.4 x 10^38 addresses)
    - Every IP address is public (no private range)
    - Example: 2001:db8:3333:4444:cccc:dddd:eeee:ffff

# VPC & Subnets Primer

- VPC - Virtual Private Cloud: private network to deploy your resources (regional resource)
- Subnets allow you to partition your network inside your VPC (Availability Zone resource)
- A public subnet is a subnet that is accessible from the internet
- A private subnet is a subnet that is not accessible from the internet
- To define access to the internet and between subnets, we use Route Tables

# Internet Gateway & NAT Gateways

- Internet Gateways helps our VPC instances connect with the internet
- Public Subnets have a route to the internet gateway
- NAT Gateways (AWS-managed) & NAT Instances (self-managed) allow your instances in your Private Subnets to access the internet while remaining private

# Network ACL & Security Groups

- NACL (Network ACL)
    - A firewall which controls traffic from and to a subnet
    - Can have ALLOW and DENY rules
    - Are attached at the Subnet level
    - Rules only include IP addresses
- Security Groups
    - A firewall that controls traffic to and from an ENI / EC2 instance
    - Can only have ALLOW rules
    - Rules include IP addresses and other security groups
    

# Network ACLs vs. Security Groups

| Security Group | Network ACL |
| --- | --- |
| Operates at the instance level | Operates at the subnet level |
| Supports allow rules only | Supports allow rules and deny rules |
| Is stateful: Return traffic is automatically allowed, regardless of any rules | Is stateless: Return traffic must be explicitly allowed by rules |
| We evaluate all rules before deciding whether to allow traffic | We process rules in number order when deciding whether to allow traffic |
| Applies to an instance only if someone specifies the security group when launching the instance, or associates the security group with the instance later on | Automatically applies to all instances in the subnets it’s associated with (therefore, you don’t have to rely on users to specify the security group) |

# VPC Flow Logs

- Capture information about IP traffic going into your interfaces:
    - VPC Flow Logs
    - Subnet Flow Logs
    - Elastic Network Interface Flow Logs
- Helps to monitor & troubleshoot connectivity issues
    - Example:
        - Subnets to internet
        - Subnets to subnets
        - Internet to subnets
- Captures network information from AWS-managed interfaces too: Elastic Load Balancers, ElastiCache, RDS, Aurora
- VPC Flow Logs data can go to S3, CloudWatch Logs, and Kinesis Data Firehose

# VPC Peering

- Connect two VPCs, privately using AWS’ network
- Make them behave as if they were in the same network
- Must not have overlapping CIDR (IP address range)
- VPC Peering connection is not transitive (must be established for each VPC that needs to communicate with one another)

# VPC Endpoints

- Endpoints allow you to connect to AWS Services using a private network instead of the public “www” network
- This gives you enhanced security and lower latency to access AWS services
- VPC Endpoint Gateway: S3 & DynamoDB
- VPC Endpoint Interface: the rest

# AWS PrivateLink (VPC Endpoint Services)

- The most secure & scalable way to expose a service to 1000s of VPCs
- Does not require VPC peering, internet gateway, NAT, route tables…
- Requires a network load balancer (Service VPC) and ENI (Customer VPC)

# Site-to-Site VPN & Direct Connect

- Site-to-Site VPN
    - Connect an on-premise VPN to AWS
    - The connection is automatically encrypted
    - Goes over the public internet
- Direct Connect (DX)
    - Establish a physical connection between on-premise and AWS
    - The connection is private, secure, and fast
    - Goes over a private network
    - Takes at least a month to establish

# Site-to-Site VPN

- On-premises: must use a Customer Gateway (CGW)
- AWS: must use a Virtual Private Gateway (VGW)

# AWS Client VPN

- Connect from your computer using OpenVPN to your private network in AWS and on-premise
- Allow you to connect to your EC2 instances over a private IP (just as if you were in the private VPC network)
- Goes over the public Internet

# Transit Gateway

- For having transitive peering between thousands of VPC and on-premises, hub-and-spoke (star) connection
- One single Gateway to provide this functionality
- Works with Direct Connect Gateway, VPN connections

# VPC Summary

- VPC - Virtual Private Cloud
- Subnets - Tied to an AZ, network partition of the VPC
- Internet Gateway - at the VPC level, provide Internet Access
- NAT Gateway / Instances - gives internet access to private subnets
- NACL - stateless, subnet rules for inbound and outbound
- Security Groups - stateful, operate at the EC2 instance level or ENI
- VPC Peering - connect two VPC with non-overlapping IP ranges, nontransitive
- Elastic IP - fixed public IPv4, ongoing cost if not in-use
- VPC Endpoints - provide private access to AWS services within VPC
- PrivateLink - privately connect to a service in a 3rd party VPC
- VPC Flow Logs - network traffic logs
- Site-to-Site VPN - VPN over public internet between on-premise DC and AWS
- Client VPN - OpenVPN connection from your computer into your VPC
- Direct Connect - direct private connection to AWS
- Transit Gateway - connect thousands of VPC and on-premises networks together