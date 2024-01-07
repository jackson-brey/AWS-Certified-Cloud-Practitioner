# Section 10: Global Infrastructure Section

# Why Make a Global Application?

- A global application is an application deployed in multiple geographies
- On AWS: this could be Regions and/or Edge Locations
- Decreased Latency:
    - Latency is the time it takes for a network packet to reach a server
    - It takes time for packet from Asia to reach the US
    - Deploy your applications closer to your users to decrease latency, better experience
- Disaster Recovery (DR)
    - If an AWS region goes down
    - You can failover to another region and have your application still working
    - A DR plan is important to increase the availability of your application
- Attack protection: distributed global infrastructure is harder to attack

# Global AWS Infrastructure

- Regions: For deploying applications and infrastructure
- Availability Zones: Made of multiple data centers
- Edge Locations (Points of Presence): for content delivery as close as possible to users

# Global Applications in AWS

- Global DNS: Route 53
    - Great to route users to the closest deployment with the least latency
    - Great for disaster recovery strategies
- Global Content Delivery Network (CDN): CloudFront
    - Replicate part of your application to AWS Edge Locations - decrease latency
    - Cache common requests - improved user experience and decreased latency
- S3 Transfer Acceleration
    - Accelerate global uploads & downloads into Amazon S3
- AWS Global Accelerator:
    - Improve global application availability and performance using the AWS global network

# Amazon Route 53 Overview

- Route53 is a Managed DNS
- DNS is a collection of rules and records which helps clients understand how to reach a server through URLs

# Route53 - Diagram for A Record

![Untitled](Section%2010%20Global%20Infrastructure%20Section%20f3f8599284804e8ab58eff87c598b569/Untitled.png)

# Route53 Routing Policies

- **Know these at a high-level for the Cloud Practitioner Exam**

![Untitled](Section%2010%20Global%20Infrastructure%20Section%20f3f8599284804e8ab58eff87c598b569/Untitled%201.png)

![Untitled](Section%2010%20Global%20Infrastructure%20Section%20f3f8599284804e8ab58eff87c598b569/Untitled%202.png)

