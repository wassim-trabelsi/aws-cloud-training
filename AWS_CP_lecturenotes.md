# Udemy AWS Certified Cloud Practitioner

## Section 3 : What is Cloud Computing ?

Cloud computing is the *on-demand delivery* of compute power, database storage, applications, and other IT resources.

*Pay-as-you-go* pricing

5 characteristics of cloud computing : 

1. On-demand self service
2. Broad network access
3. Multi tenancy and resource pooling
4. Rapid elasticity and scalability
5. Measured service


6 advantages of Cloud Computing : 

1. Trade capital expense (Capex) --> Operational expense (Opex)
2. Benefit from massive scale
3. Stop guessing capacity
4. Increase speed and agility
5. Dont spend in data centers
6. Go global in minutes

Problems solved by the Cloud : 
1. Flexibility
2. Cost effective
3. Scalability
4. Elasticity
5. High availability and fault tolerance
6. Agility

Types of Cloud computing : 

0. On permise (Manage all)
1. IaaS (Network + Computer) -- Example :  Amazon EC2
2. PaaS (Application) -- Example Elastic Beanstalk
3. SaaS (Completed product) --
Example Rekognition for Machine Learning

Pricing of the cloud : 

1. Compute : Pay for compute time
2. Storage : Pay for data stored in the Cloud
3. Data transfer : Volume OUT of the Cloud


How to choose an AWS Region ? 

1. Compliance
2. Proximity
3. Available Services
4. Pricing

AWS has 216 points of presence

Somes services are Global:

1. IAM
2. Route 53
3. CloudFront
4. WAF

Most services are Region-scoped: 

1. Amazon EC2
2. Elastic Beanstalk
3. Lambda

Shared Responsability Model diagram 

- Customer = Responsibility for the security **IN** the cloud

- AWS = Responsibility for the security **OF** the cloud

## Section 4 : IAM - Identity and Access Management

IAM Policies Structure consists of :

1. Version : Policy language version, always include "2012-10-17"
2. Id: An identifier for the policy (optional)
3. Statement: one or more individual statements (required)

Statements constists of :

1. Sid: an identifier for the statement (optional)
2. Effect: whether the statement allows or denies access (Allow,Deny)
3. Principal: account/user/role to which this policy applied to
4. Action: list of actions this policy allows or denies
5. Resource: list of resources to which the actions applied to
6. Condition: conditions for when this policy is in effect (optional)

MFA devices:

(Virtual MFA device) : Support for multiple tokens on a single device
1. Google Authenticator
2. Authy (multi-device)

(Universal 2nd Factor (U2F) Security Key)
1. YubiKey by Yubico (3rd party)
2. Hardware Key Fob MFA Device (Gemalto or SurePassId)


Setup everything from command line code with aws-cli :

```shell
my@computer:~$ aws configure
AWS Access Key ID [None]: MY_ACCESS_KEY
AWS Secret Access Key [None]: MY_SECRET_ACCESS
Default region name [None]: eu-west-1
Default output format [None]: 
```

```shell
me@computer:~$ aws iam list-user
{ 
    "Users": [
        {
            "Path": "/",
            "UserName": "MY_NAME",
            "UserId": "MY_USER_ID",
            "Arn": "arn:aws:iam::13DIGIT:user/MY_NAME",
            "CreateDate": "CreateDate"
            "PasswordLastUsed": "PwdDate"
        }
    ]
}
```

An alternative to aws-cli is AWS CloudShell (CloudShell is a shell directly on the cloud)

Running this command will create permanently a demo.txt file into my cloudshell environement.

```shell
cloudshell-user@ip:~$ echo "test" > demo.txt
```

IAM Roles for Services :

1. Some AWS service will need to perform actions on your behalf
2. To do so, we will assign permissions to AWS services with IAM Roles
3. Common roles:
    - EC2 Instance Roles
    - Lambda Function Roles
    - Roles for CloudFormation

IAM Security Tools:

1. IAM Credential Report (account-level)
    - A report that lists your account's users and the status of their various credentials
2. IAM Access Advisor (user-level)
    - Access advisor shows the service permissions granted to a user and when those services were last accessed
    - You can use this information to revise your policies


## Section 5 : EC2 - Elastic Compute Cloud

Amazon EC2:

1. EC2 is one of the most popular of AWS offering
2. EC2 = Elastic Compute Cloud = Infrastructure as a Service
3. It mainly consists in the capability of:
    - Renting virtual machines (EC2)
    - Storing data on virtual drives (EBS)
    - Distributing load across machines (ELB)
    - Scaling the services using an auto-scaling group (ASG)
4. Knowing EC2 is fundamental to understand how the Cloud works

