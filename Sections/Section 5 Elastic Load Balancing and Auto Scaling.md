# Section 5: Elastic Load Balancing and Auto Scaling Groups

# Scalability and High Availability

Scalability means that an application/system can handle greater loads by adapting

2 kinds of scalability:

1. Vertical scalability
2. Horizontal scalability (= elasticity)

Scalability is linked but different from High Availability

# Vertical Scalability

Vertical Scalability means increasing the size of the instance

- ex: application runs on a t2.micro. Scaling vertically means running it on a t2.large

Vertical Scalability is very common for non-distributed systems, such as a database

There’s usually a limit to how much you can vertically scale (hardware limit)

# Horizontal Scalability

Horizontal Scalability means increasing the number of instances/systems for your application

Horizontal scaling implies distributed systems

Very common for web applications/modern applications

- designed with horizontal scaling in mind

Easy to horizontally scale thanks to AWS services such as EC2

# High Availability

High Availability (HA) usually goes hand-in-hand with horizontal scaling

HA means running your application/system in at least 2 AZs

The goal of HA is to survive a data center loss

# Scalability vs. Elasticity (vs. Agility)

**Scalability**: ability to accommodate a larger load by making the hardware stronger (scale up), or by adding nodes (scale out)

********************Elasticity******************** (more cloud-native): once a system is scalable, elasticity means that there will be some “auto-scaling” so that the system can scaled based on the load. This is “cloud-friendly”: pay-per-use, match demands, optimize costs

**************Agility************** (not related to scalability-distractor): new IT resources are only a click away, which means that you reduce the time to make those resources available to your Developers from weeks to minutes

# *Elastic Load Balancing (ELB) Overview*

# What is Load Balancing?

Load balancers are servers that forward internet traffic to multiple servers (EC2 Instances) downstream

- also called the “backhand EC2 Instances”

Allows for better backend scaling

![Untitled](Section%205%20Elastic%20Load%20Balancing%20and%20Auto%20Scaling%20%20d60ded043be04ca2900a57789870e0f5/Untitled.png)

# Why Use a Load Balancer?

Spread load across multiple downstream instances

Expose a single point of access (DNS) to your applications

Seamlessly handle failures of downstream instances

Do regular health checks on your environment/instances

Provide SSL termination (HTTPS) for your websites

Highly available across zones

# Why Use an Elastic Load Balancer?

An ELB (Elastic Load Balancer) is a managed load balancer

- AWS guarantees that it will be working
- AWS takes care of upgrades, maintenance, HA
- AWS provides only a few configuration options

It costs less to setup your own load balancer but it will be a lot more effort on your end (maintenance and/or integrations)

3 kinds of load balancers offered by AWS:

1. Application Load Balance (HTTP/HTTPS only) - Layer 7
2. Network Load Balancer (ultra-high performance, allows for TCP) - Layer 4
3. Classic Load Balancer (slowly retiring) - Layers 4 & 7

![Untitled](Section%205%20Elastic%20Load%20Balancing%20and%20Auto%20Scaling%20%20d60ded043be04ca2900a57789870e0f5/Untitled%201.png)

# What’s an Auto Scaling Group?

In real life, the load on your sites/applications can change

In the cloud, you can create and get rid of servers very quickly

The goal of an Auto Scaling Group (ASG) is to:

- Scale-out (add EC2 instances) to match an increased load
- Scale in (remove EC2 instances) to match a decreased load
- Ensure we have a minimum & maximum # of machines running
- Automatically register new instances to a load balancer
- Replace unhealthy instances

**Cost Savings**: only run at an optimal capacity (principle of the cloud)

Implements Elasticity for your applications, across multiple AZs

# Auto Scaling Groups - Scaling Strategies

****************************Manual Scaling****************************: Update the size of an ASG manually

********************Dynamic Scaling********************: Respond to changing demand

- different scaling policies:
    - simple/step scaling
        - when a CloudWatch alarm is triggered (ex. CPU > 70%), add 2 units
        - when a CloudWatch alarm is triggered (ex. CPU < 30%), remove 1
    - target tracking scaling
        - ex. I want the average ASG CPU to stay around 40%
    - scheduled scaling
        - anticipates a scaling based on known usage patterns
            - ex. increase the minimum capacity to 10 at 5 PM on Fridays
    - predictive scaling*
        - uses Machine Learning to predict future traffic ahead of time
        - automatically provisions the right # of EC2 instances in advance
        - useful when your load has predictable time-based patterns