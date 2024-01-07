# Section 9: Deploying and Managing Infrastructure at Scale Section

# What is CloudFormation

![Untitled](Section%209%20Deploying%20and%20Managing%20Infrastructure%20at%205f13da85b89b495198981ba7938300e3/Untitled.png)

- CloudFormation is a declarative way of outlining your AWS Infrastructure, for any resources (most of them are supported).
- For example, within a CloudFormation template, you say:
    - I want a security group
    - I want two EC2 instances using this security group
    - I want an S3 bucket
    - I want a load balancer (ELB) in front of these machines
- Then CloudFormation creates those for you, in the ****right order****, with the **************************************exact configuration************************************** that you specify

# Benefits of AWS CloudFormation

- ********************************************Infrastructure as Code********************************************
    - No resources are manually created, which is excellent for control
    - Changes to the infrastructure are reviewed through code
- ********Cost********
    - Each resource within the stack is tagged with an identifier so you can easily see how much a stack costs you
    - You can estimate the costs of your resources using the CloudFormation template
    - Savings strategy: In Dev, you could automate the deletion of templates at 5 PM and have them recreated at 8 AM, safely
- ************************Productivity************************
    - Ability to destroy and recreate an infrastructure on the cloud on the fly
    - Automated generation of Diagram for your templates
    - Declarative programming (no need to figure out ordering and orchestration)
- ****************************************************Don’t re-invent the wheel****************************************************
    - Leverage existing templates on the web
    - Leverage the documentation
- **********************************************************************Supports (almost) all AWS resources**********************************************************************
    - You can use “custom resources” for resource that are not supported

# CloudFormation Stack Designer

- Example: WordPress CloudFormation Stack
- All resources can be seen as well as the relations between the components

![Untitled](Section%209%20Deploying%20and%20Managing%20Infrastructure%20at%205f13da85b89b495198981ba7938300e3/Untitled%201.png)

# AWS Cloud Development Kit

- Define your cloud infrastructure using a familiar language:
    - JavaScript/TypeScript, Python, Java, and .NET
- The code is “compiled” into a CloudFormation template (JSON/YAML)
- You can therefore deploy infrastructure and application runtime code together
    - Great for Lambda functions
    - Great for docker containers in ECS/EKS

![Untitled](Section%209%20Deploying%20and%20Managing%20Infrastructure%20at%205f13da85b89b495198981ba7938300e3/Untitled%202.png)

# CDK Example

![Untitled](Section%209%20Deploying%20and%20Managing%20Infrastructure%20at%205f13da85b89b495198981ba7938300e3/Untitled%203.png)

# Typical Architecture: Web App 3-Tier

![Untitled](Section%209%20Deploying%20and%20Managing%20Infrastructure%20at%205f13da85b89b495198981ba7938300e3/Untitled%204.png)

# Developer Problems on AWS

- Managing infrastructure
- Deploying Code
- Configuring all the databases, load balancers, etc…
- Scaling concerns
- Most web apps have the same architecture (ALB + ASG)
- All the developers want is for their code to run consistently across different applications and environments

# AWS Elastic Beanstalk Overview

![Untitled](Section%209%20Deploying%20and%20Managing%20Infrastructure%20at%205f13da85b89b495198981ba7938300e3/Untitled%205.png)

- Elastic Beanstalk is a developer-centric view of deploying an application on AWS
- It uses all the components we’ve seen before: EC2, ASG, ELB, RDS, etc…
- But it’s all in one view that’s easy to make sense of
- We still have full control over the configuration
- Beanstalk = Platform as a Server (PaaS)
- Beanstalk is free but you pay for the underlying instance

# Elastic Beanstalk

- Managed service
    - Instance configuration / OS is handled by Beanstalk
    - Deployment strategy is configurable but performed by Elastic Beanstalk
    - Capacity provisioning
    - Load balancing & auto-scaling
    - Application health-monitoring & responsiveness
- Just the application code is the responsibility of the developer
- Three architecture models:
    - Single Instance deployment: good for dev
    - LB + ASG: great for production or pre-production web applications
    - ASG only: great for non-web apps in production (workers, etc…)

