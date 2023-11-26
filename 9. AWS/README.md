
# Module 2: Security and Compliance Aspects of AWS
### AWS Shared Responsinility Model
### Basic Security Aspect of the AWS Platform
### Basic Compliance Aspect of the AWS Platform
### AWS Security Platform
### 
## Security, Identity and Compleance Services
### AWS IAM
### AWS WAF
### AWS Shield
### AWS Firewall Manager
### Amazon Inspector
### AWS Trusted Advisor


# Module 3: Platform, Technology and Services

## Core Services Overview and their use cases
### AWS IAM
- A web service that enables AWS customers to manage users and user permissions. 
- Manages users, security credentials such as access keys, and permissions. 
- A user is allowed to do only what they need to do as part of the user's job. 
- Also defines roles, policies, permissions etc. 
- Provides IAM best practices 
    - Enabling MFA for privileged users 
    - Grant least privilege etc. 

### Amazon Virtual Private Cloud (VPC)
- Amazon Virtual Private Cloud (VPC) enables you to launch AWS resources into a virtual network that you've defined.
- This virtual network closely resembles a traditional network that you'd operate your own data center, with the benefits of using the scalable infrastructure of AWS. 
- A virtual private cloud (VPC) is a virtual network dedicated to your AWS account. 
- It is logically isolated from other virtual networks in the AWS Cloud. 
- You can launch your AWS resources, such as Amazon EC2 instances, into your VPC.
- There's no additional charge for using Amazon VPC. 
- You pay the standard rates for the instances and other Amazon EC2 features that you use. 
- There are charges for using an AWS managed VPN connection and using a NAT gateway.
- VPC is specific to a region. CIDR block overview e.g. 10.0.0.0/16 
- VPC Networking Components 
    - Network Interfaces 
    - Route Tables 
    - Internet Gateways 
    - Egress-Only 
    - Internet Gateways 
    - DHCP Options Sets 
    - DNS 
    - Elastic IP Addresses 
    - VPC Endpoints 
    - NAT 
    - VPC Peering 
    - ClassicLink

### Network-to-Amazon VPC Connectivity Options 
- Useful for integrating AWS resources with your existing on-site services (for example: monitoring, authentication, security, data or other systems) by extending your internal networks into the AWS Cloud. 
- Also allows internal users to seamlessly connect to resources hosted on AWS just like any other internally facing resource. 
- AWS Managed VPN 
- AWS Transit Gateway + VPN 
- AWS Direct Connect 
- AWS Direct Connect + AWS Transit Gateway 
- AWS Direct Connect + VPN 
- AWS Direct Connect + AWS Transit Gateway + VPN 
- AWS VPN CloudHub 
- Software Site-to-Site VPN

### Amazon Elastic Compute Cloud (EC2)
#### Overview: 
- A web service that enables you to launch and manage server instances in Amazon's data centers 
- 'Instances' are available in different sizes and configurations 
- It provides resizable compute capacity in cloud 
- Reduces the time require to obtain and boot new server instances to minutes, allowing you to quickly scale capacity, both up and down, as your computing requirement changes. 

#### Purchasing Options: 
- On Demand 
- Spot 
- Reserved 
- Dedicated Hosts 
- Savings Plan

#### Choice of Instance Families: 
- General Purpose 
- Compute Optimized 
- Memory Optimized 
- Accelerated Computing 
- Storage Optimized 

- AMI: Amazon Machine Image Overview 
- Data Storage Options 
    - Amazon EC2 Instance Store 
    - Amazon EBS (Elastic Block Store) 
    - Amazon EFS 
    - Amazon S3 Reference

### Amazon Elastic Block Store (EBS)
- Easy to use, high performance block storage service designed for use with Amazon EC2 for both throughput and transaction intensive workloads at any scale. 
- A broad range of workloads supported: 
    - Relational and Non-Relational Databases 
    - Enterprise Applications 
    - Containerized Applications 
    - Big Data Analytics Engines 
- EBS is a persistent storage solution 
- Key terms: 
    - Volumes: durable, block-level storage device that you can attack to a single EC2 instance. 
    - Snapshots: Point in time back up of EBS volume. These are stored in S3. Snapshots are incremental backups. 