EC2 sizing & configuration options:

1. OS can be Lunux Windows or MAC OS
2. CPU
3. RAM
4. Storage
    - Network attached (EBS & EFS)
    - hardware (EC2 instance Store)
5. Network card: speed of the card, Public IP address
6. Firewall rules: security group
7. Bootstrap script (configure at first launch): EC2 User Data

EC2 User Data

1. It's possible to bootstrap our instances using an EC2 User data script
2. bootstrapping means launching commands when a machine start
3. That script is only run once at the instance first start
4. EC2 user data is used to automate boot tasks such as:
    - Installing software
    - Installing updates
    - Downloading common files from the internet
    - Anything you can think of
5. The EC2 User Data Script runs with the root user

EC2 Instance Types - Overview naming convention

1. m: instance class
2. 5: generation
3. xlarge: size within the instance class

EC2 Istance Types :

1. General purpose
    - Great for a diversity of workloads such as web servers or code repositories
    - Balance between Compute/Memory/Network
    - Example : t2.micro

2. Compute optimized
    - Great for CPU intensive workloads such as batch processing, video encoding, high performance web servers, Machine learning, etc
    - High CPU to RAM ratio
    - Example : c5.large

3. Memory optimized
    - Fast perfomance for workloads that process large data sets in memory
    - Great for memory intensive workloads such as in-memory databases, distributed caches, etc
    - High RAM to CPU ratio
    - Example : r5.xlarge

4. Accelerated computing
    - Great for graphics intensive workloads such as 3D visualisation, video transcoding, etc
    - High CPU to RAM ratio
    - Example : g4dn.xlarge

5. Storage optimized
    - Great for large data set workloads such as data warehousing, log processing, etc
    - Usecases inclues : High frequency online transaction processing, Relational or NoSQL databases, Cache, Distributed file systems, etc
    - High disk throughput
    - Example : i3.xlarge

Table of comparaison : 

| Instance Type | vCPU | MEM (Gib) | Storage | Network Performance | EBS Bandwidth | 
| :--- | :---: | :---: | :---: | :---: | :---: |
| t2.micro | 1 | 1 | EBS only | Low to Moderate | Up to 500 Mbps |
| t2.xlarge | 4 | 16 | EBS only | Low to Moderate | Up to 500 Mbps |
| c5d.4xlarge | 16 | 32 | 1 x 400 Gib SSD | Moderate | 4,750 |
| r5.16xlarge | 64 | 512 | EBS only | 20 Gigabit | 13,600 Mbps |
| m5.8xlarge | 32 | 128 | EBS only | 10 Gigabit | 6,800 Mbps |

Security Groups : 

1. Security groups are the fundamental of network security in AWS
2. They control the traffic for one or more EC2 instances
3. Security groups only contain allow rules
4. Security groups can reference by IP or by security group
5. Acting as a virtual firewall for your instance to control inbound and outbound traffic
6. They regulate :
    - Access to ports
    - Access to protocols
    - Authorised IP ranges
    - Control of network traffic
    

Security Groups - Good to know

1. Can be attached to multiple EC2 instances
2. Locked down to a region/VPC combination
3. Does live outside the EC2 instance
4. It's goot to maintain one seperate security group for SSH access
5. If your application is not accessible (timeout), check your security group rules
6. Inbound traffic is blocked by default
7. Outbound traffic is allowed by default
8. Can reference other security groups

Classic Ports to know:

1. Port 22 : SSH (Secure Shell) - log into a Linux instance
2. Port 21 : FTP (File Transfer Protocol) - transfer files between computers
3. Port 80 : HTTP (Hypertext Transfer Protocol) - web traffic
4. Port 443 : HTTPS (HTTP Secure) - secure web traffic
5. Port 22 : SFTP (Secure File Transfer Protocol) - upload files into a file share
6. Port 3389 : RDP (Remote Desktop Protocol) - log into a Windows instance

EC2 Instance Purchasing Options:

1. On demand
    - Pay for what you use
    - Short workloads, predictable pricing

2. Reserved
    - Up to 75% off on-demand
    - Long workloads, steady state or predictable usage
    - You can reserve instances for 1 or 3 years
    - You can make partial upfront, all upfront or no upfront payments
    - You can choose between standard or convertible reserved instances

3. Savings Plans
    - Commit to a consistent amount of usage
    - Long workloads
    - You can save up to 72% compared to on demand (Commit to USD per month)

4. EC2 Spot Instances
    - Can be up to 90% cheaper than on demand
    - Bid for unused EC2 capacity
    - Great for flexible start and end times or for applications that are only feasible at very low compute prices
    - You can lose your instance at any time
    - NOT SUITED FOR CRITICAL APPLICATIONS OR DATABASES