- Simple Routing Policy
    - Used for a single resource that performs a given function for your domain
        - Ex.: A web server that serves content for the [example.com](http://example.com) website.
    - You can use simple routing to create records in a private hosted zone
- Weighted Routing Policy
    - Used to route traffic to multiple resources in proportions that you specify
    - You can use weighted routing to create records in a private hosted zone
- Latency Routing Policy
    - Use when you have resources in multiple AWS Regions and you want to route traffic to the region that provides the best latency
    - You can use latency routing to create records in a private hosted zone
- Failover Routing Policy
    - Use when you want to configure active-passive failover
    - You can use failover routing to create records in a private hosted zone

# Amazon CloudFront

![Untitled](Section%2010%20Global%20Infrastructure%20Section%20f3f8599284804e8ab58eff87c598b569/Untitled%203.png)

- Content Delivery Network (CDN)
- Improves read performance, content is cached at the edge
- Improves users experience
- 216 Point of Presence globally (edge locations)
- DDoS protection (because worldwide), integration with Shield, AWS Web Application Firewall

# CloudFront - Origins

- S3 Bucket
    - For distributing files and caching them at the edge
    - Enhanced security with CloudFront ******************************************************Origin Access Control (OAC)******************************************************
    - OAC is replacing Origin Access Identity (OAI)
    - CloudFront can be used as an ingress (to upload files to S3)
- Custom Origin (HTTP)
    - Application Load Balancer
    - EC2 Instance
    - S3 website (must first enable the bucket as a static S3 website)
    - Any HTTP backend you want

# CloudFront at a High Level

![Untitled](Section%2010%20Global%20Infrastructure%20Section%20f3f8599284804e8ab58eff87c598b569/Untitled%204.png)

# CloudFront vs S3 Cross Region Replication

- CloudFront:
    - Global Edge network
    - Files are cached for a TTL (maybe a day)
    - Great for static content that must be available everywhere
- S3 Cross Region Replication:
    - Must be setup for each region you want replication to happen
    - Files are updated in near real-time
    - Read-only
    - Great for dynamic content that needs to be available at low latency in a few regions
    
    # S3 Transfer Acceleration
    
    - Increase transfer speed by transferring files to an AWS edge location which will forward the data to the S3 bucket in the target region

![Untitled](Section%2010%20Global%20Infrastructure%20Section%20f3f8599284804e8ab58eff87c598b569/Untitled%205.png)

# AWS Global Accelerator

![Untitled](Section%2010%20Global%20Infrastructure%20Section%20f3f8599284804e8ab58eff87c598b569/Untitled%206.png)

- Improve global application availability and performance using the AWS global network
- Leverage the AWS internal network to optimize the route to your application (60% improvement)
- 2 Anycast IPs are created for your application and traffic is sent through Edge Locations
- The Edge locations send the traffic to your application

# AWS Global Accelerator vs. CloudFront

- They both use the AWS global network and its edge locations around the world
- Both services integrate with AWS Shield for DDoS protection
- CloudFront - Content Delivery Network
    - Improves performance for your cacheable content (such as images and videos)
    - Content is served at the edge
- Global Accelerator
    - No caching, proxying packets at the edge to applications running in one or more AWS Regions
    - Improves performance for a wide range of applications over TCP or UDP
    - Good for HTTP use cases that require static IP addresses
    - Good for HTTP use cases that require deterministic, fast regional failover
    

# AWS Outposts

![Untitled](Section%2010%20Global%20Infrastructure%20Section%20f3f8599284804e8ab58eff87c598b569/Untitled%207.png)

- **************************Hybrid Cloud:************************** a business that keeps an on-premises infrastructure alongside a cloud infrastructure
- Therefore, two ways of dealing with IT systems:
    - One for the AWS cloud (using the AWS console, CLIs, and AWS APIs)
    - One for their on-premises infrastructure

- **************************************************AWS Outposts are “server racks”************************************************** that offer the same AWS infrastructure, services, APIs & tools to build your own applications on-premises just as in the cloud

![Untitled](Section%2010%20Global%20Infrastructure%20Section%20f3f8599284804e8ab58eff87c598b569/Untitled%208.png)

- ******************************************************************************************AWS will set up and manage “Outposts Racks”****************************************************************************************** within your on-premises infrastructure and you can start leveraging AWS services on-premises
- **********************************************************************************You are responsible for the Outposts Rack physical security**********************************************************************************
- Benefits:
    - Low-latency access to on-premises systems
    - Local data processing
    - Data residency
    - Easier migration from on-premises to the cloud
    - Fully managed service
- Some services that work on Outposts:
    - Amazon EC2
    - Amazon EBS
    - Amazon S3
    - Amazon EKS
    - Amazon ECS
    - Amazon RDS
    - Amazon EMR

# AWS WaveLength

![Untitled](Section%2010%20Global%20Infrastructure%20Section%20f3f8599284804e8ab58eff87c598b569/Untitled%209.png)

- WaveLength Zones are infrastructure deployments embedded within the telecommunications providers’ data centers at the edge of the 5G networks
- Brings AWS services to the edge of the 5G networks
- Example: EC2, EBS, VPC…
- Ultra-low latency applications through 5G networks
- Traffic doesn’t leave the Communication Service Provider’s (CSP) network
    
    ![Untitled](Section%2010%20Global%20Infrastructure%20Section%20f3f8599284804e8ab58eff87c598b569/Untitled%2010.png)
    
- High-bandwidth and secure connection to the parent AWS Region
- No additional charges or service agreements
- Use cases: Smart Cities, ML-assisted diagnostics, Connected Vehicles, Interactive Live Video Streams, AR/VR, Real-time Gaming…

# AWS Local Zones

- Places AWS compute, storage, database, and other selected AWS services closer to end users to run latency-sensitive applications
- Extend your VPC to more locations - “Extension of an AWS Region”
- Compatible with EC2, RDS, ECS, EBS, ElastiCache, Direct Connect…
- Example:
    - AWS Region: N. Virginia (us-east-1)
    - AWS Local Zones: Boston, Chicago, Dallas, Houston, Miami…
    

# Global Applications Architecture

![Untitled](Section%2010%20Global%20Infrastructure%20Section%20f3f8599284804e8ab58eff87c598b569/Untitled%2011.png)

![Untitled](Section%2010%20Global%20Infrastructure%20Section%20f3f8599284804e8ab58eff87c598b569/Untitled%2012.png)

# Global Applications in AWS - Summary

- Global DNS: Route53
    - Great to route users to the closest deployment with the least latency
    - Great for disaster recovery strategies
- Global Content Delivery Network (CDN): CloudFront
    - Replicate part of your application to AWS Edge Locations - decrease latency
    - Cache common requests - improved user experience and decreased latency
- S3 Transfer Acceleration
    - Accelerate global uploads & downloads into Amazon S3
- AWS Global Accelerator
    - Improve global application availability and performance using the AWS global network
- AWS Outposts
    - Deploy Outpost Racks in your own data centers to extend AWS services
- AWS WaveLength
    - Brings AWS services to the edge of the 5G networks
    - Ultra-low latency applications
- AWS Local Zones
    - Bring AWS resources (compute, database, storage, …) closer to your users
    - Good for latency-sensitive applications