- Amazon EBS Volume Types 
    - Solid State Drives (SSD) 
        - EBS Provisional IOPS SSD (io1) 
        - EBS General Purpose SSD (gp2) 
    - Hard Disk Drives (HDD) 
        - Throughput Optimized HDD (st1) 
        - Cold HDD (sc1) 
    - Previous generation - Magnetic

### Elastic Load Balancing (ELB)
- Elastic Load Balancing automatically distributes incoming application traffic across multiple targets, such as Amazon EC2 instances, containers, IP addresses, and Lambda functions. 
- It can handle the varying load of your application traffic in a Single Availability Zone or across multiple Availability Zones. 
- Four types of load balancers: 
    - Application Load Balancer 
        - Operating at the individual request level (Layer 7), it routes to targets within Amazon VPC based on the content of the request. 
    
    - Network Load Balancer 
        - Operating at the connection level (Layer 4). It routes traffic to targets within Amazon VPC and is capable of handling millions of requests per second while maintaining ultra-low latencies
        
    - Gateway Load Balancer 
        - Makes it easy to deploy, scale, and run thiry-party virtual networking appliances used for security, network analytics, and other use cases. 
    
    - Classic Load Balancer 
        - Operates at both the request level and connection level.

### AWS Auto Scaling
- AWS Auto Scaling monitors your applications and automatically adjusts capacity to maintain steady, predictable performance at the lowest possible cost. 
- Using AWS Auto Scaling, it's easy to setup application scaling for multiple resources across multiple services in minutes. 
- Helps build scaling plans for resources like Amazon EC2 instances and Spot Fleets, Amazon ECS tasks, Amazon DynamoDB tables and indexes, and Amazon Aurora Replicas.

### Amazon Aurora
- Fully managed, MySQL, and PostgreSQL compatible relational database built for the cloud. 
- Performance and availability of commercial-grade databases at 1/10th the cost. 
- Up to five times faster than standard MySQL databases and three times faster than standard PostgreSQL databases
- Auto-scales up to 64TB per database instance 
- High performance and availability 
- 15 low-latency Read replicas 
- Point-in-time recovery, continuous backup to Amazon S3, and replication across three Availability Zones (AZs)
- Maintains 2 copies of data in each AZ and needs atleast 3 Azs, hence replicating 6 copies of your data across 3 Availability Zones and backing up your data continuously to Amazon S3 
- No stand by DB instance, just a matter DB and related replicas. 
- Aurora storage is self-healing.. Disks are scanned and repaid automatically 
- Amazon Aurora Servicesless : is an on-demand, auto-scaling configuration for Aurora where the database will automatically start-up, shut down, and scale up or down capacity based on your application's needs.

### Amazon Relational Database Servcie (RDS)
- Web service to setup, operate and scale relational database in the cloud. 
- Manages common database administration tasks 
- Provides cost-efficient, resizable capacity for an industry-standard relational database

#### Features of Amazon RDS 
- Manages backups, software patching, automatic failure detection and recovery 
- You can use the database products you are already familiar with some include MySQL, PostgreSQL, Oracle, and Microsoft SQL Server 
- Automated backups of DB 
- High availability 
- You can help control who can access your RDS databases by using AWS IAM to define users and permissions 

#### Amazon RDS Components 
- DB Instances 
    - It is an isolated database environment in the cloud DB instance storage comes in three types: Magnetic, General Purpose (SSD), and Provisioned IOPS (PIOPS). 

    - Each DB instance runs a DB engine. Amazon RDS currently supports the MySQL, MariaDB, PostgreSQL, Oracle, and Microsoft SQL Server DB engines. 
    - The computation and memory capacity of a DB instance is determined by its DB instance class 
- Security Groups 
    - It controls the access to a DB instance 
- Regions and Availability Zones 
    - You can run your DB instance in several Availability Zones, an option called a Multi-AZ deployment. 
- Monitoring an Amazon RDS DB Instance 
    - You can use the free Amazon CloudWatch service to monitor the performance and health of a DB instance 
- DB instance Backups 
    - Automated Backup 
    - DB Snapshots 
- DB Instance Replication 
    - High Availability (Multi-AZ) 
    - Read Replicas 
