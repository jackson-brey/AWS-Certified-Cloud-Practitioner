# Section 3: EC2: Elastic Compute Cloud

# EC2 Basics - Amazon EC2

EC2 is one of the most popular offerings of AWS

- stands for Elastic Compute Cloud —> IaaS

It mainly consists of:

- renting virtual machines (EC2)
- storing data on virtual drives (EBS)
- distributing load across machines (ELB)
- scaling the services using an auto-scaling group (ASG)

***Knowing EC2 is fundamental to understanding how the Cloud works***

# EC2 Sizing and Configuration Options

Operating System (OS):

- Linux (most popular)
- Windows
- MacOS

How much compute power and cores (CPU)?

How much random-access memory (RAM)?

How much storage space:

- Network-attached (EBS & EFS)
- Hardware (EC2 Instance Store)

*Network card*: speed of the card, Public IP address

*Firewall rules*: security group

*Bootstrap script* (configure at first launch): EC2 User Data

# EC2 User Data

It is possible to bootstrap our instances using an EC2 user data script

Bootstrapping means launching commands when a machine starts

That script is only run once at the instance’s first start

EC2 user data is used to automate boot tasks such as:

- installing updates/software
- downloading common files from the internet

The EC2 User Data Script runs with the Root user

<img src="../images/bootstrap_example.png" height="300" width="300">

# EC2 Instance Types: Example

| Instance | vCPU | Mem (GiB) | Storage | Network Performance | EBS Bandwidth |
| --- | --- | --- | --- | --- | --- |
| t2.micro | 1 | 1 | EBS-Only | Low to Moderate | N/A |
| t2.xlarge | 4 | 16 | EBS-Only | Moderate | N/A |
| c5d.4xlarge | 16 | 32 | 1x400 NVMe SSD | Up to 10 Gbps | 4750 Mbps |
| x5.16xlarge | 64 | 512 | EBS-Only | 20 Gbps | 13600 Mbps |
| m5.8xlarge | 32 | 128 | EBS-Only | 10 Gbps | 6800 Mbps |

**t2.micro is part of the AWS free tier (~750 hours/month)**

# EC2 Instance Types - Overview

AWS has the following naming convention:

- m5.2xlarge
    - **m**: instance class
    - **5**: generation (AWS improves them over time)
    - **************2xlarge**************: size within the instance class

# EC2 Instance Types - General Purpose

Great for a diversity of workloads such as web servers or code repositories —> t2.micro is general-purpose

Balance between:

- Compute
- Memory
- Networking

*General purpose instances are either T, M, or A-series*

# EC2 Instance Types - Compute Optimized

Great for compute-intensive tasks that require high-performance processors:

- Batch processing workloads
- Media transcoding
- High-performance web servers
- High-performance computing (HPC)
- Scientific modeling and machine learning
- Gaming servers

*Compute-optimized start with the C-series or H*

# EC2 Instance Types - Memory Optimized

Fast performance for workloads that process large datasets in memory

Use cases:

- High-performance, relational/non-relational databases
- Distributed web-scale cache stores
- In-memory databases optimized for business intelligence (BI)
- Applications performing real-time processing of big, unstructured data

************************Memory-optimized start with the R-series, X, High Memory, or Z************************

# EC2 Instance Types - Storage Optimized

Great for storage-intensive tasks that require high, sequential read and write access to large datasets on local storage

Use cases: 

- High-frequency online transaction processing (OLTP) systems
- Relational and NoSQL databases
- Cache for in-memory databases (ex. Redis)
- Data warehousing applications
- Distributed file systems

***Storage-optimized start with I, D, or H-series***

# Introduction to Security Groups

Security Groups are the fundamentals of network security in AWS

They control how traffic is allowed in and out of our EC2 instances

Security groups only contain *****allow***** rules

A security group’s rules can be referenced by IP or by security group

Security groups act as a “firewall” on EC2 instances

They regulate:

- Access to ports
- Authorized IP ranges — IPv4 and IPv6
- Control of inbound and outbound network

<img src="../images/security_group_creation.png" height="400" width="700">

# Security Groups - Good to Know

Can be attached to multiple instances

Locked down to a region/VPC combination

Does live “outside” the EC2 — if traffic is blocked, the EC2 won’t see it

It’s a good practice to have a separate security group for SSH access

If your application is not accessible (time out), then it’s a security group issue

- “Connection refused”? —> Application error

All inbound traffic is blocked by default

- Outbound traffic is authorized by default

# Classic Ports to Know