- Support for many platforms:
    - Go
    - Java SE
    - Java with Tomcat
    - .NET on Windows Server w/ IIS
    - Node.js
    - PHP
    - Python
    - Ruby
    - Packer Builder
    - Single Container Docker
    - Multi-Container Docker
    - Preconfigured Docker
    - If not supported, you can write your custom platform (advanced)

# Elastic Beanstalk - Health Monitoring

- Health agent pushes metrics to CloudWatch
- Checks for app health and publishes health events

# AWS CodeDeploy

![Untitled](Section%209%20Deploying%20and%20Managing%20Infrastructure%20at%205f13da85b89b495198981ba7938300e3/Untitled%206.png)

- We want to deploy our application automatically

- Works with EC2 Instances

![Untitled](Section%209%20Deploying%20and%20Managing%20Infrastructure%20at%205f13da85b89b495198981ba7938300e3/Untitled%207.png)

- Works with On-Premises Servers
- Hybrid service
- Servers/Instances must be provisioned and configured ahead of time with the CodeDeploy Agent

# AWS CodeCommit

![Untitled](Section%209%20Deploying%20and%20Managing%20Infrastructure%20at%205f13da85b89b495198981ba7938300e3/Untitled%208.png)

- Before pushing the application code to servers, it needs to be stored somewhere
- Developers usually store code in a repository, using the Git technology
- A famous public offering is GitHub, and AWS’ competing product is CodeCommit
- CodeCommit:
    - Source-control service that ********************************************************hosts Git-based repositories********************************************************
    - Makes it easy to collaborate with others on code
    - The code changes are automatically versioned
- Benefits:
    - Fully managed
    - Scalable & highly available
    - Private, Secured, Integrated with AWS

# AWS CodeBuild

![Untitled](Section%209%20Deploying%20and%20Managing%20Infrastructure%20at%205f13da85b89b495198981ba7938300e3/Untitled%209.png)

- Code building service in the cloud
- Compiles source code, runs tests, and produces packages that are ready to be deployed (by CodeDeploy for example)

![Untitled](Section%209%20Deploying%20and%20Managing%20Infrastructure%20at%205f13da85b89b495198981ba7938300e3/Untitled%2010.png)

- Benefits:
    - Fully managed, serverless
    - Continuously scalable & highly available
    - Secure
    - Pay-as-you-go pricing - only pay for build time
    

# AWS CodePipeline

![Untitled](Section%209%20Deploying%20and%20Managing%20Infrastructure%20at%205f13da85b89b495198981ba7938300e3/Untitled%2011.png)

- Orchestrate the different steps to have the code automatically pushed to production
    - Code ⇒ Build ⇒ Test ⇒ Provision ⇒
    - Basis for CICD (Continuous Integration & Continuous Delivery)
- Benefits:
    - Fully managed, compatible with CodeCommit, CodeBuild, CodeDeploy, Elastic Beanstalk, CloudFormation, GitHub, 3rd-party services (GitHub…) & custom plugins
    - Fast delivery & rapid updates

![Untitled](Section%209%20Deploying%20and%20Managing%20Infrastructure%20at%205f13da85b89b495198981ba7938300e3/Untitled%2012.png)

# AWS CodeArtifact

![Untitled](Section%209%20Deploying%20and%20Managing%20Infrastructure%20at%205f13da85b89b495198981ba7938300e3/Untitled%2013.png)

- Software packages depend on each other to be built (also called code dependencies), and new ones are created
- Storing and retrieving these dependencies is called ************artifact management************
- Traditionally you need to set up your own artifact management system
- CodeArtifact is a secure, scalable, and cost-effective artifact management for software development
- Works with common dependency tools such as Maven, Gradle, npm, yarn, twine, pip, and NuGet
- Developers and CodeBuild can then retrieve dependencies straight from CodeArtifact

# AWS CodeStar

![Untitled](Section%209%20Deploying%20and%20Managing%20Infrastructure%20at%205f13da85b89b495198981ba7938300e3/Untitled%2014.png)

- Unified UI to easily manage software development activities in one place
- “Quick way” to get started to correctly set up CodeCommit, CodePipeline, CodeBuild, CodeDeploy, Elastic Beanstalk, EC2, etc…
- Can edit the code “in-the-cloud” using AWS Cloud9

