# Section 16: Account Management, Billing and Support

# AWS Organizations

- Global service
- Allows to manage multiple AWS accounts
- The main account is the master account
- Cost Benefits:
    - Consolidated Billing across all accounts - single payment method
    - Pricing benefits from aggregated usage (volume discount for EC2, S3…)
    - Pooling of Reserved EC2 instances for optimal savings
- API is available to automate AWS account creation
- Restrict account privileges using Service Control Policies (SCP)

# Multi-Account Strategies

- Create accounts per department, per cost center, per dev/test/prod, based on regulatory restrictions (using SCP), for better resource isolation (Ex: VPC), to have separate per-account service limits, isolated account for logging
- Multi-Account vs One Account Multi VPC
- Use tagging standards for billing purposes
- Enable CloudTrail on all accounts, send logs to central S3 account
- Send CloudWatch Logs to central logging account

# Service Control Policies (SCP)

- Whitelist or blacklist IAM actions
- Applied at the OU or Account level
- Does not apply to the Master Account
- SCP is applied to all the Users and Roles of the Account, including Root user
- The SCP does not affect service-linked roles
    - Service-linked roles enable other AWS services to integrate with AWS Organizations and can’t be restricted by SCPs
- SCP must have an explicit Allow (does not allow anything by default)
- Use cases:
    - Restrict access to certain services (for example: can’t use EMR)
    - Enforce PCI compliance by explicitly disabling services

# AWS Organization - Consolidated Billing

- When enabled, provides you with:
    - Combined Usage - combine the usage across all AWS accounts in the AWS Organization to share the volume pricing, Reserved Instances and Savings Plans discounts
    - One Bill - get one bill for all AWS Accounts in the AWS Organization
- The management account can turn off Reserved Instances discount sharing for any account in the AWS Organization, including itself

# AWS Control Tower

- Easy way to set up and govern a secure and compliant multi-account AWS environment based on best practices
- Benefits:
    - Automate the set up of your environment in a few clicks
    - Automate ongoing policy management using guardrails
    - Detect policy violations and remediate them
    - Monitor compliance through an interactive dashboard
- AWS Control Tower runs on top of AWS Organizations:
    - It automatically sets up AWS Organizations to organize accounts and implement SCPs (Service Control Policies)

# AWS Resource Access Manager (AWS RAM)

- Share AWS resources that you own with other AWS accounts
- Share with any account or within your Organization
- Avoid resource duplication
- Supported resources include Aurora, VPC Subnets, Transit Gateway, Route53, EC2 Dedicated Hosts, License Manager Configurations

# AWS Service Catalog

- Users that are new to AWS have too many options, and may create stacks that are not compliant/in-line with the rest of the organization
- Some users just want a quick self-service portal to launch a set of authorized products pre-defined by admins
- Includes: virtual machines, databases, storage options, etc…

# Pricing Models in AWS

- AWS has 4 pricing models:
1. Pay as you go: pay for what you use, remain agile, and responsive, and meet scale demands
2. Save when you reserve: minimize risks, predictably manage budgets, comply with long-term requirements
    1. Reservations are available for EC2 Reserved Instances, DynamoDB Reserved Capacity, ElastiCache Reserved Nodes, RDS Reserved Instance, Redshift Reserved Nodes
3. Pay less by using more: volume-based discounts
4. Pay less as AWS grows

# Free Services & Free Tier in AWS

- IAM
- VPC
- Consolidated Billing
- Elastic Beanstalk (you do pay for the resources created)
- CloudFormation (you do pay for the resources created)
- Auto Scaling Groups (you do pay for the resources created)
- Free Tier: https://aws.amazon.com/free/
    - EC2 t2.micro instance for a year
    - S3, EBS, ELB, AWS Data transfer

# Compute Pricing - EC2

- Only charged for what you use
- Number of instances
- Instance configuration:
    - Physical capacity
    - Region
    - OS and software
    - Instance type & size
- ELB running time and amount of data processed
- Detailed monitoring
- On-demand instances:
    - Minimum of 60s
    - Pay-per-second (Linux/Windows) or per hour (other)
- Reserved instances:
    - Up to 75% discount compared to On-demand on hourly rate
    - 1- or 3-year commitment
    - All upfront, partial upfront, no upfront
- Spot instances:
    - Up to 90% discount compared to On-demand on hourly rate
    - Bid for unused capacity
- Dedicated Host:
    - On-demand
    - Reservation for 1 year or 3-year commitment
- Savings plans as an alternative to save on sustained usage

# Compute Pricing - Lambda & ECS

- Lambda:
    - Pay-per-call
    - Pay-per-duration
- ECS:
    - EC2 Launch Type Model: No additional fees, you pay for AWS resources stored and created in your application
- Fargate:
    - Fargate Launch Type Model: Pay for vCPU and memory resources allocated to your applications in your containers

# Storage Pricing - EBS

- Volume type (based on performance)
- Storage volume in GB per month provisioned
- IOPS:
    - General Purpose SSD: included
    - Provisioned IOPS SSD: Provisioned amount in IOPS
    - Magnetic: Number of requests
