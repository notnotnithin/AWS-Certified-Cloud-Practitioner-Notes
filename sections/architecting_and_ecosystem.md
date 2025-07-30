# AWS Architecting & Ecosystem

- [AWS Architecting \& Ecosystem](#aws-architecting--ecosystem)
  - [Well Architected Framework General Guiding Principles](#well-architected-framework-general-guiding-principles)
  - [AWS Cloud Best Practices - Design Principles](#aws-cloud-best-practices---design-principles)
  - [Well Architected Framework 6 Pillars](#well-architected-framework-6-pillars)
    - [1. Operational Excellence](#1-operational-excellence)
    - [2. Security](#2-security)
    - [3. Reliability](#3-reliability)
    - [4. Performance Efficiency](#4-performance-efficiency)
    - [5. Cost Optimization](#5-cost-optimization)
    - [6. Sustainability](#6-sustainability)
  - [AWS Well-Architected Tool](#aws-well-architected-tool)
  - [AWS Right Sizing](#aws-right-sizing)
  - [AWS Ecosystem - Free Resources](#aws-ecosystem---free-resources)
    - [AWS Ecosystem - AWS Support](#aws-ecosystem---aws-support)
  - [AWS Marketplace](#aws-marketplace)

## Well Architected Framework General Guiding Principles

- In the cloud when you want a good architecture, you need to **stop guessing your capacity needs** and instead use auto scaling and scale based on what the actual demand on your system is going to be 
- You should also test your systems at production scale. In the cloud you can create as many resources as you want very quickly and as such there is no reason why, for example, for one hour you could not test a system at production scale. This allows you in the future to make sure your system is ready when you actually publish it to your customers
- It is also important to automate to facilitate architectural experimentation easier. For this, CloudFormation is very important because you get Infrastructure as Code (IaC) then you can easily create an architecture on different accounts and different regions. Also, using Platform As A Service Beanstalk could be very helpful to experiment quickly
- You should also allow to make your architectures evolutionary
  - Means you need to design based on changing requirements.
- Drive architectures using data. It is good to guess it is good to look at what the actual usage is. What are the patterns, what are the queries, and then drive your architecture and use the right services based on what you actually need based on this data
- Also improve through game days by simulating applications for flash sale days which will stress your system which allows you know whether your system is doing well. For example, Netflix has something called "Chaos Monkey" a program that is in their EC2 environment goes ahead and terminates EC2 instances at random in production. Thanks to Chaos Monkey they will see if they are ready for failures, ready for big spikes, ready for errors. 

## AWS Cloud Best Practices - Design Principles

- **Scalability**: Scale both vertically and horizontally
- **Disposable Resources**: Servers should be disposable and easily configured. If you put too much configuration onto a server and somehow that server loses its resource, and you need three days to reconfigure that server back, then you haven't done things properly
- **Automation**: Guided by principles like serverless, infrastructure as a service, auto-scaling and so on
- **Loose Coupling**: 
  - Break monolithic applications into smaller
  - Loosely coupled components 
  - A change or a failure in one component should not cascade to other components
- **Services, Not Servers**: 
  - Don't use just EC2
  - Use managed services, databases, and serverless options, etc..

## Well Architected Framework 6 Pillars

- This is where the Well Architected Framework made up of 6 pillars comes in:
  - Operational Excellence
  - Security
  - Reliability
  - Performance Efficiency
  - Cost Optimization
  - Sustainability
- The idea is that acting with these 6 pillars you are having good architecture on AWS
- They are not something you balance and compromise between them or trade offs, they are actually a synergy. For example, when you prove your operational excellence you are probably also improving your cost optimization

### 1. Operational Excellence

- This is the ability to run and monitor systems for business value and continuously improve supporting processes and procedures.
- **Design Principles**:
  - **To perform operations as code** - Infrastructure as code
  - **Make frequent, small, reversible changes** - So that in case of any failure, you can reverse it. If you start making huge changes every 3 months, things are not going to go well
  - **Refine operations procedures frequently** - ensure that all your team members are familiar with these new operations procedures
  - **Anticipate failure**
  - **Learn from all operational failures**
  - **Use managed services** - to reduce operational burden
  - **Implement observability for actionable insights** - This includes performance, reliability, and cost

### 2. Security

- It includes the ability to protect information, systems, and assets while delivering business value through risk assessments and mitigation strategies. Once you have security in place, you are really minimizing a risk over time and you save cost from disasters and you really don't want to have a risk, or a security issue in your company
- **Design Principles**:
  - **Implement a strong identity foundation** - You may want to centralize how you manage your accounts, you may want to rely on priniciple of least privilege and IAM is one the services to help you do that
  - **We want to enable traceability** - that means we need to look into all the logs, all the metrics and store them and automatically respond and take action every time something loks really weird
  - **Apply security at all layers** - like edge network, VPC, subnet, load balancer, every instance you ever have, OS, application
  - **Automate security best practices** - it is mostly done well, when it is automated
  - **Protect data in transit and at rest** - Means always enable encryption, always do SSL, always use tokenization, and do access control to keep people away from data. Why is someone requesting data? Isn't that risk when you allow someone to access data, do they really need it or is there a way to automate the need for that direct access and that manual processing of data
  - **Prepare for security events** - Security event must happen in every company someday. Then you run response simulations, use tools to automate the speed of detection, investigation and recovery

### 3. Reliability

- It is the ability to recover from infrastructure or service disruptions, dynamically acquire computing resources to meet demand, and mitigate disruptions such as misconfigurations or transient network issues
- It is about making sure your application runs no matter what
- **Design Principles**:
  - **Test recovery procedures** - Use automation to simulate different failures or to recreate scenarios that led to failures before
  - **Automatically recover from failure** - You need to anticipate and remediate failures before they occur
  - **Scale horizontally to increase aggregate system availability** - Distribute requests across multiple, smaller resources to ensure that they don't share a common point of failure
  - **Stop guessing capacity** - Maintain the optimal level to satisfy demand without over or under provisioning - Use Auto Scaling to make sure you have the right capacity at any time
  - **Manage change in automation** - Use automation to make changes to infrastructure. This is to make sure that your application will be reliable or you can rollback or whatever

### 4. Performance Efficiency

- It includes the ability to use computing resources efficiently to meet system requirements, and to maintain that efficiency as demand changes and technologies evolve
- It is all about adapting and providing the best performance
- **Design Principles**:
  - **Democratize advanced technologies** - To use advance technologies and as the services become available they might be helpful for your product developments
  - **Go global in minutes** - If you need to deploy in multiple regions, it should not last days, it should last minutes may be using CloudFormation
  - **Use serverless architectures** - That's the golden state. That means you do not manage any servers, and everything scales for you.
  - **Experiment more often** - May be you have something working really well today, but you think it won't scale to 10 times the load, you experiment. May be try serverless architectures and see if that works for you.
  - **Mechanical Sympathy** - Be aware of all AWS services

### 5. Cost Optimization

- Includes the ability to run systems to deliver business value at the lowest price point
- **Design Principles**:
  - **Adopt a consumption model** - Pay for only what you use. For example, AWS Lambda is one of the services, you do not pay for it, where as RDS, if you do not use your database, you still pay for it because you have provisioned your database.
  - **Measure overall efficiency** - Use CloudWatch
  - **Stop spending on data center operations** - AWS does the infrastructure for you and they just allow you to focus on your applications, your systems
  - **Analyze and attribute expenditure** - Meaning when you do not use tags on your AWS resources, you are goingg to have a lot of trouble figuring out which application is costing yoou a lot of money. Using tags ensures that you are able track the cost of each application and optimize them over time and get a Return On Investment (ROI) on how much money it generates for your business
  - **Use managed and application level services to reduce cost of ownership** - Because managed services leverage the cloud and operate at cloud scale and they can offer such a lower cost per transaction or service, and that is really something that you have to remember about the cloud

### 6. Sustainability

- The sustainability pillar focuses on minimizing the environmental impacts of running cloud workloads.
- **Design Principles**:
  - **Understand your impact** - Establish performance indicators, and evalute improvements
  - **Establish sustainability goals for each workload** - Set long-term goals for each workload, model Return On Investment (ROI)
  - **Maximize utilization of your resources** - Because you want to be energy efficient and obviously be environmental conscious
  - **Anticipate and adopt new, more efficient hardware and software offerings** - Because AWS does some optimizations to their infrastructure and if you use their newer stuff, then you are being more efficient
  - **Use managed services** - Because you share the infrastructure with many people and therefore you are in a better space for sustainability. One of the best practices is moving infrequent accessed data to cold storage and adjusting compute capacity.
  - **Reduce downstream impact of your cloud workloads** - Reduce the amount of energy or resources required to use your services and reduce the need for your customers to constantly upgrade their devices

## AWS Well-Architected Tool

- Free tool to review architectures against the 6 pillars and adopt best practices
- **How it works**:
  - Select your workload and answer questions
  - Review answers against the 6 pillars
  - Obtain advice: videos, documentation, reports, and dashboards

## AWS Right Sizing

- Match instance types and sizes to workload performance and capacity requirements at the lowest cost
- Right sizing involves starting small and scaling up easily, continuously adjusting after cloud onboarding, and using tools like CloudWatch, Cost Explorer, and Trusted Advisor

## AWS Ecosystem - Free Resources

- **AWS Blogs**: [AWS Blogs](https://aws.amazon.com/blogs/aws/)
- **AWS Forums**: [AWS Forums](https://forums.aws.amazon.com/index.jspa)
- **AWS Whitepapers & Guides**: [AWS Whitepapers & Guides](https://aws.amazon.com/whitepapers)
- **AWS Quick Starts**: [AWS Quick Starts](https://aws.amazon.com/quickstart/)
  - Automated, gold-standard deployments in the AWS Cloud.
  - Examples: WordPress on AWS, leveraging CloudFormation.
- **AWS Solutions**: [AWS Solutions](https://aws.amazon.com/solutions/)
  - Vetted technology solutions for the AWS Cloud.
  - Example - AWS Landing Zone (secure, multi-account environment).

### AWS Ecosystem - AWS Support

| DEVELOPER                                               | BUSINESS                                                      | ENTERPRISE                                                      |
| ------------------------------------------------------- | ------------------------------------------------------------- | --------------------------------------------------------------- |
| Business hours email access to Cloud Support Associates | 24x7 phone, email, and chat access to Cloud Support Engineers | Access to a Technical Account Manager (TAM)                     |
| General guidance: < 24 business hours                   | Production system impaired: < 4 hours                         | Concierge Support Team (for billing and account best practices) |
| System impaired: < 12 business hours                    | Production system down: < 1 hour                              | Business-critical system down: < 15 minutes                     |

## AWS Marketplace

- Digital catalog with thousands of software listings from independent software vendors.
- Examples:
  - Custom AMIs, CloudFormation templates, SaaS, containers.
- Purchases go into your AWS bill.
- You can sell your own solutions on the AWS Marketplace.