# AWS Cloud9

![Untitled](Section%209%20Deploying%20and%20Managing%20Infrastructure%20at%205f13da85b89b495198981ba7938300e3/Untitled%2015.png)

- AWS Cloud9 is a cloud IDE (Integrated Development Environment) for writing, running, and debugging code
- “Classic” IDE (like IntelliJ, and Visual Studio Code…) are downloaded on a computer before being used
- A cloud IDE can be used within a web browser, meaning you can work on your projects from your office, home, or anywhere with the internet with no setup necessary
- AWS Cloud9 also allows for code collaboration in real-time (pair programming)

# AWS Systems Manager (SSM)

![Untitled](Section%209%20Deploying%20and%20Managing%20Infrastructure%20at%205f13da85b89b495198981ba7938300e3/Untitled%2016.png)

- It helps you manage your EC2 and On-Premises systems at scale
- Another Hybrid AWS service
- Get operation insights about the state of your infrastructure
- A suite of 10+ products
- The most important features are:
    - Patching automation for enhanced compliance
    - Run commands across an entire fleet of servers
    - Store parameter configuration with the SSM Parameter Store
- Works for Linux, Windows, MacOS, and RaspberryPi OS (Raspbian)

# How Systems Manager Works

- We need to install the SSM agent onto the systems we control

- Installed by default on Amazon Linux AMI & some Ubuntu AMI

![Untitled](Section%209%20Deploying%20and%20Managing%20Infrastructure%20at%205f13da85b89b495198981ba7938300e3/Untitled%2017.png)

- If an instance can’t be controlled with SSM, it’s probably an issue with the SSM agent!
- Thanks to the SSM agent, we can ******************run commands, patch & configure****************** our servers

# Systems Manager - SSM Session Manager

- Allows you to start a secure shell on your EC2 and on-premises servers

- No SSH access, bastion hosts, or SSH keys needed

![Untitled](Section%209%20Deploying%20and%20Managing%20Infrastructure%20at%205f13da85b89b495198981ba7938300e3/Untitled%2018.png)

- No port 22 needed (better security)
- Supports Linux, macOS, and Windows
- Send session log data to S3 or CloudWatch Logs

# AWS OpsWorks

![Untitled](Section%209%20Deploying%20and%20Managing%20Infrastructure%20at%205f13da85b89b495198981ba7938300e3/Untitled%2019.png)

- Chef & Puppet helps you perform server configuration automatically, or repetitive actions
- They work great with EC2 and on-premises VM
- AWS OpsWorks = Managed Chef & Pupper
- It’s an alternative to AWS SSM
- Only provision standard AWS resources:
    - EC2 Instances, Database, Load Balancers, EBS volumes…
- **In the exam: Chef or Puppet needed —> AWS OpsWorks**

# OpsWorks Architecture

![Untitled](Section%209%20Deploying%20and%20Managing%20Infrastructure%20at%205f13da85b89b495198981ba7938300e3/Untitled%2020.png)

# Deployment - Summary

- CloudFormation: (AWS only)
    - Infrastructure as Code and works with almost all AWS resources
    - Repeat across Regions & Accounts
- Beanstalk: (AWS only)
    - Platform as a Service (PaaS), limited to certain programming languages or Docker
    - Deploy code consistently with a known architecture: (ex. ALB + EC2 + RDS)
- CodeDeploy (hybrid): deploy & upgrade any application onto servers
- Systems Manager (hybrid): patch, configure, and run commands at scale
- OpsWorks (hybrid): managed Chef and Puppet in AWS

# Developer Services - Summary

- CodeCommit: Store code in a private git repository (version controlled)
- CodeBuild: Build & test code in AWS
- CodeDeploy: Deploy code onto servers
- CodePipeline: Orchestration of pipeline (from code to build to deploy)
- CodeArtifact: Store software packages/dependencies on AWS
- CodeStar: Unified view for allowing developers to do CICD and code
- Cloud9: Cloud IDE (Integrated Development Environment) with collaboration
- AWS CDK: Define your cloud infrastructure using a programming language