5. Dedicated Hosts
    - Physical EC2 server dedicated for your use
    - Great for regulatory requirements or licensing that does not support multi-tenancy
    - Purchasing options = On demand or Reservation
    - The most expensive EC2 purchasing option
    - Useful for licensing that have complicated licensing models (BYOL)
    - Or for companies that have strong regulatory or compliance needs

6. Dedicated Instances
    - EC2 instance on a physical server dedicated for your use
    - May share hardware with other instances in same account
    - No control over instance placement 

7. Capacity Reservations
    - Reserve capacity for your AWS account
    - Great for applications that have specific capacity requirements
    - Suitable for short term uninterrupted workloads that needs to be in a specific AZ

## Section 6: EC2 Instance Storage

EBS - Elastic Block Store

1. EBS is a network drive that can be attached to one or more EC2 instances
2. Can be mounted to one instance at a time
3. They are bound to a specific AZ
4. It's a network drive, so it's slower than an instance store
5. Have a provisioned Capacity (size)

EBS Snapshots

1. Make a backup of your EBS volume
2. Not necesssary to detach the volume from the instance (but recommended)
3. Can copy snapshots across AZ and regions
4. Features :
    - Move to archive tier that is 75% cheaper (24h to restore the snapshot)
    - Setup rules to retain deleted snapshots
    - Specify retention period (from a day to a year)

AMI Overview

1. AMI stands for Amazon Machine Image
2. It's a template for your EC2 instance (OS, Application, Permissions, etc)
3. AMI are build for a specific region
4. You can launch EC2 instances from :
    - Public AMI
    - Private AMI
    - Marketplace AMI (AWS Marketplace)

EC2 Image builder

1. It's a service that makes it easy to create, secure and manage custom AMI
2. Can be run on a schedule
3. Free service

EC2 Instance Store

1. EBS Volumes are network drives with good but limited performance
2. EC2 Instance Store are physical drives attached to the host
3. EC2 Instance Store lose their storage when the instance is stopped
4. Good for buffer/cache/temporary storage
5. Not a good choice for databases

EFS - Elastic File System

1. EFS is a network file system that can be mounted on multiple EC2 instances
2. EFS works with Linux EC2 instances in multiple AZ
3. Highly available, scalable, expensive (3x gp2), pay per use, no capacity planning

EFS Infrequent Access (EFS IA)

1. Storage cost optimised for files not accessed every day
2. Up to 20% cheaper than EFS
3. EFS will automatically move files to IA after 60 days of inactivity (need to enable lifecycle policy)

Amazon FSx - Overview

1. Launch 3rd party file systems on AWS (fully managed service)
2. Built on Windows File Server
3. Supports SMB and NTFS  
4. Integrated with Active Directory

Amazon FSx for Lustre

1. For High Performance Computing (HPC)
2. Linux based
3. Machine Learning, Analytics, Video Rendering, etc
4. Scales up to 100s of GB/s and millions of IOPS

## Section 7: ELB & ASG - Elastic Load Balancer & Auto Scaling Group

High Availability

1. High Availability usually goes hand in hand with horizontal scaling
2. High Availability means running your application in multiple AZ
3. The goal is to have your application running in multiple AZ in case one AZ goes down (disaster)

High Availability & Scalability for EC2

1. Vertical Scaling : Scale up your EC2 instance = Increase the size of your EC2 instance
    - From : t2.nano - 0.5 Go RAM - 1 CPU
    - To : u-12tb1.metal - 12.3 TB of RAM -  448 CPU

2. Horizontal Scaling : Scale out your EC2 instance = Add more EC2 instances
    - Auto Scaling Group
    - Load Balancer

3. High Availability : Run your application in multiple AZ
    - Multi AZ
    - Load Balancer

Formal definitions :

1. Scalability : The ability to accommodate a larger load by making the hardware stronger (scale up), or by adding nodes (scale out)

2. Elasticity : Once you have a scalable system, elasticity means that there will be some 'auto-scaling' mechanism that will automatically scale up or down based on the load

3. Agility : New IT Ressources are only a click away

Load Balancer:

1. Load balancers are servers that forward internet traffic to multiple EC2 instances downstream
2. Why to use ? :
    - Spread load across multiple downstram instances
    - Expose a single point of access (DNS) to your application
    - Seamlessly handle failures of downstream instances
    - Do regular health checks on downstream instances
    - Provide SSL termination for your website
    - High availability across multiple AZ
3. An ELB is a managed load balancer:
    - AWS guarantees that it will work
    - AWS takes care of upgrades, maintenance, high availability, etc
    - AWS provides only a few configuration knobs
