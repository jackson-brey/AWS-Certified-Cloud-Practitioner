# Section 6: Amazon S3

# S3 Overview

- Amazon S3 is one of the main building blocks of AWS
- It’s advertised as “infinitely scaling” storage
- Many websites use Amazon S3 as a backbone
- Many AWS services use Amazon S3 as an integration as well

# Amazon S3 Use Cases

- Backup and storage
- Disaster Recovery
- Archive
- Hybrid Cloud storage
- Application Hosting
- Media Hosting
- Data Lakes and Big Data Analytics
- Software Delivery
- Static Websites

# Amazon S3 - Buckets

- Amazon S3 allows you to store objects (files) in “buckets” (directories)
- Buckets must have a globally unique name (across all regions, all accounts)
- Buckets are defined at the region level
- S3 looks like a global service, but buckets are created in a region

Naming convention:

- no uppercase, no lowercase
- 3-63 characters long
- not an IP
- must start with a lowercase letter or number
- must ***not*** start with the prefix “xn- -“
- must ***not*** end with the suffix “-s3alias”

# Amazon S3 - Objects

Objects (files) have a Key

The key is the FULL path:

- s3://my-bucket/my_file.txt
- s3://my-bucket/my_folder1/another_folder/my_file.txt

The key is composed of prefix and object name

- s3://my-bucket/my_folder1/another_folder/my_file.txt
    - prefix: *my_folder1/another_folder*
    - object: ***********my_file.txt***********

There’s no concept of “directories” within buckets (although, the UI will trick you to think otherwise)

Just keys with very long names that contain slashes (“/”)

Object values are the content of the body:

- Max. object size is 5 TB (5000 GB)
- If uploading more than 5 GB, must use “multi-part upload”

Metadata (list of text key/value pairs — system or user metadata)

Tags (Unicode key/value pair — up to 10): useful for security/lifestyle

Version ID (if versioning is enabled)

# Amazon S3 - Security

User-Based

- IAM Policies - which API calls should be allowed for a specific user from IAM

Resource-Based

- Bucket Policies - bucket-wide rules from the S3 console - allows cross account
- Object Access Control List (ACL) - finer grain (can be disabled)
- Bucket Access Control List (ACL) - less common (can be disabled)

******************************************************************************************************************************************************Note: an IAM principle can access an S3 object if:******************************************************************************************************************************************************

- ***************************************************************************The user IAM permissions ALLOW it OR THE resource policy ALLOWS it***
- ***************************There’s no explicit DENY***************************

Encryption: encrypt objects in Amazon S3 using encryption keys

# S3 Bucket Policies

- JSON-based policies
    - Resources: buckets and objects
    - Effect: Allow / Deny
    - Actions: Set of APIs to Allow or Deny
    - Principal: The account or user to apply the policy to
- Use S3 bucket for policy to:
    - Grant public access to the bucket
    - Force objects to be encrypted at upload
    - Grant access to another account (Cross Account)

![Untitled](Section%206%20Amazon%20S3%20a19fcd1856d5443fa378c5a5deb06ff4/Untitled.png)

# Bucket Settings for Block Public Access

![Untitled](Section%206%20Amazon%20S3%20a19fcd1856d5443fa378c5a5deb06ff4/Untitled%201.png)

- **These settings were created to prevent company data leaks**
- If you know your bucket shouldn’t be public, leave these settings enabled
- Can be set at the account level

# Amazon S3 - Static Website Hosting

- S3 can host static websites and have them accessible on the Internet
- The website URL will be (depending on the region)
    - http://***********bucket-name***********.s3-website-**********aws-region**********.amazonaws.com
    
    OR
    
    - http://***********bucket-name***********.s3-website.**********aws-region**********.amazonaws.com
- If you get a **************************403 Forbidden************************** error, make sure the bucket policy allows public reads!

# ********************************************************************Amazon S3 - Versioning********************************************************************

You can version your files in Amazon S3

It’s enabled at the bucket level

Same key overwrite will change the “version”: 1, 2, 3….

It is best practice to version your buckets

- Protect against unintended deletes (ability to restore a version)
- Easy roll back to the previous version

Notes:

- Any file that is not versioned before enabling versioning will
- Suspending versioning does not delete the previous versions

# Amazon S3 - Replication (CRR & SRR)

- Must enable Versioning in source and destination buckets
- Cross-Region Replication (CRR)
- Same-Region Replication (SRR)
- Buckets can be in different AWS accounts
- Copying is asynchronous
- Must give proper IAM permissions to S3

![Untitled](Section%206%20Amazon%20S3%20a19fcd1856d5443fa378c5a5deb06ff4/Untitled%202.png)

Use cases:

- CRR - compliance, lower latency access, replication across accounts
- SRR - log aggregation, live replication between production and test accounts

# S3 Storage Classes

- Amazon S3 Standard - General Purpose
- Amazon S3 Standard - Infrequent Access (IA)
- Amazon S3 One Zone - Infrequent Access
- Amazon S3 Glacier Instant Retrieval
- Amazon S3 Glacier Flexible Retrieval
- Amazon S3 Glacier Deep Archive
- Amazon S3 Intelligent Tiering