- Snapshots:
    - Added data cost per GB per month
- Data transfer:
    - Outbound data transfers are tiered for volume discounts
    - Inbound is free

# Database Pricing - RDS

- Per hour billing
- Database characteristics:
    - Engine
    - Size
    - Memory class
- Purchase type:
    - On-demand
    - Reserved instances (1 or 3 years) with required up-front
- Backup Storage: There is no additional charge for backup storage up to 100% of your total database storage for a region
- Additional storage (per GB per month)
- Number of input and output requests per month
- Deployment type (storage and I/O are variable):
    - Single AZ
    - Multiple AZs
- Data transfer:
    - Outbound data transfers are tiered for volume discounts
    - Inbound is free

# Content Delivery - CloudFront

- Pricing is different across different geographic regions
- Aggregated for each edge location, then applied to your bill
- Data Transfer Out (volume discount)
- Number of HTTP/HTTPS requests

# Networking Costs in AWS per GB - Simplified

- Use Private IP instead of Public IP for good savings and better network performance
- Use the same AZ for maximum savings (at the cost of high availability)

# Savings Plan

- Commit a certain $ amount per hour for 1 or 3 years
- The easiest way to setup long-term commitments on AWS
- EC2 Savings Plan
    - Up to 72% discount compared to On-Demand
    - Commit to the usage of individual instance families in a region (e.g. C5 or M5)
    - Regardless of AZ, size (m5.xl to m5.4xl), OS (Linux/Windows), or tenancy
    - All upfront, partial upfront, no upfront
- Compute Saving Plan
    - Up to 66% discount compared to On-Demand
    - Regardless of Family, Region, size, OS, tenancy, compute options
    - Compute Options: EC2, Fargate, Lambda
- Machine Learning Savings Plan: SageMaker
- Setup from the AWS Cost Explorer console
- Estimate pricing at https://aws.amazon.com/savingsplans/pricing

# AWS Compute Optimizer

- Reduce costs and improve performance by recommending optimal AWS resources for your workloads
- Helps you choose optimal configurations and right-size your workloads (over/under-provisioned)
- Uses Machine Learning to analyze your resources’ configurations and their utilization CloudWatch metrics
- Supported resources
    - EC2 instances
    - EC2 Auto Scaling Groups
    - EBS volumes
    - Lambda functions
- Lower your costs by up to 25%
- Recommendations can be exported to S3

# Billing and Costing Tools

- Estimating costs in the cloud:
    - Pricing Calculator
- Tracking costs in the cloud:
    - Billing Dashboard
    - Cost Allocation Tags
    - Cost and Usage Reports
    - Cost Explorer
- Monitoring against costs plans:
    - Billing Alarms
    - Budgets

# AWS Pricing Calculator

- Available at https://calculator.aws
- Estimate the cost for your solution architecture

# Cost Allocation Tags

- Use cost allocation tags to track your AWS costs on a detailed level
- AWS generated tags
    - Automatically applied to the resource you create
    - Starts with Prefix aws: (e.g. aws: createdBy)
- User-defined tags
    - Defined by the user
    - Starts with Prefix user:

# Tagging and Resource Groups

- Tags are used for organizing resources:
    - EC2: instances, images, load balancers, security groups
    - RDS, VPC resources, Route53, IAM users, etc…
    - Resources created by CloudFormation are all tagged the same way
- Free naming, common tags are Name, Environment, Team…
- Tags can be used to create Resource Groups
    - Create, maintain, and view a collection of resources that share common tags
    - Manage these tags using the Tag Editor

# Cost and Usage Reports

- Dive deeper into your AWS costs and usage
- The AWS Cost & Usage Report contains the most comprehensive set of AWS cost and usage data available, including additional metadata about AWS services, pricing, and reservations (e.g., Amazon EC2 Reserved Instances (RIs))
- The AWS Cost & Usage Report lists AWS usage for each service category used by an account and its IAM users in hourly or daily line items, as well as any tags that you have activated for cost allocation purposes
- Can be integrated with Athena, Redshift, or QuickSight

# Cost Explorer

- Visualize, understand, and manage your AWS costs and usage over time
- Create custom reports that analyze cost and usage data
- Analyze your data at a high level: total costs and usage across all accounts
- Or Monthly, hourly, resource-level granularity
- Choose an optimal Savings Plan (to lower prices on your bill)
- Forecast usage up to 12 months based on previous usage

# Billing Alarms in CloudWatch

- Billing data metric is stored in CloudWatch us-east-1
- Billing data are for overall worldwide AWS costs
- It’s for actual cost, not for projected costs
- Intended as a simple alarm (not as powerful as AWS Budgets)

# AWS Budgets

- Create budgets and send alarms when costs exceed the budget
- 4 types of budgets: Usage, Cost, Reservation, Saving Plans
- For Reserved Instances (RI)
    - Track utilization
    - Supports EC2, ElastiCache, RDS, Redshift
- Up to 5 SNS notifications per budget
- Can filter by Service, Linked Account, Tag, Purchase Option, Instance Type, Region, Availability Zone, API Operation, etc…
- The same options as AWS Cost Explorer
- 2 budgets are free, then $0.02/day/budget