4. It cost less to setup your own load balancer (NGINX, HAProxy, etc) but you have to manage it yourself
5. 4 types of load balancers :
    - Application Load Balancer (HTTP/HTTPS/gRPC) -- Static DNS (URL) (Layer 7)
    - Network Load Balancer (ultra high performance, allow for TCP) -- Static IP through Elastic IP (Layer 4)
    - Classic Load Balancer (Layer 4)
    - Gateway Load Balancer -- GENEVE Protocol on IP Packets ; Route Traffic to Firewalls ; Intrusion detection(Layer 3)

Auto Scaling Group : 

1. In real live, the load can change
2. In the cloud, you can get rid of servers very quickly
3. The goal of an Auto Scaling Group (ASG) is to:
    - Scale out
    - Scale in
    - Ensure we have a minimum and a maximum of machines running
    - Automatically register new instances to a load balancer
    - Replace unhealthy instances
4. Huge Cost Savings

Auto Scaling Group Strategies : 

1. Manual Scaling (Update manually)
2. Dynamic Scaling
    -  Simple / Step Scaling : When a cloud watch alarm is triggered (ex. CPU > 70%)
3. Target Tracking Scaling
    - Example : I want the average ASG CPU to stay at around 40%
4. Scheduled Scaling
    - Anticipate a scaling based on known usage patterns
    - Example increast the min capacity
5. Predictive Scaling : Based on Machine Learning


## Section 8 : Amazon S3

S3 Use cases :
1. Backup & Storage
2. Disaster Recovery
3. Archive
4. Hybrid Cloud Storage
5. Application hosting
6. Media hosting
7. Data lakes & big data analytics
8. Software delivery
9. Static website

S3 - Buckets:

1. Amazon S3 allows peoples to store objects (files) in "buckets" (directory)
2. Buckets must have a globally unique name (across all regions all accounts)
3. Buckets are defined at a region level
4. S3 looks like a global service but buckets are created in a region
5. Naming convention : 
    - No uppercase, No underscore
    - 3-63 char long
    - not an ip
    - Must start with lowercase letter or number
    - NOT START WITH (xn--)
    - NOT END WITH (-s3alias)

S3 - Objects : 

1. Key = FULL PATH :
    - s3://my-bucket/my_file.txt
    - s3://my-bucket/my_folder1/another_folder/my_file.txt

2. The key is composed of prefix (my_folder1/another_folder/) + object name (my_file.txt)

3. There is no concept of "directories" within buckets (although the UI will trick you yo think otherwise)

4. Just keys with very long names that contain slashes

Amazon S3 - Objects (cont.):

1. Object values are the content of the body:
    - Max. Object Size is 5TB
    - If uploading more than 5GB - use multi part upload
2. Metadata (list of text key / value pairs - system or user metadata)
3. Tags (Unicode key / value pair - up to 10) - useful for security / lifecycle
4. Version ID (if versioning is enabled)

Amazon S3 - Security :

1. User-Based:
    - IAM Policies - which API calls should be allowed for a specific user from IAM

2. Resource-Based
    - Bucket Policies - bucket wide rules from the S3 console - allows cross account
    - Object Access Control List (ACL) - finer grain
    - Bucket Access Control List (ACL) - less common (can be disabled)

3. Note: an IAM principal can access an S3 object if 
    - The user IAM permissions allow it OR the resoure policy ALLOWS it
    - AND no explicit DENY

4. Encryption: encrypt objects in Amazon S3 using encryption keys

S3 Bucket Policies : 

1. JSON based policies
    - Resources: buckets and objects
    - Effect: Allow/Deny
    - Actions: Set of API to Allow or Deny
    - Principal: The account or user to apply the policy to

2. Use S3 bucket for policy to: 
    - Grant public access to the bucket
    - Force objects to be encrypted at upload
    - Grant access to another account (Cross Account)

Amazon S3 - Static Website Hosting

1. S3 can host static websites and have them accessible on the Internet
2. The website URL will be http://bucket-name.s3-website-aws-region.amazonaws.com

Amazon S3 - Versioning 

1. You can version your file in Amazon S3
2. It's enabled at the bucket level
3. Same key overwrite will change the "version": 1 2 3
4. It's best practice to version your bucket:
    - Protect against unintended delete
    - Easy rollback
5. Notes: 
    - Any file that is not versioned prior to enabling versioning will have a version "null"
    - Suspending versioning doesn't delete the previous versions

Amazon S3 - Replication (CRR & SRR) --> Want to setup an asynchronous replication

1. Must enable Versioning in source and destination buckets
2. Cross Region Replication (CRR)
    - Asynchronous replication
    - Can replicate to another account