Can move between classes manually or using S3 Lifecycle configurations

# S3 Durability and Availability

- Durability:
    - High durability (99.999999999%) of objects across multiple AZ
    - If you store 10,000,000 objects with Amazon S3, you can on average expect to incur a loss of a single object once every 10k years
    - Same for all storage classes
- Availability:
    - Measures how readily available a service is
    - Varies depending on storage class
    - Example: S3 standard has 99.99% availability = not available 53 minutes per yet

# S3 Standard - General Purpose

- 99.99% Availability
- Used for frequently accessed data
- Low latency and high throughput
- Sustain 2 concurrent facility failures
- Use Cases: Big Data analytics, mobile & gaming applications, content distribution…

# S3 Storage Classes - Infrequent Access

- For data that is less frequently accessed, but requires rapid access when needed
- Lower cost than S3 Standard

- Amazon S3 Standard - Infrequent Access (S3 Standard-IA)
    - 99.9% Availability
    - Use cases: Disaster Recovery, backups

- Amazon S3 One Zone - Infrequent Access (S3 One Zone-IA)
    - High durability (99.999999999%) in a single AZ; data lost when AZ is destroyed
    - 99.5% Availability
    - Use Cases: Storing secondary backup copies of on-premise data, or data you can recreate

# Amazon S3 Glacier Storage Classes

- Low-cost object storage meant for archiving/backup
- Pricing: price for storage + object retrieval cost

- Amazon S3 Glacier Instant Retrieval
    - Millisecond retrieval, is great for data accessed once a quarter
    - Minimum storage duration of 90 days
- Amazon S3 Glacier Flexible Retrieval (formerly Amazon S3 Glacier):
    - Expedited (1 to 5 minutes), Standard (3 to 5 hours), Bulk (5 to 12 hours) – free
    - Minimum storage duration of 90 days
- Amazon S3 Glacier Deep Archive – for long-term storage:
    - Standard (12 hours), Bulk (48 hours)
    - Minimum storage duration of 180 days

# S3 Intelligent-Tiering

- Small monthly monitoring and auto-tiering fee
- Moves objects automatically between Access Tiers based on usage
- There are no retrieval charges in S3 Intelligent-Tiering

- ***Frequent Access tier (automatic)***: default tier
- **********************************Infrequent Access tier (automatic)**********************************: objects not accessed for 30 days
- ***************************************Archive Instant Access tier (automatic)***************************************: object not accessed for 90 days
- **********Archive Access tier (optional)**********: configurable from 90 days to 700+ days
- ***********************************Deep Archive Access tier (optional)***********************************: configurable from 180 days to 700+ days

# S3 Encryption

![Untitled](Section%206%20Amazon%20S3%20a19fcd1856d5443fa378c5a5deb06ff4/Untitled%203.png)

# Shared Responsibility Model for S3

| AWS is responsible for: | You are responsible for: |
| --- | --- |
| Infrastructure (global security, durability, availability, sustaining the concurrent loss of data in two facilities) | S3 Versioning |
| Configuration and vulnerability analysis | S3 Bucket Policies |
| Compliance validation | S3 Replication Setup |
|  | Logging and Monitoring |
|  | S3 Storage Classes |
|  | Data encryption at rest and in transit |

# AWS Snow Family

Highly secure, portable devices to collect and process data at the edge, and migrate data into and out of AWS

Data migration: 

![Untitled](Section%206%20Amazon%20S3%20a19fcd1856d5443fa378c5a5deb06ff4/Untitled%204.png)

Edge computing:

![Untitled](Section%206%20Amazon%20S3%20a19fcd1856d5443fa378c5a5deb06ff4/Untitled%205.png)

# Data Migrations with AWS Snow Family

![Untitled](Section%206%20Amazon%20S3%20a19fcd1856d5443fa378c5a5deb06ff4/Untitled%206.png)

Challenges:

- Limited connectivity
- Limited bandwidth
- High network cost
- Shared bandwidth (can’t maximize the line)
- Connection stability

| AWS Snow Family: offline devices to perform data migrations |
| --- |
| If it takes more than a week to transfer over the network, use Snowball devices! |

# Diagrams

- Direct upload to S3:

![Untitled](Section%206%20Amazon%20S3%20a19fcd1856d5443fa378c5a5deb06ff4/Untitled%207.png)

- With Snow Family:

![Untitled](Section%206%20Amazon%20S3%20a19fcd1856d5443fa378c5a5deb06ff4/Untitled%208.png)

# Snowball Edge (for data transfers)

- Physical data transport solution: move TBs or PBs of data in or out of AWS
- Alternative to moving data over the network (and paying network fees)
- Pay per data transfer job
- Provide block storage and Amazon S3-compatible object storage
- **************************************************************Snowball Edge Storage Optimized**************************************************************
    - 80 TB of HDD or 28TB NVMe capacity for block volume and S3-compatible object storage
- Use cases: large data cloud migrations, DC decommission, disaster recovery