# AWS Cost Anomaly Detection

- Continuously monitor your cost and usage using ML to detect unusual spends
- It learns your unique, historic spending patterns to detect one-time cost spikes and/or continuous cost increases (you don’t need to define thresholds)
- Monitor AWS services, member accounts, cost allocation tags, or cost categories
- Sends you the anomaly detection report with root-cause analysis
- Get notified with individual alerts or daily/weekly summaries (using SNS)

# AWS Service Quotas

- Notify you when you’re close to a service quota value threshold
- Create CloudWatch Alarms on the Service Quotas console
- Example: Lambda concurrent executions
- Request a quota increase from AWS Service Quotas or shutdown resources before limit is reached

# Trusted Advisor

- No need to install anything - a high-level AWS account assessment
- Analyze your AWS accounts and provides recommendations on 5 categories
- Cost optimization
- Performance
- Security
- Fault tolerance
- Service limits

# Trusted Advisor - Support Plans

| 7 Core Checks (Basic & Developer Support Plan) | Full Checks (Business & Enterprise Support Plan) |
| --- | --- |
| S3 Bucket Permissions | Full Checks available on the 5 categories |
| Security Groups - Specific Ports Unrestricted | Ability to set CloudWatch alarms when reaching limits |
| IAM Use (one IAM user minimum) | Programmatic Access using AWS Support API |
| MFA on Root Account |  |
| EBS Public Snapshots |  |
| RDS Public Snapshots |  |
| Service Limits |  |

# AWS Basic Support Plan

- Customer Service & Communities - 24x7 access to customer service, documentation, whitepapers, and support forums.
- AWS Trusted Advisor - Access to the 7 core Trusted Advisor checks and guidance to provision your resources following best practices to increase performance and improve security.
- AWS Personal Health Dashboard - A personalized view of the health of AWS services, and alerts when your resources are impacted.

# AWS Developer Support Plan

- All Basic Support Plan +
- Business hours email access to Cloud Support Associates
- Unlimited cases / 1 primary contact
- Case severity/response times:
    - General guidance: < 24 business hours
    - System impaired: < 12 business hours
    

# AWS Business Support Plan (24/7)

- Intended to be used if you have production workloads
- Trusted Advisor – Full set of checks + API access
- 24x7 phone, email, and chat access to Cloud Support Engineers
- Unlimited cases / unlimited contacts
- Access to Infrastructure Event Management for an additional fee.
- Case severity/response times:
    - General guidance: < 24 business hours
    - System impaired: < 12 business hours
    - Production system impaired: < 4 hours
    - Production system down: < 1 hour

# AWS Enterprise On-Ramp Support Plan 
(24/7)

- Intended to be used if you have production or business-critical workloads
- All of Business Support Plan +
- Access to a pool of Technical Account Managers (TAM)
- Concierge Support Team (for billing and account best practices)
- Infrastructure Event Management, Well-Architected & Operations Reviews
- Case severity/response times:
    - Production system impaired: < 4 hours
    - Production system down: < 1 hour
    - Business-critical system down: < 30 minutes

# AWS Enterprise Support Plan (24/7)

- Intended to be used if you have mission-critical workloads
- All of Business Support Plan +
- Access to a designated Technical Account Manager (TAM)
- Concierge Support Team (for billing and account best practices)
- Infrastructure Event Management, Well-Architected & Operations Reviews
- Case severity/response times:
    - Production system impaired: < 4 hours
    - Production system down: < 1 hour
    - Business-critical system down: < 15 minutes

# Account Best Practices - Summary

- Operate multiple accounts using Organizations
- Use SCP (service control policies) to restrict account power
- Easily setup multiple accounts with best practices with AWS Control Tower
- Use Tags & Cost Allocation Tags for easy management & billing
- IAM guidelines: MFA, least privilege, password policy, password rotation
- Config to record all resource configurations & compliance over time
- CloudFormation to deploy stacks across accounts and regions
- Trusted Advisor to get insights, Support Plan adapted to your needs
- Send Service Logs and Access Logs to S3 or CloudWatch Logs
- CloudTrail to record API calls made within your account
- If your Account is compromised: change the root password, delete and rotate all passwords/keys, contact the AWS support
- Allow users to create pre-defined stacks defined by admins using AWS Service Catalog

# Billing and Costing Tools - Summary

- **Compute Optimizer**: recommends resources’ configurations to reduce cost
- **Pricing Calculator**: cost of services on AWS
- **Billing Dashboard**: high-level overview + free tier dashboard
- **Cost Allocation Tags**: tag resources to create detailed reports
- **Cost and Usage Reports**: most comprehensive billing dataset
- **Cost Explorer**: View current usage (detailed) and forecast usage
- **Billing Alarms**: in us-east-1 – track overall and per-service billing
- **Budgets**: more advanced – track usage, costs, RI, and get alerts
- **Savings Plans**: easy way to save based on long-term usage of AWS
- ******************************************************Cost Anomaly Detection:****************************************************** detect unusual spending using Machine Learning
- **************************Service Quotas:************************** notify you when you’re close to the service quota threshold