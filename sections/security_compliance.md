# Security & Compliance

- [Security \& Compliance](#security--compliance)
  - [AWS Shared Responsibility Model](#aws-shared-responsibility-model)
    - [Example, for RDS](#example-for-rds)
    - [Example for S3](#example-for-s3)
    - [Shared Responsibility Model Diagram](#shared-responsibility-model-diagram)
  - [DDOS Protection on AWS](#ddos-protection-on-aws)
    - [Sample Reference Architecture for DDoS Protection](#sample-reference-architecture-for-ddos-protection)
  - [AWS Shield](#aws-shield)
  - [AWS WAF - Web Application Firewall](#aws-waf---web-application-firewall)
  - [AWS Network Firewall](#aws-network-firewall)
  - [AWS Firewall Manager](#aws-firewall-manager)
  - [Penetration Testing on AWS Cloud](#penetration-testing-on-aws-cloud)
  - [Data at rest vs. Data in transit](#data-at-rest-vs-data-in-transit)
  - [AWS KMS (Key Management Service)](#aws-kms-key-management-service)
  - [AWS CloudHSM](#aws-cloudhsm)
    - [CloudHSM Diagram](#cloudhsm-diagram)
  - [Types of KMS Keys](#types-of-kms-keys)
  - [AWS Certificate Manager (ACM)](#aws-certificate-manager-acm)
  - [AWS Secrets Manager](#aws-secrets-manager)
  - [AWS Artifact (not really a service)](#aws-artifact-not-really-a-service)
  - [AWS GuardDuty](#aws-guardduty)
  - [AWS Inspector](#aws-inspector)
    - [What does AWS Inspector evaluate?](#what-does-aws-inspector-evaluate)
  - [AWS Config](#aws-config)
    - [AWS Config Resource](#aws-config-resource)
  - [Amazon Macie](#amazon-macie)
  - [AWS Security Hub](#aws-security-hub)
  - [Amazon Detective](#amazon-detective)
  - [AWS Abuse](#aws-abuse)
  - [Root user privileges](#root-user-privileges)
  - [IAM Access Analyzer](#iam-access-analyzer)
  - [Summary](#summary)


## AWS Shared Responsibility Model

- AWS and customers share responsibilities for security and compliance.
  - **AWS Responsibility:** - Security **of** the Cloud
    - Protecting infrastructure (hardware, software, facilities, and networking) that runs all the AWS services
    - Managed services like S3, DynamoDB, RDS, etc.
  - **Customer:** - Security **in** the cloud
    - For EC2 instance, customer is responsible for management of the guest OS (including security patches and updates), firewall & network configuration, IAM
    - Encrypting application data
  - **Shared controls:** Patch Management, Configuration Management, Awareness & Training

### Example, for RDS

- **AWS responsibility:**
  - Manage the underlying EC2 instance, disable SSH access
  - Automated DB patching
  - Automated OS patching
  - Audit the underlying instance and disks & guarantee it functions
- **Your responsibility:**
  - Check the ports / IP / security group inbound rules in DB’s SG
  - In-database user creation and permissions
  - Creating a database with or without public access
  - Ensure parameter groups or DB is configured to only allow SSL connections
  - Database encryption setting

### Example for S3

- **AWS responsibility:**
  - Guarantee you get unlimited storage
  - Guarantee you get encryption
  - Ensure separation of the data between different customers
  - Ensure AWS employees can’t access your data
- **Your responsibility:**
  - Bucket configuration
  - Bucket policy / public setting
  - IAM user and roles
  - Enabling encryption

### Shared Responsibility Model Diagram

  ![SHARED RESPONSIBILITY MODEL DIAGRAM](../images/Shared_Responsibility_Model.jpg)

## DDOS Protection on AWS

- **AWS Shield Standard**: protects against DDOS attack for your website and applications, for all customers at no additional costs
- **AWS Shield Advanced**: 24/7 premium DDoS protection
- **AWS WAF**: Filter specific requests based on rules
- **CloudFront and Route 53**:
  - Availability protection using global edge network
  - Combined with AWS Shield, provides attack mitigation at the edge
- Be ready to scale – leverage **AWS Auto Scaling**

### Sample Reference Architecture for DDoS Protection

  ![DDOS ATTACK SAMPLE ARCHITECTURE](../images/Sample_Reference_Architecture_DDOS_Attack.PNG)

## AWS Shield

- **AWS Shield Standard:**
  - Free service that is activated for every AWS customer
  - Provides protection from attacks such as SYN/UDP Floods, Reflection attacks and other layer 3/layer 4 attacks
- **AWS Shield Advanced:**
  - Optional DDoS mitigation service ($3,000 per month per organization)
  - Protect against more sophisticated attack on **Amazon EC2, Elastic Load Balancing (ELB), Amazon CloudFront, AWS Global Accelerator, and Route 53**
  - 24/7 access to AWS DDoS response team (DRP)
  - Protect against higher fees during usage spikes due to DDoS

## AWS WAF - Web Application Firewall

- Protects your web applications from common web exploits (Layer 7)
- **Layer 7 is HTTP** (vs Layer 4 is TCP)
- Deploy on **Application Load Balancer, API Gateway, CloudFront**
- Define Web ACL (Web Access Control List):
  - Rules can include **IP addresses**, HTTP headers, HTTP body, or URI strings
  - Protects from common attack - **SQL injection and Cross-Site Scripting (XSS)**
  - Size constraints, **geo-match (block countries)**
  - **Rate-based rules** (to count occurrences of events) – **for DDoS protection**

## AWS Network Firewall

  ![AWS NETWORK FIREWALL](../images/AWS_Network_Firewall.PNG)

- Protect your entire Amazon VPC
- From Layer 3 to Layer 7 protection
- Any direction, you can inspect
  - VPC to VPC traffic
  - Outbound to internet
  - Inbound from internet
  - To / from Direct Connect & Site-to-Site VPN

## AWS Firewall Manager

- **Manage security rules in all accounts of an AWS Organization**
- Security policy: common set of security rules
  - VPC Security Groups for EC2, Application Load Balancer, etc...
  - WAF rules
  - AWS Shield Advanced
  - AWS Network Firewall
- **Rules are applied to new resources as they are created (good for compliance) across all and future accounts in your Organization**

## Penetration Testing on AWS Cloud

- AWS customers are welcome to carry out security assessments or penetration tests against their AWS infrastructure **without prior approval for 8 services:**
  - Amazon EC2 instances, NAT Gateways, and Elastic Load Balancers
  - Amazon RDS
  - Amazon CloudFront
  - Amazon Aurora
  - Amazon API Gateways
  - AWS Lambda and Lambda Edge functions
  - Amazon Lightsail resources
  - Amazon Elastic Beanstalk environments
- List can increase over time (you won’t be tested on that at the exam)
- **Prohibited Activities**
  - DNS zone walking via Amazon Route 53 Hosted Zones
  - Denial of Service (DoS), Distributed Denial of Service (DDoS), Simulated DoS, Simulated DDoS
  - Port flooding
  - Protocol flooding
  - Request flooding (login request flooding, API request flooding)
- For any other simulated events, contact aws-security-simulatedevent@amazon.com
- Read more: <https://aws.amazon.com/security/penetration-testing/>

## Data at rest vs. Data in transit

  ![DATA AT REST AND TRANSIT](../images/Data_At_Rest_At_Transit.PNG)

- **At rest:** data stored or archived on a device
  - On a hard disk, on a RDS instance, in S3 Glacier Deep Archive, etc.
- **In transit (in motion):** data being moved from one location to another
  - Transfer from on-premises to AWS, EC2 to DynamoDB, etc.
  - **Means data transferred on the network**
- We want to encrypt data in both states to protect it!
- For this we leverage **encryption keys**

## AWS KMS (Key Management Service)

- Anytime you hear “encryption” for an AWS service, it’s most likely KMS
- KMS = **AWS manages the encryption keys for us**
- **Encryption Opt-in:**
  - EBS volumes: encrypt volumes
  - S3 buckets: Server-side encryption of objects (SSE-S3 enabled by default, SSE-KMS opt in
  - Redshift database: encryption of data
  - RDS database: encryption of data
  - EFS drives: encryption of data
- **Encryption Automatically enabled:**
  - CloudTrail Logs
  - S3 Glacier
  - Storage Gateway

## AWS CloudHSM

  ![SAMPLE CLOUD HSM DEVICE](../images/Sample_CloudHSM_Device.PNG)

- KMS => AWS manages the software for encryption
- CloudHSM => AWS provisions encryption **hardware**
- Dedicated Hardware (HSM = Hardware Security Module)
- You manage your own encryption keys entirely (not AWS)
- HSM device is tamper resistant, FIPS 140-2 Level 3 compliance

### CloudHSM Diagram

  ![CLOUD HSM DIAGRAM](../images/CloudHSM_Diagram.PNG)

## Types of KMS Keys

- **Customer Managed CMK:**
  - Create, manage and used by the customer, can enable or disable
  - Possibility of rotation policy (new key generated every year, old key preserved)
  - Possibility to bring-your-own-key
- **AWS managed CMK:**
  - Created, managed and used on the customer’s behalf by AWS
  - Used by AWS services (aws/s3, aws/ebs, aws/redshift)
- **AWS owned CMK:**
  - Collection of CMKs that an AWS service owns and manages to use in multiple accounts
  - AWS can use those to protect resources in your account (but you can’t view the keys)
- **CloudHSM Keys (custom keystore):**
  - Keys generated from your own CloudHSM hardware device
  - Cryptographic operations are performed within the CloudHSM cluster

## AWS Certificate Manager (ACM)

  ![AWS CERTIFICATE MANAGER](../images/AWS_Certificate_Manager.PNG)

- Let’s you easily provision, manage, and deploy **SSL/TLS Certificates**
- Used to provide in-flight encryption for websites (HTTPS)
- Supports both public and private TLS certificates
- Free of charge for public TLS certificates
- Automatic TLS certificate renewal
- Integrations with (load TLS certificates on)
  - Elastic Load Balancers
  - CloudFront Distributions
  - APIs on API Gateway

## AWS Secrets Manager

- Newer service, meant for storing secrets
- Capability to force **rotation of secrets** every X days
- Automate generation of secrets on rotation (uses Lambda)
- Integration with **Amazon RDS** (MySQL, PostgreSQL, Aurora)
- Secrets are encrypted using KMS
- Mostly meant for RDS integration

## AWS Artifact (not really a service)

- **Portal that provides customers with on-demand access to AWS compliance documentation and AWS agreements**
- **Artifact Reports** - Allows you to download AWS security and compliance documents from third-party auditors, like AWS ISO certifications, Payment Card Industry (PCI), and System and Organization Control (SOC) reports
- **Artifact Agreements** - Allows you to review, accept, and track the status of AWS agreements such as the Business Associate Addendum (BAA) or the Health Insurance Portability and Accountability Act (HIPAA) for an individual account or in your organization
- Can be used to **support internal audit or compliance**

## AWS GuardDuty

- Intelligent Threat discovery to protect your AWS Account
- Uses Machine Learning algorithms, anomaly detection, 3rd party data
- One click to enable (30 days trial), no need to install software
  - Input data includes:
    - **CloudTrail Events Logs** – unusual API calls, unauthorized deployments
      - **CloudTrail Management Events** – create VPC subnet, create trail, ...
      - **CloudTrail S3 Data Events** – get object, list objects, delete object, ...
    - **VPC Flow Logs** – unusual internal traffic, unusual IP address
    - **DNS Logs** – compromised EC2 instances sending encoded data within DNS queries
    - **Kubernetes Audit Logs** – suspicious activities and potential EKS cluster compromises
- Can setup **EventBridge rules** to be notified in case of findings
- CloudWatch Events rules can target AWS Lambda or SNS
- **Can protect against CryptoCurrency attacks (has a dedicated “finding” for it)**

  ![AWS GUARD DUTY](../images/AWS_GuardDuty.PNG)

## AWS Inspector

  ![AWS INSPECTOR](../images/AWS_Inspector.PNG)

- **Automated Security Assessments**
- **For EC2 instances**
  - Leveraging the **AWS System Manager (SSM) agent**
  - Analyze against **unintended network accessibility**
  - Analyze the **running OS** against **known vulnerabilities**
- **For Container Images push to Amazon ECR**
  - Assessment of Container Images as they are pushed
- **For Lambda Functions**
  - Identifies software vulnerabilities in function code and package dependencies
  - Assessment of functions as they are deployed
- Reporting & integration with AWS Security Hub
- Send findings to Amazon Event Bridge

### What does AWS Inspector evaluate?

- **Remember: only for EC2 instances and container infrastructure**
- Continuous scanning of the infrastructure, only when needed
- Package vulnerabilities (EC2 & ECR) – database of CVE
- Network reachability (EC2)
- A risk score is associated with all vulnerabilities for prioritization

## AWS Config

- Helps with **auditing and recording compliance of your AWS resources**
- Helps **record configurations and changes over time**
- Possibility of storing the configuration data into S3 (analyzed by Athena)
- Questions that can be solved by AWS Config:
  - Is there unrestricted SSH access to my security groups?
  - Do my buckets have any public access?
  - How has my ALB configuration changed over time?
- You can receive alerts (SNS notifications) for any changes
- AWS Config is a per-region service
- Can be aggregated across regions and accounts

### AWS Config Resource

- **View compliance of a resource over time**

  ![AWS CONFIG 1](../images/AWS_Config_1.PNG)

- **View configuration of a resource over time**

  ![AWS CONFIG 2](../images/AWS_Config_2.PNG)

- **View CloudTrail API calls if enabled**

## Amazon Macie

  ![AWS MACIE](../images/AWS_Macie.PNG)

- Amazon Macie is a fully managed data security and data privacy service that uses **machine learning and pattern matching to discover and protect your sensitive data in AWS**
- Macie helps identify and alert you to sensitive data, such as **personally identifiable information (PII)**

## AWS Security Hub

- **Central security tool** to manage security **across several AWS accounts and automate security checks**
- Integrated dashboards showing current security and compliance status to quickly take actions
- Automatically aggregates alerts in predefined or personal findings formats from various AWS services & AWS partner tools:
  - Config
  - GuardDuty
  - Inspector
  - Macie
  - IAM Access Analyzer
  - AWS Systems Manager
  - AWS Firewall Manager
  - AWS Health
  - AWS Partner Network Solutions
- Must first enable the AWS Config Service

  ![AWS SECURITY HUB](../images/AWS_Security_Hub.PNG)

## Amazon Detective

- GuardDuty, Macie, and Security Hub are used to identify potential security issues, or findings
- Sometimes security findings require deeper analysis to isolate the root cause and take action – it’s a complex process
- Amazon Detective **analyzes, investigates, and quickly identifies the root cause of security issues or suspicious activities (using ML and graphs)**
- **Automatically collects and processes events** from VPC Flow Logs, CloudTrail, GuardDuty and create a unified view
- Produces visualizations with details and context to get to the root cause

## AWS Abuse

- **Report suspected AWS resources used for abusive or illegal purposes**
- Abusive & prohibited behaviors are:
  - **Spam** – receiving undesired emails from AWS-owned IP address, websites & forums spammed by AWS resources
  - **Port scanning** – sending packets to your ports to discover the unsecured ones
  - **DoS or DDoS attacks** – AWS-owned IP addresses attempting to overwhelm or crash your servers/softwares
  - **Intrusion attempts** – logging in on your resources
  - **Hosting objectionable or copyrighted content** – distributing illegal or copyrighted content without consent
  - **Distributing malware** – AWS resources distributing software to harm computers or machines
- Contact the AWS Abuse team: [AWS Abuse Form](https://aws.amazon.com/forms/report-abuse), or abuse@amazonaws.com

## Root user privileges

- Root user = Account Owner (created when the account is created)
- Has complete access to all AWS services and resources
- **Lock away your AWS account root user access keys!**
- Do not use the root account for everyday tasks, even administrative tasks
- **Actions that can be performed only by the root user:**
  - **Change account settings** (account name, email address, root user password, root user access keys)
  - View certain tax invoices
  - **Close your AWS account**
  - Restore IAM user permissions
  - **Change or cancel your AWS Support plan**
  - **Register as a seller in the Reserved Instance Marketplace**
  - Configure an Amazon S3 bucket to enable MFA
  - Edit or delete an Amazon S3 bucket policy that includes an invalid VPC ID or VPC endpoint ID
  - Sign up for GovCloud

## IAM Access Analyzer

  ![IAM ACCESS ANALYZER](../images/IAM_Access_Analyzer.PNG)

- AWS IAM Access Analyzer is a tool that scans your AWS resource policies to find any unintended public or cross-account access. It helps you identify and fix security issues, ensuring that only authorized entities have access to your resources.
- Find out which resources are shared externally:
  - S3 Buckets
  - IAM Roles
  - KMS Keys
  - Lambda Functions and Layers
  - SQS queues
  - Secrets Manager Secrets
- Define **Zone of Trust** = AWS Account or AWS Organization.
- Access outside zone of trusts => findings

## Summary

- **Shared Responsibility on AWS**
- **Shield:** Automatic DDoS Protection + 24/7 support for advanced
- **WAF:** Firewall to filter incoming requests based on rules
- **KMS:** Encryption keys managed by AWS
- **CloudHSM:** Hardware encryption, we manage encryption keys
- **AWS Certificate Manager:** provision, manage, and deploy SSL/TLS Certificates
- **Artifact:** Get access to compliance reports such as PCI, ISO, etc...
- **GuardDuty:** Find malicious behavior with VPC, DNS & CloudTrail Logs
- **Inspector:** find software vulnerabilities in EC2, ECR Images, and Lambda functions
- **Network Firewall:** Protect VPC against network attacks
- **Config:** Track config changes and compliance against rules
- **Macie:** Find sensitive data (ex: PII data) in Amazon S3 buckets
- **CloudTrail:** Track API calls made by users within account
- **AWS Security Hub:** gather security findings from multiple AWS accounts
- **Amazon Detective:** find the root cause of security issues or suspicious activities
- **AWS Abuse:** Report AWS resources used for abusive or illegal purposes
- **Root user privileges:**
  - Change account settings
  - Close your AWS account
  - Change or cancel your AWS Support plan
  - Register as a seller in the Reserved Instance Marketplace
- **IAM Access Analyzer:** identify which resources are shared externally
- **Firewall Manager:** manage security rules across an Organization (WAF, Shield…)