- Amazon RDS Security 
    - Run your DB instance in an Amazon Virtual Private Cloud (VPC) 
    - Use IAM policies for assigning permissions Use security groups to control what IP addresses or Amazon EC2 instances can connect to your database on a DB instance 
    - Use Secure Socket Layer (SSL) connections with DB instances running the MySQL, MariaDB, PostgreSQL, Oracle, or Microsoft SQL Server database engines 
    - Use RDS encryption to secure your RDS instances and snapshots at rest. 
    - Use network encryption and transparent data encryption with Oracle DB instances 
    - Use the security features of your DB engine to control who can log in to the databases on a database was on your local network

### Amazon DynamoDB
- DynamoDB is a NoSQL database. 
- Except for the required primary key, an DynamoDB table is schema-less. 
- Individual items in an DynamoDB table can have any number of attributes, although there is a limit on the item size. 
- DynamoDB achieves much of its speed through the service's extensive use of solid-state drives (SSD)
- DynamoDB stores data in partitions. A partition is an allocation of storage for a table, backed by solid-state drives (SSDs) and automatically replicated across multiple Availability Zones within an AWS Region. 
- Data Model 
    - Table, Items, Attributes 
    - Primary Key: Primary key (single attribute - Hash key) 
    - Compose Primary Key (Hash and Range Key) 

### Amazon RedShift
- A fast, fully, managed, petabyte-scale data warehouse service in the cloud. 
- You can query and combine exabytes of structured and semi-structured data across your data warehouse, operational database, and data lake using standard SQL
- Simple and cost-effective to efficiently analyze all your data using your existing business intelligence tools.- Lets you easily save the results of your queries back to your S3 data lake using open formats, like Apache Parquet, so that you can do additional analytics from other analytics services like Amazon EMR, Amazon Athena, and Amazon SageMaker.

### Amazon EMR
- Cloud big data platform for processing vast amounts of data using open source tools such as Apache Spark, Apache Hive, Apache Hbase, Apache Flink, Apache Hudi, and Presto. 
- Run Petabyte-scale analysis at less than half of the cost of traditional on-premises solutions. 
- Over 3x faster than standard Apache Spark 
- Amazon EMR uses Apache Hadoop as its distributed data processing engine. 
- It utilizes a hosted Hadoop framework running on the web-scale infrastructure of Amazon EC2 and Amazon S3. 
- Amazon EMR lets you focus on crunching or analyzing your data without having to worry about time-consuming set-up, management or tuning of Hadoop clusters or the compute capacity upon which they sit. 
- Concepts: EMR Cluster, Master node, Core node, worker/task nodes

### Amazon Athena
- An interactive query service that makes it easy to analyze data in Amazon S3 using standard SQL 
- Athena is serverless, easy to use 
- Point to your data in Amazon S3, define the schema, and start querying using standard SQL 
- Results delivered within seconds 
- Athena enables querying on large data sets without needing complex ETL jobs 
- Some Uses: 
    - Query log files stored in S3 like ELB logs 
    - Analyze AWS cost and usage reports 
    - Generate business reports on data in S3

### AWS Glue
- A fully managed extract, transform, and load (ETL) service that makes it easy for customers to prepare and load their data for analytics. 
- We can create and run an ETL job with a few clicks in the AWS Management Console. 
- Point AWS Glue to your data stored on AWS, and AWS Glue discovers your data and stored associated metadata (e.g. table definition and schema) in the AWS Glue Data Catalog 
- Data is immediately searchable, queryable, and available for ETL 
- Example use case: Queries against an Amazon S3 data lake

### Amazon QuickSight
- Amazon QuickSight is a scalable, serverless, embeddable, machine learning-powered business intelligence (BI) service built for the cloud. 
    - Amazon QuickSight is a cloud-scale business intelligence (BI) service that you can use to deliver easy-to-understand insights to the people who you work with, wherever they are. 
    - QuickSight can include AWS data, third-party data, big data, spreadsheet data, SaaS data, B2B data, and more. 
    - As a fully managed cloud-based service, Amazon QuickSight provides enterprise-grade security, global, availability, and built-in redundancy. 
    - It also provides the user-management tools that you need to scale from 10 users to 10,000, all with no infrastructure to deploy or manage. 
    - QuickSight gives decision-makers the opportunity to explore and interpret information in an interactive visual environment. 
    - They have secure access to dashboards from any device on your network and from mobile devices. 
