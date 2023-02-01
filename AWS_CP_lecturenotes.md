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