3. Same Region Replication (SRR)
    - Synchronous replication
4. Must give IAM permissions to S3 service

Use cases: 

1. CRR - compliance, lower latency, replication across accounts
2. SRR - log aggregation, live replication between dev and prod

S3 Storage Classes :

1. S3 Standard - General Purpose
    - 99.99% availability
    - Used for frequently accessed data
    - Low latency
    - Sustainable 2 concurrent facility failure
    - Uses cases : Big Data Analytics, mobile & gaming app, Content Distribution, 
2. S3 Standard - Standard-Infrequent Access (IA)
    - For data that is less frequently accessed
    - Lower cost than S3 Standard
    - 99.9% availability
    - Uses cases : Backup, Disaster Recovery
3. S3 One Zone - IA
    - High durability 99.999999999% (11 9's) in a single AZ; data is lost when AZ is destroyed
    - 99.5% availability
    - Use Cases : Storing secondary backup copies of on-premises data
4. S3 Glacier Instant Retrieval
    - Low cost object storage
    - Pay for storage and retrieval cost
    - Millisecond retrieval
    - Minimum storage duration of 90 days
5. S3 Glacier Flexible Retrieval
    - Expedited (1-5 minutes), Standard (3-5 hours), Bulk (5-12 hours) - free
    - Minimum storage duration of 90 days
6. S3 Glacier Deep Archive
    - Long term storage
    - Standard (12 hours), Bulk (48 hours)
    - Minimum storage duration of 180 days
7. S3 Intelligent Tiering
    - Small monthly monitoring and auto-tiering fee
    - Moves objects automatically between Access Tier based on usage
    - There are no retrieval charges in S3 Intelligent Tiering


Can move between classes manually or using S3 Lifecycle configurations

Durability and Availability :

1. Durability:
    - High durability - 99.999999999% (11 9's)
    - Data is stored in 3 AZ
    - If you store 10.000.000 objects == 1 Loss in 10.000years
    - Same for all storage classes
2. Availability:
    - Measures how readily available a service is
    - Varies depending on storage class
    - S3 Standard - 99.99% (4 9's) = Not available 53 minutes per year

S3 Encryption :

1. Server Side Encryption (SSE-S3)
    - Managed by Amazon S3
    - AES-256 encryption
    - Key is managed by Amazon S3
    - S3 Managed Keys (SSE-S3)

2. Client Side Encryption (SSE-C)
    - Managed by the customer
    - Client encrypts the data


AWS Snow Family = Offline devices to perform data migration

1. Highly-secure, portable storage devices to collect and process data at the edge, and migrate data into and out of AWS
2. Data migration:
    - Snowcone
    - Snowmobile
    - Snowball Edge
        - 80TB (or 42) of HDD capacity for block volume
3. Edge computing:
    - Snowcone
    - Snowball Edge
4. OpsHub - Management console for Snowball Family

Hybrid Cloud Storage:

Use AWS Storage Gateway to connect on-premises applications to cloud-based storage

## Section 9: Databases & Analytics

Services:
1. RDS (Relational Database Service)
    - Suited for OLTP (Online Transaction Processing)

2. Aurora
    - MySQL and PostgreSQL compatible
    - 5x faster than MySQL
    - 3 copies of data across 3 AZ
3. ElastiCache
    - Get managed Redis or Memcached
    - In-memory cache
4. DynamoDB
    - NoSQL family (Json, Key-Value, Document, Wide Column)
    - Tables can be global
    - Exclusive feature DAX (DynamoDB Accelerator)
5. Redshift
    - Based on PostgreSQL
    - Columnar storage
    - Massively Parallel Processing (MPP)
    - Data Warehouse
    - Load the data 10x faster
6. Neptune
    - Graph Database
7. DocumentDB
    - MongoDB compatible
    - Fast, scalable, highly available
    - Managed MongoDB
8. Athena
    - Serverless interactive query service
    - Query data in S3 using standard SQL
    - Pay per query
9. EMR (Elastic Map Reduce)
    - Elastic Map Reduce
    - 100 of EC2 instances
    - Big Data Analytics
10. QuickSight
    - Serverless BI
11. Glue
    - Serverless ETL (Extract, Transform, Load) to prepare and load data for analytics
    - Fully serverless
    - Glue Data Catalog
12. QLDB (Quantum Ledger Database)
    - Centralized ledger for tracking changes to data over time
    - Immutable, append-only, cryptographically verifiable
13. Managed Blockchain
    - Create and manage blockchain networks using open source frameworks
    - Hyperledger Fabric and Ethereum
13. DMS (Database Migration Service)
    - Migrate databases to AWS
    - Supports on-premises and cloud databases
    - Supports homogeneous migrations such as Oracle to Oracle
    - Supports heterogeneous migrations such as Oracle to MySQL

## Section 10: Other Compute Services: ECS, Lambda, Batch, Lightsail

Run Docker containers:
1. ECS (Elastic Container Service)
    - You manage the EC2 instances
2. Fargate
    - Serverless
3. ECR (Elastic Container Registry)
    - Docker container registry = Store the images to run on ECS or Fargate

Lambda:
1. Serverless compute service
2. Event driven
3. Pay per request
4. Easy to scale
5. Supports Java, Node.js, Python, C#, Go, PowerShell
6. CRON jobs
    - Schedule Lambda functions to run on a schedule
7. Amazon API Gateway
    - Create REST APIs
    - Trigger Lambda functions


Amazon Lightsail: 
1. Easy alternative to EC2, RDS, ELB, Route53
2. Virtual server, storage, database, load balancer, DNS ... in the cloud


## Section 11: Deployment & Managing Infrastructure

1. CloudFormation
    - It's a declarative way of outlining your infrastructure, for any resources (most of them are supported)
    - For example within a CloudFormation Templates you can define:
        - a VPC
        - a security group
        - 2 EC2 instances
        - a Load balancer (ELB) in front of theses machine
    - Then CloudFormation will create all these resources for you in the right order
    - Benefits :
        - Infrastructure as code (No ressources are manually created)
        - Changes to the infrastructure can be tracked
        - Cost:
            - Each resources is tagged with a stack name (easy tracked)
            - You can do saving strategies (Delete Dev stack between 5PM and 8AM safely)
        - Productivity:
            - Ability to destroy and re-create an infrastructure on the cloud on the fly
            - Automated generation of Diagram for your templates
            - Declarative programming
    - Don't re-invent the wheel
2. AWS Cloud Development Kit (CDK)
    - Define your cloud infrastucture using familiar programming languages
3. AWS Elastic Beanstalk
    - Is a developer centric view of deploying an app
    - It's a PaaS (Platform as a Service)
    - Deployment strategy is configurable but performed by EBS
    - Just the application code is the responsibility of the developer
    - 3 architecture models:
        - Single instance (Good for de)
        - Load balanced (great for production or preprod web app)
        - Autoscaling (ASG only great for non-web apps in production) (workers, etc.)
4. AWS CodeDeploy
    - We want to deploy our application automatically
    - Works with EC2 Instances
    - Works with On-Premises Servers
    - Hybrid service
    - Servers/Instances must be provisioned and configured ahead of time with CodeDeploy agent
5. AWS CodeCommit
    - Github concurrent for AWS
6. AWS CodeBuild
    - Code building service in the cloud
    - Compiles source code, run tests, and produces packages that are ready to be deployed (by CodeDeploy for example)
7. AWS CodePipeline
    - Orchestrate the different steps to have the code automatically pushed to production
    - Basis for CI/CD 
    - CodeCommit, CodeBuild, CodeDeploy, Elastic Beanstalk
    - Fully managed and compatible with GitHub
8. AWS CodeArtifact (Private Package Manager)
    - pip, npm, maven, nuget, etc.
9. AWS CodeStar
    - Project management service (Overview of software development activities in one place)
    - Quick way to get started to correctly setup CodeCommit, CodeBuild, CodeDeploy, CodePipeline
10. AWS Cloud9
    - Cloud IDE
    - Develop, run, and debug code with just a browser
11. AWS Systems Manager (SSM)
    - Helps you manage your EC2 instances at scale
    - Another Hybrid AWS service
    - Get operational insights about the state of your infrastructure
    - Suite of 10+ products:
        - Do automatic patching and OS updates
        - Run commands across an entire fleet of servers
        - Store parameter configuration with the SSM Parameter Store
    - Works both with Windows and LinuxOS
    - SSM Session Manager
        - No SSH required
        - Can allow a user to connect to an EC2 instance
12. OpsWorks:
    - Chef and Puppet
    - It's an alternative to AWS SSM

## Section 12: Leveraging the AWS Global Infrastructure

Why Global Application ?:
- Deployed in multiple geographic regions
- On AWS that could be regions or Edge locations
- Decreaseds Latency
- Disaster Recovery
- Attack protection 

1. Global DNS: Route53
    - IT'as a managed Domain Name System (DNS) service
    - Collection of rules that translate human readable domain names into IP addresses
    - Most common: 
        - www.google.com -> 12.34.56.76 == A record (IPv4)
        - www.google.com -> 2001:0db8:85a3:0000:0000:8a2e:0370:7334 == AAAA record (IPv6)
        - search.google.com -> www.google.com == CNAME record (Alias)
        - example.com -> www.example.com == Alias
    - Routing policies:
        - Simple: Only 1 instance (no health check)
        - Weighted: Route traffic based on weights
        - Latency: Route traffic based on lowest latency
        - Failover: Route traffic based on health checks
2. Global Content Delivery Network (CDN): CloudFront
    - Improves read performance, content is cached at the edge
    - Improves users experience
    - 216 Points of Presence (PoP) around the world
    - DDOS protection with WAF And Shield
    - Enhanced security with OAC (Origin Access Control) for S3 buckets
    - Custom Origin (HTTP)
3. S3 Transfer Acceleration
    - Upload data to S3 over an optimized network path
4. AWS Global Accelerator
    - Route traffic to optimal AWS endpoints
5. AWS Outposts
    - Amazon on your premises
6. AWS WaveLength
    - Bring AWS to the edge of the 5G Network
7. AWS Local Zones
    - Place AWS compute Storage and database resources closer to your users
    - Extend your VPC to more locations

## Section 13: Cloud Integration

1. Amazon SQS (Simple Queue Service)
    - First service on the cloud
    - Decouple applications
    - Asynchronous messaging service
    - Default retention period: 4 days (max 14 days)
2. Amazon Kinesis = real-time big data streaming
3. Amazon SNS = Publication Notification Service
3. Amazon MQ = Managed Message Broker for RabbitMQ and ActiveMQ

## Section 14: Cloud Monitoring

1. AWS CloudWatch
    - Metrics for every services
    - Monitor CPU, memory, network, disk, usd, etc.
    - Metrics have timestamps
    - Can create dashboards
    - Metrics for EC2:  (every 5mins (or 1min if you enable detailed monitoring not free!))
        - CPUUtilization
        - Network
        - StatusCheck
    - Metrics for EBS:
        - Disk Read/Writes
    - Metrics for S3:
        - BucketSizeBytes
        - NumberOfObjects
        - AllRequests
    - Billing:
        - Total Estimated Charges
    - Service Limits:
        - How much you've been using a service API
    - Custom Metrics:
        - You can create your own metrics
    - Alarms:
        - You can create alarms based on metrics
        - You can create SNS topics to be notified when an alarm is triggered
        - You can create CloudWatch EventBridge to trigger Lambda functions when an alarm is triggered
2. CloudWatch Logs
    - CloudWatch Logs can collect logs from:
        - Elastic Beanstalk
        - ECS
        - Lambda
        - CloudTrail
        - CloudWatch
        - Route53 : Log DNS queries
    - Enable real-time monitoring of logs
    - Adjustable CloudWatch Logs retention period (1 day to 10 years)
    - Need to setup a cloudwatch agent on the EC2 instance to push logs toward CloudWatch Logs
    - CloudWatch log agent can be setup on-premises too
3. CloudWatch EventBridge
    - Schedule CRON jobs (scheduled scripts)
    - Trigger Lambda functions, send SNS messages, etc.
    - EventBridge can be used to trigger Lambda functions based on CloudWatch alarms
    - Can recieve events from:
        - AWS services
        - Custom applications
        - SaaS partner applications
4. AWS CloudTrail
    - Provides governance, compliance, and operational auditing of your AWS account
    - CloudTrail is enabled by default
    - Get an history of events / API calls made within your AWS
    - Send them To CloudWatch Logs or S3

5. X-Ray Overview
    - Debbuging tool
    - Visual analysis of our applications
    - Troubleshooting performance (bottlenecks)
    - Pinpoint service issues

6. Amazon CodeGuru
    - ML powered code review
    - Recommandation about performance, security, reliability, etc.
    - CodeGuru Profiler
        - Identify performance bottlenecks
        - Remove code inefficiencies
    - CodeGuru Reviewer
        - Identify security issues
        - Identify code quality issues
        - Identify code best practices

7. AWS Health Dashboard
    - Get notified about AWS service issues
    - Get notified about AWS service limits
    - Get notified about AWS service events

8. Personal Health Dashboard
    - AWS events that affect your infrastructure

## Section 15: VPC

VPC & Subnets Primers
- VPC = Virtual Private Cloud
- Subnets allow you to partition your network inside your VPC
- A public subnet is a subnet that has a route to the internet
- A private subnet is a subnet that does not have a route to the internet
- To define access to the internet, you need to define a route table

Internet Gateway helps your VPC to access the internet:
- NAT Gateway (AWS Managed) & NAT Instance (Self Managed) allow your instance in a private subnet to access the internet while keeping your instance private

1. NACL = Network ACL = First lign of defense:
    - Can have ALLOW and DENY rules
    - Are attached at he subnet level
    - Runes only inclue IP addresses
    - Stateless: If you allow traffic in, you don't automatically allow traffic out
2. Security Groups = Second line of defense:
    - Can have ALLOW rules only
    - Rules include IP addresses, ports, and protocols
    - A firewall that controls traffic to and from an EC2 instance
    - Stateful: If you allow traffic in, you automatically allow traffic out

VPC Flow Logs:
1. Log about IP traffic going in and out of your interfaces
    - VPC Flow Logs
    - Subnet Flow Logs
    - Elastic Network Interface ENI Flow Logs
2. Helps you monitor & troubleshoot network issues
3. Captures network information from AWS managed interfaces too: Elastic Load Balancer, ElastiCache, RDS, etc.
4. VPC Flow logs data can go to CloudWatch Logs or S3

VPC Peering = Connect two VPCs together:
1. Make them behave as if they were in the same network
2. Must not overlapping CIDR (IP address range)
3. VPC connection is not transitive

VPC Endpoints - Interface & Gateway:
- Connect your VPC to supported AWS services
- Private connection between your VPC and the service
- For S3 and DynamoDB = Gateway
- For other services = Interface

PrivateLink:
- Most secure and & scalable way to connect a service to 1000s of VPCs
- Does not require VPC peering, internet gateway, NAT, route table,...

Require a network load balancer (Service VPC) and ENI (Customer VPC).

= Network Load Balancer (NLB) + ENI (Elastic Network Interface)

Direct Connect & Site to Site VPN:
1. Connect on-premises network to AWS (goes over the public internet)
2. Direct Connect (DX) = Establish a dedicated network connection from your premises to AWS (private network) need a month
3. Needs a CGW (Customer Gateway) and a VGW (Virtual Private Gateway) to connect to your on-premises network

Client VPN:
1. Privately connect to the client VPN to your private network

Transit Gateway:
1. For having transitive peering between VPCs ==> Transit Gateway
2. Avoid complex routing
3. Works with Direct Connect, VPN Connections, and VPC Peering

## Seection 16: Security

DDoS protection:
1. Aws Shield Standard
    - Free
    - Protecting against network layer 3/4 attacks
2. AWS Shield Advanced
    - 24/7 DDoS protection (premium) 3000$/month
3. AWS WAF
    - Filter specific requests based on rules (Layer 7) - Rules are based on IP, URL, HTTP headers, etc.
4. CloudFront and Route53
    - Availability protection using global edge network
    - Combiened with AWS Shield, provides attack mitigation at the edge

Penetration testing:
1. No need prior approval for 8 services:
    - EC2, Nat Gateway, Elastic Load Balancer
    - Aurora
    - API Gateway
    - RDS
    - AWS Lambda
    - Amazon Lightsail
    - Elastic Beanstalk
    - CloudFront
2. Not allowed to:
    - DNS zone walking via Route53
    - DoS
    - Port, protocol, request flooding

For any other simulated events contact AWS support.

Encryption:
1. Encryption at rest:
    - On hard disk
2. Encryption in transit:
    - On transmission

AWS KMS:
1. Key Management Service  = AWS managed service
2. CloudHSM = Hardware Security Module = You manage the encryption keys

AWS Certificate Manager:
1. Let yout manage SSL/TLS certificates

AWS Secrets Manager:
1. Store, manage, and retrieve secrets

Artifact:
1. AWS Artifact = AWS managed service
2. Store and share documents

Guard Duty:
1. Threat discorvery to protect your AWS account and workloads
2. Use machine learning to analyze and identify potential threats

Amazon Inspector:
1. Analyse known vulnerabilities and recommend fixes (EC2, Container & Lambda)

AWS Config:
1. Monitor and record your AWS resource configurations = In compliance with defined rules

Macie:
1. Machine learning based security service
2. Alert on PII (Personally Identifiable Information) data

AWS Security Hub:
1. Centralized view of your security state

Amazon Detective:
1. Investigate security issues (Using ML and graph analysis)

Action that can be done only by the root account:
1. Change account setting
2. View certain tax invoices
3. Close account
4. Restore IAM User permissions
5. Change or cancer AWS support plan
6. Register oas a seller on AWS Marketplace
7. Configure an Amazon S3 bucket to enable MFA
8. Edit or delete an Amazon S3 bucket policy that includes an invalid VPC ID or VPC endpoint ID
9. Sign up for GovCloud (US)

## Section 18: Account Management, Billing & Support

1. AWS Organizations
    - Organize multiple AWS accounts
    - Consolidated billing
    - Service control policies (SCPs)

2. 