- QuickSight offers per-user pricing for admins, authors ($24/month), and readers (Max charge of $5/month ($0.30 session)) Ideal for Business Intelligence (BI) applications and embedded use cases with high per-user activity in a month.

### Some Storage Services from AWS
- Amazon S3
- Amazon S3 Glacier
- AWS Snowball
- Amazon Elastic File System (EFS)
- AWS Storage Gateway

### Amazon Simple Storage Service (S3)
- Amazon Simple Storage Service (Amazon S3) is an object storage service that offers industry-leading scalability, data and security, and performance. 
- Use cases: websites, mobile applications, backup and restore, enterprise applications, IoT devices, and big data analytics 
- Designed for 99.99999999999% (11 9's) of durability, and stores data for millions of applications for companies all around the world 
- S3 best practices recommend NOT making bucket or data public unless absolutely needed. 
- Also, S3 supports data encryption, both client-side and server-side. 
- Also, S3 supports data encryption, both client-side and server-side. 
- Bottomline - Protect your data! 
- S3 Components: 
    - Bucket 
    - Object 
    - Folder 
    - Keys 
    - Regions 
    - ACLs and Bucket Policies

### S3 Storage Classes
- Storage Class is a system metadata, configured for the object. 
- Each object in Amazon S3 has a storage class associated with it. 
- Amazon S3 supports the following storage classes: 
    - S3 Standard: Default 
    - S3 Intelligent-Tiering: Designed to optimize storage costs by automatically moving data to the most cost-effective storage access tier. 
    - Reduced Redundancy Storage (RRS): Designed for non-critical, reproducible data that can be stored with less redundancy than the STANDARD storage class. 
    - S3 Standard-IA: Designed for long-lived and infrequently accessed data, Multi AZ 
    - S3 OneZone-IA: Designed for long-lived and infrequently accessed data, Single AZ 
    - S3 Glacier: Use for archives where portions of the data might need to be retrieved in minutes. 
    - S3 Glacier Deep Archive: Use for archiving data that rarely needs to be accessed. 
    - S3 Outposts

### Data Encryption in S3
- Data while in-transit (as it travels to and from Amazon S3) 
    - Use Secure Sockets Layer (SSL) or Client-Side Encryption 
- At rest (while it is stored on disks in Amazon S3 data centers) 
    - Server-Side Encryption: Request Amazon S3 to encrypt your object before saving it on disks in its data centers and then decrypt it when you download the objects. 
        - Server-Side Encryption with Amazon S3-Managed Keys (SSE-S3) 
        - Server-Side Encryption with Customer Master Keys (CMKs) Stored in AWS Key Management Service (SSE-KMS) 
        - Server-Side Encryption with Customer-Provided Keys (SSE-C) 
    - Client-Side Encryption: Encrypt data client-side and upload the encrypted data to Amazon S3. In this case, you manage the encryption process, the encryption keys, and related tools. 
        - Use a customer master key (CMK) stored in AWS Key Management Service (AWS KMS) 
        - Use a master key you store within your application.

### Amazon Route 53
- Amazon Route 53 is a highly available and scalable Domain Name System (DNS) web service. You can use Route 53 to perform three main functions in any combination. 
1. Register Domain Names: Route 53 lets you register a name for your website or web application, known as a domain name. 

2. Route Internet Traffic: to the resources for your domain When a user opens a web browser and enters your domain name (example.com) subdomain name (apex.example.com) in the address bar, Route 53 helps connect the browser with your website or web application. 

3. Check the Health of your resources Route 53 sends automated requests over the internet to a resource, such as a web server, to verify that it's reachable, available, and functional.

### Amazon CloudFront
- Amazon CloudFront is a web service that speeds up distribution of your static and dynamic web content such as .html .css, .js and image files to your users. 
- CloudFront delivers your content through a worldwide network of data centers called edge locations. 
- When a user requests content that you're serving with CloudFront, the user is routed to the edge location that provides the lowest latency (time delay), so that content is delivered with the best possible performance. 
- If the content is already in the edge location with the lowest latency. CloudFront delivers it immediately. 
- If the content is not in the edge location, CloudFront retrieves it from an origin that you've defined - such as an Amazon S3 bucket, a MediaPackage channel, or an HTTP server (for eample, a web server) that you have identified as the source for the definitive version of your content.

### AWS Global Accelerator
- A networking service that improves the performance of your users' traffic by up to 60% using Amazon Web Services' global network infrastructure. 
- Internet could be congested and slower, AWS Global Accelerator optimizes the path to your application to keep packet loss, jitter, and latency consistently low. 
- Provided with two global static public Ips that act as a fixed entry point to your application, improving availability.

### Amazon S3 Glacier
#### What is Amazon S3 Glacier? 
- Amazon S3 Glacier is an extremely low-cost storage service that provides secure, durable, and flexible storage for long-term data backup and archival. 
- In fact, a very high percentage of the data stored in Amazon Glacier today comes directly from customers using S3 Lifecycle policies to move cooler data into Amazon Glacier. 
- Now, Amazon Glacier is officially part of S3 and will be known as Amazon S3 Glacier (S3 Glacier). 

#### Glacier direct APIs 
- All of the existing Glacier direct APIs continue to work just as they have, but AWS now made it even easier to use the S3 APIs to store data in the S3 Glacier storage class. 

#### Availability and Durability 
- Amazon S3 Glacier is designed to provide average durability of 99.999999999999% for an archive. 
- Amazon S3 Glacier is designed for 99.99% availability and is backed by the Amazon S3 Service Level Agreement. 

#### Store using Amazon S3 Glacier? 
- There is no minimum and maximum limit to the total amount of data that can be stored in Amazon S3 Glacier. Individual archives can be from 1 byte to 40 terabytes.

#### Billing 
- With Amazon S3 Glacier, storage is priced from $0.004 per gigabyte per month, and you pay for what you use.

### AWS Snowball
#### What is Snowball?
- Snowball is a petabyte-scale data transport solution that uses secure appliances 
- Physical devices to transfer large amounts of data into and out of the AWS cloud. 
- Using Snowball addresses common challenges with large-scale data transfers including high network costs, long transfer times, and security concerns. 

#### Why to use Snowball? 
- Simple, fast, secure (256-bit encryption), and highly scalable. 
- Saves cost! As little as 1/5th the cost of transferring data via high-speed internet. 
- Addresses concerns of high network costs, long transfer times, and security No need to write any code. 
- No hardware purchase necessary. Common use cases: Migration of analytics data, genomics, data, video libraries, image repositories, backup, and to archive part of data center shutdowns, tape replacement or application migration projects. 
- Features: 
    - Rain and dust Resistant 
    - Tamper-resistant case 
    - Sizes: 50TB and 80TB 
    - 10G Network 
    - All data encrypted end to end

### Amazon Elastic File System (EFS)
#### What is Amazon Elastic File System?
- Provides simple, serverless, set-and-forget, scalable elastic file storage/system that makes it easy to set up, scale, and cost-optimize file storage in the AWS Cloud.
- Storage capacity is elastic, growing and shrinking automatically as you add and remove files, so your application have the storage they need, when they need it.
- Amazon EFS supports the Network File System version 4 (NFSv4.1 and NFSv4.0) protocol.
- The service is designed to be highly scalable, highly available, and highly durable.
- User can enable encryption is transit and encryption at rest.
- Desined to provide 99.99999999999% (11 9's) of durability over a given year.
- Performance Modes:
    - General Purpose
    - Max I/O
- Throughput Modes
    - Bursting
    - Provisioned
- Amazon Elastic File System (EFS) | Cloud File Storage | FAQs


- Storage: Data is stored in a single region and replicated across multiple availability zones.
- Accessibility: Data is accessible across availability zones within same region.
- Data Access: Thousands of Amazon EC2 instances, from multiple AZ's can connect concurrently to a file system.
- Access Control: Security Groups and IAM User Authentication
- AZ Failure: Can withdstand
- Cost: Pay only for file system storage you use per month
- Use Cases: Big data and analytics, media processing workflow, content management, web serving and home directories.

### AWS Storage Gateway
#### What is Storage Gateway?
- It is a set of hybried cloud services that give you on-premises access to virtually unlimited cloud storage.
- Provides a standard set of storage protocols such as iSCI, SMB, and NFS, which alllow you to use AWS storage without rewriting your existing applications.
- Customers use Storage Gateway to integrate AWS Cloud Storage with existing on-site wokrloads so they can simplify storage management and reduce costs for key hybrid cloud storage use cases.
- These include moving backups to the cloud, using on-premises file shares backed by cloud storage, and procviding low latency access to data in AWS for on-premises applications.

#### Why should I use AWS Storage Gateway?
- Storage Gateway enables you to reduce your on-premises storage footprint and associated costs by leveraging AWS cloud storage device.

#### What use cases does AWS Storage Gateway support?
- Storage Gateway supports four key hybrid use cases:
    1. Move backups and archives to the cloud
    2. Reduce on-premises storage with cloud-backed file shares.
    3. Provide on-premises applications low latency access to data stored in AWS.
    4. Data like access for pre and post processing workflow.

### Serverless Architecture
### AWS Lambda
- AWS Lambda is a compute service that lets you run code without provisioning or managing servers.
- AWS Lambda executed your code only when needed and scales automatically, from a few requests per day to thousands per second.
- With AWS Lambda, you can run code for virtually any type of application or backend service: all with zero administration.
- All you need to do is supply your code in one of the languages that AWS Lambda supports (currently supporting AWS Runtimes (many versions)): Node.js, Java, Ruby, .NET, Go and Python)
- You can use AWS Lambda
    - to run your code in response to events, e.g. if there are changes to data in an Amazon S3 bucket or an Amazon DynamoDB table
    - to run your code in response HTTP requests using Amazon API Gateway.
    - to invoke your code using API calls made using AWS SDKs.

### Amazon API Gateway
- A fully managed service that makes it easy for devopers to create, publish, maintain, monitor, and secure APIs at any scale.
- Create REST and WebSocket APIs to access data, business logic, or functionality from your backend services.
- No minimum fees or startup costs; pay only for the API calls you receieve and the amount of data trasnferred out.

### AWS Elastic Beanstalk
- AWS Elastic Beanstalk is an easy-to-use service for deploying and scaling web applications and services developed with Java, .NET, PHP, Node.js, Python, Ruby, Go, and Docker on familiar servers such as Apache, Nginx, Passenger, and IIS. Simply upload your code and Elastic Beanstalk automatically handles the deployment: capacity provisioning, load balancing, auto-scaling application health monitoring etc. we also retain full control over the AWS resources powering the application and can access the underlying resources at any time. There is no additional charge for Elastic Beanstalk: one pays only for the AWS resources needed to store and run your applications.

### Amazon CloudWatch
- Amazon CloudWatch is a monitoring and observability service built for DevOps engineers, developers, site reliability engineers (SREs), and IT managers. 
- Provides you with data and actionable insights to monitor your applications, respond to system-wide performance changes, optimize resource utilization, and get a unified view of operational health. 
- Collects monitoring and operational data in the form of logs, metrics, and events, providing you with a unified view of AWS resources, applications, and services that run on AWS and on-premises servers. 
- Uses: 
    - Set alarms 
    - Visualize logs and metrics side by side T
    - ake automated actions Troubleshoot issues Detect Anomalous behavior in your environments Discover insights to keep your applications running smoothly

### AWS CloudTrail
- Enables governance, compliance, operational auditing, and risk auditing of your AWS account.
- Log, continuously monitor, and retain account related to actions across your AWS infrastructure. 
- CloudTrail provides event history of your AWS account activity, including actions take through the AWS Management Console, AWS SDKs, command line tools, and other AWS services. 
- Also helps detect unusual activity in your AWS accounts

### AWS Config
Enables you to assess, audit, and evaluate the configurations of your AWS resources. 
- Log, continuously monitor, and retain account activity related to actions across your AWS infrastructure. 
- Review changes in configuration and relationships between AWS resources Check detailed resource configuration histories Find out overall compliance against the configurations specified in your internal guidelines

### AWS CloudFormation
- A service that helps you model and set up your Amazon Web Services resources so that you can spend less time managing resources and more time focusing on your applications that run in AWS. 
- With AWS CloudFormation you can: 
    - Simplify Infrastructure Management 
    - Quickly Replicate Your Infrastructure Easily Control and Track Changes to Your Infrastructure

### Amazon SNS
- It is a fully managed messaging service for both application-to-application (A2A) and application-to-person (A2P) communication. 
- There are two types of clients: publishers and subscribers: also referred to as producers and consumers. 
- Using Amazon SNS topics, publisher systems can fanout messages to a large number of subscriber systems including Amazon SQS queues, AWS Lambda functions and HTTPS endpoints, for parallel processing, and Amazon Kinesis Data Firehouse. 
- The A2P functionality enables you to send messages to users at scale via SMS, mobile, push, and email. 
- Two types of topics: 
    - Standard: give maximum throughput, best-effort ordering, best-effort deduplication, multiple subscription types, message fanout. 
    - FIFO: high throughput, strict ordering, strict deduplication, SQS FIFO subscriptions, message fanout
### Amazon SQS
- Amazon Simple Queue Service 
- Fully managed message queuing service that enables you to decouple and scale microservices, distributed systems, and serverless applications. 
- SQS eliminates the complexity and overhead associated with managing and operating message oriented middleware, and empowers developers to focus on differentiating work. 
- Using SQS, you can send, store and receive messages between software components at any volume, without losing messages or requiring other services to be available. 
- Two types of message queues: 
    - SQS Standard queues: give maximum throughput, best-effort ordering, and at-least-one delivery. 
    - SQS FIFO queues: designed to guarantee that messages are processed exactly once, in the exact order that they are sent.

### Amazon Rekognition
- Amazon Rekognition makes it easy to add image and video analysis to your applications. 
- Just provide an image or video to the Rekognition API, and the service can identify the objects, people, text, scenes, and activities, as well as detect any inappropriate content. 
- Amazon Rekognition also provides highly accurate facial analysis and facial recognition on images and video that you provide. 
- You can detect, analyze, and compare faces for a wide variety of user verification, people counting, and public safety use cases.

### Amazon Elastic Container Service (ECS)
#### What is a Docker? 
- Docker is a tool designed to make it easier to create, deploy, and run applications by using containers. 
- Containers allow a developer to package up an application with all the parts it needs, such as libraries and other dependencies, and deploy it as one package. 
- By doing so, the application will run on any other Linux machine regardless of any customized settings that machine might have that could differ from the machine used for writing and testing the code. 

#### What is Amazon ECS? 
- Amazon ECS manages containers and allows developers to run applications in the cloud without having to configure an environment for the code to run in. 
- It enables developers with AWS accounts to deploy and manage scalable applications that run on groups of servers called clusters through application program interface (API) calls and task definitions. 
- Amazon ECS enables developers to easily use Docker containers for a range of activities; from hosting a simple website to running complex, distributed microservices that require thousands of containers.

### Amazon Elastic Kubernetes Service (EKS)
#### What is Amazon Elastic Kubernetes Service (Amazon EKS)? 
- It is a managed service that gives you the flexibility to start, run and scale Kubernetes applications on AWS without needing to install and operate your own Kubernetes applications control plane or worker nodes in the AWS Cloud or on-premises. 
- It helps you provide highly available and secure clusters and automates key tasks such as patching, node provisioning, and updates. 
- EKS runs upstream Kubernetes and is certified Kubernetes conformant for a predictable experience. 
- It supports Linux x86 ARM, and Windows Server operating system distributions that are compatible with Kubernetes. 
- It provides optimized AMIs for Amazon Linux 2 and Windows Server 2019.

#### Pricing and Availability 
- You pay $0.10 per hour for each Amazon EKS cluster that you create and for the AWS resources create to run your Kubernetes worker nodes. 
- Please visit the AWS global Infrastructure Region table for the most up-to-date information on Amazon EKS Regional availability.

________________________________________________________

# Module 4
## Key Principles of AWS Pricing
### How pricing in AWS works - Fundamentals
### Cost Optimization
### Power of Flexibility
### Choose the right AWS Pricing Model
### AWS FRee Tier
### Estimate your Bill
### AWS Pricing Philosophy
### AWS Account Structure - AWS Organizations
### AWS Best Practices - Architecting for the Cloud
### AWS Support
### Sources of documentation or technical assistance
### AWS Cost Estimation
### AWS Pricing API
### Cost Explorer
### AWS Cost and Usage Report
### AWS Service Health Dashboard
### The role of the Concierge for AWS Enterprise SUpport Plan Customers
### Premium Support
### AWS MarketPlace Tools
### Support Cases
### AWS Account Clousre/Deactivation (after training completes)
### 
