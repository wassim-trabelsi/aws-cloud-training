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

