# Global Infrastructure

- [Global Infrastructure](#global-infrastructure)
  - [Why make a global application?](#why-make-a-global-application)
    - [Global AWS Infrastructure](#global-aws-infrastructure)
    - [Global Applications in AWS](#global-applications-in-aws)
  - [Amazon Route 53 Overview](#amazon-route-53-overview)
    - [Route 53 - Diagram for A Record](#route-53---diagram-for-a-record)
  - [Route 53 Routing Policies](#route-53-routing-policies)
    - [simple routing policy](#simple-routing-policy)
    - [weighted routing policy](#weighted-routing-policy)
    - [latency routing policy](#latency-routing-policy)
    - [failover routing policy](#failover-routing-policy)
  - [AWS CloudFront](#aws-cloudfront)
    - [CloudFront - Origins](#cloudfront---origins)
    - [CloudFront at a high level](#cloudfront-at-a-high-level)
    - [CloudFront - S3 as an origin](#cloudfront---s3-as-an-origin)
    - [CloudFront vs S3 Cross Region Replication](#cloudfront-vs-s3-cross-region-replication)
    - [S3 Transfer Acceleration](#s3-transfer-acceleration)
  - [AWS Global Accelerator](#aws-global-accelerator)
    - [AWS Global Accelerator vs CloudFront](#aws-global-accelerator-vs-cloudfront)
  - [AWS Outposts](#aws-outposts)
    - [AWS Outposts Benefits](#aws-outposts-benefits)
  - [AWS WaveLength](#aws-wavelength)
  - [AWS Local Zones](#aws-local-zones)
  - [Global Applications - Summary](#global-applications---summary)

## Why make a global application?

- A global application is an application deployed in **multiple geographies**
- When you translate this on AWS: this could mean that you deploy your application onto different **AWS Regions** and / or **Edge Locations**
- **Decreased Latency**
  - When you deploy the app on different AWS Regions and / or edge locations, what it gives for users around the world is a decreased latency
  - **Latency** is the time it takes for a network packet to reach a server
  - If you consider that Earth is big, then it takes more time for a packet from Asia to reach the US
  - If you deploy your applications closer to your users then it will decrease the latency, and the users will have a better experience
- **Disaster Recovery (DR)**
  - The idea is to not rely on one data center or one region. For example, if an entire AWS region goes down (earthquake, storms, power shutdown, politics), then by having disaster recovery strategy in place you can fail-over to another region and have your application will still be working
  - A DR plan is important to increase the availability of your application
- **Attack protection**: It is common for hackers online to attack your application and if you have your application across multiple regions and distributed globally, then it is going to be much harder for an attacker to attack all these locations at once making you application more protected against these attacks

### Global AWS Infrastructure

- Regions: For deploying applications and infrastructure
- Availability Zones: Regions are made of multiple availability zones which are nothing but multiple data centers
- Edge Locations (Points of Presence): Used for content delivery as close as possible to users. Cannot deploy an application there, but something like CloudFront will be using it 
- More at: <https://infrastructure.aws/>

### Global Applications in AWS

- **Global DNS: Route 53**
  - Great to route users to the closest deployment with least latency
  - Great for disaster recovery strategies
- **Global Content Delivery Network (CDN): CloudFront**
  - Replicate part of your application to AWS Edge Locations again to decrease the latency
  - Cache common requests – improved user experience and a decreased latency
- **S3 Transfer Acceleration**
  - Accelerate global uploads & downloads into Amazon S3
- **AWS Global Accelerator:**
  - Improve global application availability and performance using the AWS global network

## Amazon Route 53 Overview

- Amazon Route53 is going to be very important for us to deploy a global application
- Route53 is a Managed DNS (Domain Name System)
- DNS is like a phone book with a collection of rules and records which helps clients find the right servers through URLs.
- In AWS, the most common records are:
  - if you are mapping a **www.google.com** hostname to an **IP address** say, 12.34.56.78 then it is called an **A record (IPv4)**
  - if you are mapping a **www.google.com** hostname to an **IPv6 address** say, 2001:0db8:85a3:0000:0000:8a2e:0370:7334 then it is called an **AAAA rcord (IPv6)**
  - if you are mapping a **search.google.com** hostname to another hostname say, **www.google.com** then it is called a **CNAME** because we are mapping a **hostname to hostname**
  - if you are mapping a **example.com** hostname to an **AWS resource**, it is a special type of rcord called an **Alias record (ex: ELB, CloudFront, S3, RDS, etc...)**
- Basically, Route53 will allow us to create a record which will guide us to the instance that has the least latency

### Route 53 - Diagram for A Record

- For Route53 say, we want to see what happens for an A record
- We have a web browser and an application server that we have deployed that has a public IPv4
- We want to be able to access our application server using a normal URL
- For this, we go into Route53 and create an A record so that when the web browser does a DNS request for **myapp.mydomain.com**, the DNS will reply back with an IP
- That IP can be used by our web browser to get to our correct server and then get the HTTP response from our server. At a very high level this is how th DNS works

![Route 53](./images/../../images/Route_53.png)

<!--
```mermaid
sequenceDiagram
    participant Web browser
    participant Route 53
    participant Application Server(IP=11.12.13.1)
    Web browser->>Route 53: DNS Request app.domain.com
    Route 53 ->> Web browser: Send back IP:11.12.13.1(A record: hostname or IP)
    Web browser->>Application Server(IP=11.12.13.1): HTTP Request IP:11.12.13.1 (Host:app.domain.com)
    Application Server(IP=11.12.13.1) ->> Web browser: HTTP Response
``` -->

## Route 53 Routing Policies

Need to know them at a high-level for the Cloud Practitioner Exam

- Simple Routing Policy
- Weighted Routing Policy
- Latency Routing Policy
- Failover Routing Policy

### simple routing policy

- Use for a single resource that performs a given function for your domain
- for example, a web server that serves content for the example.com website
- You can use simple routing to create records in a private hosted zone

### weighted routing policy

- Use to route traffic to multiple resources in proportions that you specify
- You can use weighted routing to create records in a private hosted zone

### latency routing policy

- Use when you have resources in multiple AWS Regions and you want to route traffic to the region that provides the best latency
- You can use latency routing to create records in a private hosted zone

### failover routing policy

- Use when you want to configure active-passive failover
- You can use failover routing to create records in a private hosted zone

## AWS CloudFront

- Content Delivery Network (CDN)
- **Improves read performance, content is cached at the edge**
- Improves users experience
- CloudFront is made of 216 Points of Presence globally which corresponds to edge locations around the world, and AWS keeps on adding locations to improve user experience even further everywhere
- By having the content distributed globally, we are getting a DDoS protection because your application is worldwide then you are protected against these attacks, integration with Shield, AWS Web Application Firewall
- Source: <https://aws.amazon.com/cloudfront/features/?nc=sn&loc=2>
- Example - Say, we had created an S3 bucket and website on our S3 bucket in Australia, but we had a user may be in the US of A, then if the user requests the content from a American Edge location using CloudFront, and CloudFront will be able to fetch the content from Australia. Now, if another user in the US of A requests the same content, then it will be served directly from the edge without going all the way to Australia to serve that content. Same holds true for an user in China, it will talk to a Chinese Point of Presence, which is redirected to the S3 buckets in Australia, later the content will be cached at the Chinese edge location for other user requests

### CloudFront - Origins

CloudFront has several types of origins, which are basically backends you want the CloudFront to connect to
- S3 bucket
  - For distributing files and caching them at the edge
  - For uploading files to S3, CloudFront can be used as an ingress
  - The connection between S3 bucket and CloudFront is enhanced with security using Origin Access Control (OAC)
- VPC Origin
  - We can connect CloudFront directly to the applications hosted in VPC private subnets
  - Application Load Balancer / Network Load Balancer / EC2 Instances
- Custom Origin (anything that uses HTTP can be used in the backend)
  - An S3 website (must first enable the bucket as a static S3 website)
  - Any public HTTP backend you want within or outside of AWS

### CloudFront at a high level

### CloudFront - S3 as an origin

### CloudFront vs S3 Cross Region Replication

| CloudFront | S3 Cross Region Replication |
| ---------- | --------------------------- |
| Global Edge network (about 216 Points of Presence) | Must be setup for each region you want replication to happen |
| Files are cached in each edge location for a TTL (Time to Live) (maybe a day) | Files are updated in near real-time. It is for read-only |
| **Great for static content that must be available everywhere around the world** | **Great for dynamic content that needs to change all the time and be available at low-latency in a few regions** |

### S3 Transfer Acceleration

- Increase transfer speed by transferring file to an AWS edge location which will forward the data to the S3 bucket in the target region
- if we try to upload file to Australia S3 bucket it will take time using CloudFront we can rescue time.
- File in USA -> Edge Location(USA) -> S3 Bucket(Australia)
- Test the tool at: <https://s3-accelerate-speedtest.s3-accelerate.amazonaws.com/en/accelerate-speed-comparsion.html>

## AWS Global Accelerator

- Improve global application availability and performance using the AWS global network
- Traffic is routed to your applications using the AWS global network instead of the internet.
- Leverage the AWS internal network to optimize the route to your application (60% improvement)
- 2 Anycast IP are created for your application and traffic is sent through Edge Locations
- The Edge locations send the traffic to your application
- Test the tool at: <https://speedtest.globalaccelerator.aws/#/>

### AWS Global Accelerator vs CloudFront

- They both use the AWS global network and its edge locations around the world
- Both services integrate with AWS Shield for DDoS protection.
- CloudFront – Content Delivery Network
  - Improves performance for your cacheable content (such as images and videos)
  - Content is served at the edge
- Global Accelerator
  - No caching, proxying packets at the edge to applications running in one or more AWS Regions.
  - Improves performance for a wide range of applications over TCP or UDP
  - Good for HTTP use cases that require static IP addresses
  - Good for HTTP use cases that required deterministic, fast regional failover

## AWS Outposts

- **Hybrid Cloud**: businesses that keep an on - premises infrastructure alongside a cloud infrastructure
- Therefore, two ways of dealing with IT systems: • One for the AWS cloud (using the AWS console, CLI, and AWS APIs)
- One for their on-premises infrastructure
- **AWS Outposts are “server racks”** that offers the same AWS infrastructure, services, APIs & tools to build your own applications on-premises just as in the cloud
- **AWS will setup and manage “Outposts Racks”** within your on-premises infrastructure and you can start leveraging AWS services on-premises
- You are responsible for the Outposts Rack physical security

### AWS Outposts Benefits

- Low-latency access to on-premises systems
- Local data processing
- Data residency
- Easier migration from on-premises to the cloud
- Fully managed service
- Some services that work on Outposts:
  - EC2
  - EBS
  - S3
  - EKS
  - ECS
  - RDS
  - EMR

## AWS WaveLength

- WaveLength Zones are infrastructure deployments embedded within the telecommunications providers’ datacenters at the edge of the 5G networks
- Brings AWS services to the edge of the 5G networks
- Example: EC2, EBS, VPC…
- Ultra-low latency applications through 5G networks
- Traffic doesn’t leave the Communication Service Provider’s (CSP) network
- High-bandwidth and secure connection to the parent AWS Region
- No additional charges or service agreements
- Use cases: Smart Cities, ML-assisted diagnostics, Connected Vehicles, Interactive Live Video Streams, AR/VR, Real-time Gaming

## AWS Local Zones

- Places AWS compute, storage, database, and other selected AWS services closer to end users to run latency-sensitive
applications
- Extend your VPC to more locations – “Extension of an AWS Region”
- Compatible with EC2, RDS, ECS, EBS, ElastiCache, Direct Connect …
- Example:
  - AWS Region: N. Virginia (us-east-1)
  - AWS Local Zones: Boston, Chicago, Dallas, Houston, Miami

## Global Applications - Summary

- Global DNS: Route 53
  - Great to route users to the closest deployment with least latency
  - Great for disaster recovery strategies
- Global Content Delivery Network (CDN): CloudFront
  - Replicate part of your application to AWS Edge Locations – decrease latency
  - Cache common requests – improved user experience and decreased latency
- S3 Transfer Acceleration
  - Accelerate global uploads & downloads into Amazon S3
- AWS Global Accelerator
  - Improve global application availability and performance using the AWS global network
- AWS Outposts
  - Deploy Outposts Racks in your own Data Centers to extend AWS services
- AWS WaveLength
  - Brings AWS services to the edge of the 5G networks
  - Ultra-low latency applications
- AWS Local Zones
  - Bring AWS resources (compute, database, storage, …) closer to your users
  - Good for latency-sensitive applications
