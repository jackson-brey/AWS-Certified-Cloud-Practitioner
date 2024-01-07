# Section 11: Cloud Integration

# Section Introduction

- When we start deploying multiple applications, they will inevitably need to communicate with one another
- There are two patterns of application communication
    - Synchronous Communications: application-to-application
    - Asynchronous/Event-Based: application-to-queue-to-application
- Synchronous between applications can be problematic if there are sudden spikes in traffic
- What if you need to suddenly encode 1000 videos but usually it’s 10?
- In that case, it’s better to decouple your applications:
    - using SQS: queue model
    - using SNS: pub/sub model
    - using Kinesis: real-time data streaming model
- These services can scale independently from our application

# Amazon SQS (Simple Queue Service) - Standard Queue

![Untitled](Section%2011%20Cloud%20Integration%20322103da891241f6bc4e16c9c98f575e/Untitled.png)

- Oldest AWS offering (over 10 years old)
- Fully managed service (~serverless), used to ****************decouple**************** applications
- Scales from 1 message per second to 10,000s per second
- Default retention of messages: 4 days, maximum of 14 days
- No limit to how many messages can be in the queue
- Messages are deleted after they’re read by consumers
- Low latency (<10 ms on publish and receive)
- Consumers share the work to read messages & scale horizontally

# Amazon Kinesis

![Untitled](Section%2011%20Cloud%20Integration%20322103da891241f6bc4e16c9c98f575e/Untitled%201.png)

- ********************************************************************************************************For the exam: Kinesis = real-time big data streaming********************************************************************************************************
- Managed service to collect, process, and analyze real-time streaming data at any scale
- Too detailed for the Cloud Practitioner exam but good to know:
    - ******************************Kinesis Data Streams******************************: low latency streaming to ingest data at a scale from hundreds of thousands of sources
    - ********************************************Kinesis Data Firehose:******************************************** load streams into S3, Redshift, ElasticSearch, etc.
    - ********************************Kinesis Data Analytics:******************************** Perform real-time analytics on streams using SQL
    - **********************************************Kinesis Video Streams:********************************************** monitor real-time video streams for analytics or ML

# Amazon SNS

![Untitled](Section%2011%20Cloud%20Integration%20322103da891241f6bc4e16c9c98f575e/Untitled%202.png)

- What if you want to send one message to many receivers
- Amazon Simple Notification Service is a notification service provided as part of Amazon Web Services since 2010. It provides a low-cost infrastructure for mass delivery of messages, predominantly to mobile users
- The “event publishers” only send messages to one SNS topic
- Each subscriber to the topic will get all the messages
- Up to 12,500,000 subscriptions per topic, 100,000 topics limit

# Amazon MQ

![Untitled](Section%2011%20Cloud%20Integration%20322103da891241f6bc4e16c9c98f575e/Untitled%203.png)

- SQS and SNS are “cloud-native” services; proprietary protocols from AWS
- Traditional applications running from on-premises may use open protocols such as MQTT, AMQP, STOMP, Openwire, WSS
- When migrating to the Cloud, instead of re-engineering the application to use SQS and SNS, we can use Amazon MQ
- Amazon MQ is a managed message broker for RabbitMQ, ApacheMQ, and ActiveMQ
- Amazon MQ doesn’t “scale” as much as SQS/SNS
- Amazon MQ runs on servers and can run in Multi-AZ with failover
- Amazon MQ has both queue feature (~SQS) and topic features (~SNS)

# Integration Section - Summary

- SQS:
    - Queue service in AWS
    - For multiple Producers, messages are kept up to 14 days
    - Multiple Consumers share the read and delete messages when done
    - Used to decouple applications in AWS
- SNS:
    - Notification service in AWS
    - Subscribers: Email, Lambda, SQS, HTTP, Mobile
    - Multiple Subscribers, send all messages to all of them
    - No message retention
- Kinesis: real-time data streaming, persistence, and analysis
- Amazon MQ: managed message broker for ActiveMQ and RabbitMQ in the cloud (MQTT, AMQP.. protocols)