![Untitled](Section%206%20Amazon%20S3%20a19fcd1856d5443fa378c5a5deb06ff4/Untitled%209.png)

# AWS Snowcone & Snowcone SSD

- Small, portable computing, anywhere, rugged & secure, withstands harsh environments
- Light (4.5 lbs, 2.1 kg)
- Device used for edge computing, storage, and data transfer
- ****************Snowcone**************** - 8 TB of HDD Storage
- **************************Snowcone SSD************************** - 14 TB of SSD Storage
- Use Snowcone where Snowball does not fit (space-constrained environment)
- Must provide your own battery/cables
- Can be sent back to AWS offline, or connect it to the internet and use AWS DataSync to send data

![Untitled](Section%206%20Amazon%20S3%20a19fcd1856d5443fa378c5a5deb06ff4/Untitled%2010.png)

# AWS Snowmobile

- Transfer exabytes of data (1 EB = 1,000 PB = 1,000,000 TBs)
- Each Snowmobile has 100 PB of capacity (use multiple in parallel)
- High security: temperature controlled, GPS, 24/7 video surveillance
- Better than Snowball if you transfer more than 10 PB

# AWS Snow Family for Data Migrations

![Untitled](Section%206%20Amazon%20S3%20a19fcd1856d5443fa378c5a5deb06ff4/Untitled%2011.png)

# Snow Family - Usage Process

1. Request Snowball devices from the AWS console for delivery
2. Install the Snowball client / AWS OpsHub on your servers
3. Connect the Snowball to your servers and copy files using the Client
4. Ship back the device when you’re done (goes to the right AWS facility)
5. Data will be loaded into an S3 bucket
6. Snowball is completely wiped

# What is Edge Computing

- Process data while it’s being created on an edge location
- These locations may have:
    - Limited / no internet access
    - Limited / no easy access to computing power
- We set up a **************************************************Snowball Edge / Snowcone************************************************** device to do edge computing
- Use cases of Edge Computing:
    - Preprocess data
    - Machine learning at the edge
    - Transcoding media streams
- Eventually (if need be) we can ship back the device to AWS (for transferring data, for example)

# Snow Family - Edge Computing

- Snowcone & Snowcone SSD (smaller)
    - 2 CPUs, 4 GB of memory, wired or wireless access
    - USB-C power using a cord or the optional battery
- Snowball Edge - Compute Optimized
    - 104 vCPUs, 416 GiB of RAM
    - Optional CPU (useful for video processing or machine learning)
    - 28 TB NVMe or 42TB HDD usable storage
    - Storage Clustering available (up to 16 nodes)
- Snowball Edge -  Storage Optimized
    - Up to 40 vCPUs, 80 GiB of RAM, 80 TB storage
- All: Can run EC2 Instance & AWS Lambda functions (using AWS IoT Greengrass)
- Long-term deployment options: 1 and 3 years discounted pricing

# AWS OpsHub

- Historically, to use Snow Family devices, you needed a CLI
- Today, you can use AWS OpsHub (a software you install on your computer/laptop) to manage your Snow Family Device
    - Unlocking and configuring single or clustered devices
    - Transferring files
    - Launching and managing instances running on Snow Family Devices
    - Monitor device metrics (storage capacity, active instances on your device)
    - Launch compatible AWS services on your devices (Ex: Amazon EC2 instances, AWS DataSync, Network File System (NFS))

# Hybrid Cloud for Storage

- AWS is pushing for a “hybrid cloud”
    - Part of your infrastructure is on-premises
    - Part of your infrastructure is on the cloud
- This can be due to:
    - Long cloud mgirations
    - Security requirements
    - Compliance requirements
    - IT strategy
- S3 is a proprietary storage technology (unlike EFS / NFS), so how is S3 data exposed on premise? —> AWS Storage Gateway

# AWS Storage Cloud Native Options

![Untitled](Section%206%20Amazon%20S3%20a19fcd1856d5443fa378c5a5deb06ff4/Untitled%2012.png)

# AWS Storage Gateway

- Bridge between on-premise data and cloud data in S3
- Hybrid storage service to allow on-premises to seamlessly use the AWS Cloud
- Use cases: disaster recovery, backup & restore, and tiered storage
- Types of Storage Gateway:
    - File Gateway
    - Volume Gateway
    - Tape Gateway
- *No need to know the types at the exam*

![Untitled](Section%206%20Amazon%20S3%20a19fcd1856d5443fa378c5a5deb06ff4/Untitled%2013.png)

# Amazon S3 - Summary

- Buckets vs Objects: global unique name, tied to a region
- S3 security: IAM policy, S3 Bucket Policy (public access), S3 Encryption
- S3 Websites: host a static website on Amazon S3
- S3 Versioning: multiple versions for files, prevent accidental deletes
- S3 Replication: same-region or cross-region, must enable versioning
- S3 Storage Classes: Standard, IA, IZ-IA, Intelligent, Glacier (Instant, Flexible, Deep)
- Snow Family: import data onto S3 through a physical device, edge computing
- OpsHub: desktop application to manage Snow Family devices
- Storage Gateway: hybrid solution to extend on-premise storage to S3