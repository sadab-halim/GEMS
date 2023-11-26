
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
### Amazon Virtual Private Cloud (VPC)
### Amazon Elastic Compute Cloud (EC2)
### Amazon Elastic Block Store (EBS)
### Elastic Load Balancing (ELB)
### AWS Auto Scaling
### Amazon Aurora
### Amazon Relational Database Servcie (RDS)
### Amazon DynamoDB
### Amazon RedShift
### Amazon EMR
### Amazon Athena
### AWS Glue
### Amazon QuickSight
### Some Storage Services from AWS
### Amazon Simple Storage Service (S3)
### S3 Storage Classes
### Data Encryption in S3
### Amazon Route 53
### Amazon CloudFront
### AWS Global Accelerator
### Amazon S3 Glacier
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