22: SSH (Secure Shell), log into a Linux instance

21: FTP (File Transfer Protocol), upload files into a file share

22: SFTP (Secure FTP), upload files via SSH

80: HTTP, access unsecured websites

443: HTTPS, access secured websites

3389: RDP (Remote Desktop Protocol), log into a Windows instance

# EC2 Instance Connect

EC2 Instance Connect allows us to do a browser-based SSH session into our EC2 instance

- Connections get established using a temporary SSH key

# EC2 Instances - Hands On

It’s bad practice to configure EC2 instances with our personal details because any IAM user in our account will be able to connect and retrieve the credential values for that instance

Use IAM Roles for EC2 instances

# EC2 Instances - Purchasing Options

On-Demand Instances - short workload, predictable pricing, pay by second

Reserved (1 & 3 years):

- **Reserved instances** - long workloads
- **Convertible Reserved Instances** - long workloads with flexible instances

Savings Plans (1 & 3 years) - a commitment to an amount of usage, long workload

Spot Instances - short workloads, cheap, can lose instances (less reliable)

Dedicated Hosts - book an entire physical server, control instance placement

Dedicated Instances - no other customers will share your hardware

Capacity Reservations - reserve capacity in a specific AZ for any duration

# EC2 On-Demand

Pay for what you use:

- Linux or Windows - billing per second, after the first minute
- All other OS - billing per hours

Has the highest cost but no upfront payment

No long-term commitment

Recommended for short-term and uninterrupted workloads, where you can’t predict how the application will behave

# EC2 Reserved Instances

Up to 72% discount compared to on-demand

You reserve a specific instance attribute (Instance Type, Region, Tenancy, OS)

Reservation Period - 1 year (small discount) or 3 years (larger discount)

Payment options - no upfront, partial upfront, all upfront

Reserved Instance’s Scope - Regional or Zonal (reserve capacity in an AZ)

Recommended for steady-state usage applications (ex: databases)

You can buy and sell in the Reserved Instance Marketplace

Convertible Reserved Instance

- Can change the EC2 instance type, instance family, OS, scope, and tenancy
- Up to 66% discount

# EC2 Savings Plan

Get a discount based on long-term usage (up to 72%, same as Reserved Instances)

Commit to a certain type of usage ($10/hour for 1 or 3 years)

Usage beyond EC2 Savings Plans is billed at the On-Demand price

Locked to a specific instance family and AWS region (ex. m5 in us-east-1)

Flexible across:

- Instance Size (ex. m5.xlarge, m5.2xlarge)
- OS
- Tenancy (Host, Dedicated, Default)

# EC2 Spot Instances

Can get a discount of up to 90% compared to on-demand

Instances that you can “lose” at any point in time if your max price is less than the current spot prices

The **********MOST********** cost-efficient instances in AWS

Useful for workloads that are resilient to failure:

- Batch jobs
- Data analysis
- Image processing
- Any distributed workloads
- Workloads with a flexible start/end time

*Not suitable for critical jobs or databases*

# EC2 Dedicated Hosts

A physical server with an EC2 instance capacity fully dedicated to your server

Allows you to address **********************************************compliance requirements********************************************** and use your existing server-bound software licenses (per socket, per core, per VM)

Purchasing options:

- On-demand - pay per second for active Dedicated Hosts
- Reserved - 1 or 3 years (no upfront, partial, or full payment options)

The most expensive option

Useful for software that has complicated license models (BYOL - Bring Your Own License) or for companies that have strong regulatory or compliance needs

# EC2 Dedicated Instances

Instances run on hardware that’s dedicated to you

May share hardware with other instances in the same account

No control over instance placement (can move after stop/start)

# EC2 Capacity Reservations

Reserve On-Demand instances capacity in a specific AZ for any duration

You always have access to EC2 capacity when you need it

No time commitment (create/cancel anytime), no billing discounts

Combine with Regional Reserved Instances and Savings Plans to benefit from billing discounts

You’re charged at On-Demand rates whether you run instances or note

Suitable for short-term uninterrupted workloads that need to be in a specific AZ

# Shared Responsibility Model for EC2

| AWS is responsible for: | You are responsible for: |
| --- | --- |
| Infrastructure (global network security) | Security groups’ rules |
| Isolation on physical hosts | OS patches and updates |
| Replacing faulty hardware | Software and utilities installed on the EC2 instance |
| Compliance validation | IAM Roles assigned to EC2 and IAM user access management |
|  | Data security on your instance |