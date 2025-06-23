# AWS Interview Questions and Answers

## 1. What are the security parameters we must consider while creating an EC2 instance for production?
   - Using security groups to control traffic
   - Implementing IAM roles
   - Enabling encryption for EBS volumes
   - Regular security updates and patches
   - Monitoring and logging

## 2. How can you protect the data in an AWS instance?
   - Encrypting data at rest and in transit
   - Restricting access using IAM, SGs, and VPC
   - Using SSM over SSH
   - Monitoring and auditing activity
   - Taking regular, encrypted backups

## 3. How to login to a VM if it has only a private IP?
   - Using a bastion host (jump server)
   - Using VPN
   - Using AWS Session Manager (SSM) if SSM Agent is installed
   - Using IAM permissions for secure access

## 4. What are the limitations of AWS Lambda?
   - Execution Time: Maximum 15 minutes (900 seconds)
   - Memory: 128 MB to 10,240 MB (10 GB)
   - Package Size:
     - 50 MB (direct upload)
     - 250 MB (uncompressed in /tmp)
     - Layer limit: 5 layers, 50 MB each
   - Stateless, no GPU, limited persistent storage

## 5. How does Lambda work with containers?
   - Supports Docker container images up to 10 GB
   - Provides flexibility in dependencies and tooling
   - Allows custom runtime environments
   - Supports multiple programming languages

## 6. What is the purpose of creating S3 bucket policies?
   - Control access to S3 bucket and contents
   - Define who can perform what actions
   - Set conditions for access
   - Manage cross-account access

## 7. How do you maintain the lifecycle of an S3 bucket?
   -A lifecycle policy defines when and how S3 objects should be: Transitioned to cheaper storage classes (e.g., Glacier) Expired (deleted) after a certain period Noncurrent versions (in versioned buckets) cleaned up

    "I usually start by analyzing the data retention and access patterns. Based on that, I create lifecycle rules which:
        Automatically transition infrequently accessed data to cheaper storage classes like S3 Standard-IA, Glacier, or Deep Archive after a set number of days.
        Expire temporary or outdated objects, for example, logs older than 90 or 180 days.
        If versioning is enabled, I also set rules to delete non-current object versions after a specific period to reduce cost.

I use S3 lifecycle rules to automatically transition data to cheaper storage and delete old or unused files — either by prefix or tag — to optimize costs and meet data retention policies."

## 8. What is the difference between S3 bucket policies and ACLs?
   S3 Bucket Policies:
   - JSON-based resource policies
   - Fine-grained access control
   - Support conditions (IP, VPC, time)
   - Enable cross-account access
   
   S3 ACLs:
   - Legacy access control mechanism
   - Basic read/write permissions
   - Limited to specific accounts/groups
   - No complex conditions

## 9. What is the difference between S3 and EBS?
   S3:
   - Object-based storage
   - Accessed via API/web
   - Unlimited storage
   - Pay per use
   
   EBS:
   - Block-level storage
   - Mounted to EC2
   - Limited size per volume
   - Direct access

## 10. How do you configure AWS RDS, and what factors do you consider?
    1. Database Engine Selection
    2. Instance Size (Compute + Memory)
    3. Storage Type
    4. High Availability
    5. Network Configuration
    6. Backup and Maintenance
    7. Monitoring and Alerts
    8. Security Considerations

## 11. How many masters and slaves are in RDS?
    - One primary (master) instance
    - Up to 5 read replicas per master
    - Typical setup: 1 primary + 2 read replicas

## 12. What are Network ACLs and Security Groups, and how do they differ?
    Security Groups:
    - Instance-level firewall
    - Stateful
    - Allow rules only
    
    Network ACLs:
    - Subnet-level firewall
    - Stateless
    - Allow and deny rules

## 13. How many NAT Gateways are needed for two public & two private subnets in a single VPC?
    - Minimum: 1 NAT Gateway
    - Recommended: 2 NAT Gateways (one per AZ)
    - For high availability: 2 NAT Gateways in different AZs

## 14. What is Route 53 and how do you use it?

    - Route 53 is a scalable DNS web service that provides domain registration, DNS routing, and health checking.
     1️⃣ DNS Service (Domain Name Resolution) Translates domain names (e.g., www.example.com) into IP addresses (e.g., 192.0.2.1) so that users can access web applications. 
     2️⃣ Domain Registration You can buy and manage domain names directly through Route 53. 
     3️⃣ Traffic Routing (Global DNS Load Balancing) Route 53 routes users to resources based on advanced routing policies:
     4️⃣ Health Checks and Failover Route 53 can monitor the health of endpoints using: HTTP/HTTPS/TCP checks Integrated with DNS to failover to healthy endpoints
   

## 15. Explain TTL in DNS - how does it work?
    - TTL (Time To Live) defines cache duration
    - Flow:
      1. User requests domain
      2. Resolver checks cache
      3. If not cached, queries authoritative server
      4. Caches response for TTL duration
      5. Uses cached value until TTL expires

## 16. What is the difference between IAM Users and Roles?
    IAM Users:
    - Permanent identity
    - Long-term credentials
    - Direct access to services
    
    IAM Roles:
    - Temporary identity
    - No permanent credentials
    - Assumed by users/services

## 17. What are the best password security practices?
    1. Strong Password Policies
    2. Multi-Factor Authentication (MFA)
    3. No Hardcoded Passwords
    4. Regular Password Rotation

## 18. What are the use cases for CloudWatch?
    1. Resource Monitoring
    2. Log Collection and Analysis
    3. Real-Time Alerting
    4. Dashboard Visualization

## 19. How do you implement cost optimization in AWS?
    - Right-sizing instances
    - Using Reserved Instances
    - Implementing auto-scaling
    - Using Spot Instances
    - Optimizing storage
    - Regular resource review
    - Using Cost Explorer

## 20. What is AWS Fargate?
    - Serverless compute engine for containers
    - Works with ECS and EKS
    - No EC2 instance management
    - Automatic resource allocation

## 21. What is AWS Storage Gateway?
    - Hybrid cloud storage service
    - Connects on-premises to AWS
    - Local caching capability
    - Secure data transfer

## 22. What are the types of load balancers in AWS?
    1. Application Load Balancer (ALB)
    2. Network Load Balancer (NLB)
    3. Gateway Load Balancer (GWLB)

## 23. How does weighted routing work in load balancers?
    Weighted routing allows you to distribute traffic to different targets based on assigned weights. This is useful for testing new application versions or gradually shifting traffic.

    Where Weighted Routing Is Used: 🔹 1. Route 53 (DNS-Based Weighted Routing) Not a load balancer, but used for routing traffic between resources like:

        Different EC2 instances
        Load balancers in different regions
        S3 static websites
        Any IP-based endpoint
    Example: You define two records with the same domain (e.g., app.example.com): Record A → ALB in us-east-1 with weight 80 Record B → ALB in us-west-1 with weight 20 🎯 80% of DNS traffic is routed to us-east-1, and 20% to us-west-1.

    Starting from 2023, ALB supports weighted target groups within the same listener rule.

    "Weighted routing allows us to split traffic between multiple targets based on defined percentages. In Route 53, it works at the DNS level, useful for region-based routing or blue/green deployments. With ALB, weighted target groups allow canary deployments by controlling how much traffic is sent to each version of the application.

## 24. How would you approach RDS migration with minimal downtime?
    - Use AWS Database Migration Service
    - Implement replication
    - Perform cutover during low-traffic periods
    - Validate data consistency
    - Monitor performance during migration

## 25. How do you implement best security policies on AWS?
    1. IAM Best Practices
    2. Data Protection
    3. Network Security
    4. Monitoring & Auditing

1. In CloudWatch, what is the use of log groups and Cloud trails?
Log groups are used to logically organize and manage logs from AWS services and custom apps.

CloudTrail, on the other hand, captures API calls and account activity across AWS. It's used for auditing and security monitoring.

2. What is the purpose of creating S3 bucket policies?
The purpose of creating policies is to control the access to S3 bucket and its contents. It defines who (principals) can do what (actions) on which resources, and under what conditions.

3. How do you maintain the lifecycle of an S3 bucket?
To maintain the lifecycle of an S3 bucket, I configure lifecycle policies that automate object transitions between storage classes and delete data after a defined period

This helps manage costs and data retention — for example, I might move logs to Glacier after 30 days and delete them after 1 year. I typically define these rules using Terraform or the AWS Console, and align them with business compliance requirements."

4. What are Network ACLs and Security Groups, and how do they differ?
In AWS, Security Groups and Network ACLs (NACLs) are both used to control network traffic, but they serve different purposes and operate at different levels.

`Security Group` act as firewalls at the instance level - for EC2, RDS. They are stateful, meanining if you allow inbound traffic, the outbound traffic is automatically allowed, and vice versa. you can only define allow rules, not deny.

` NACLs`, on the other hand, operate at the subnet level. Theya are stateless, so you must explicitly define both inbound and outbound rules for bidirectional communication. NACLs support both allow and deny rules, making them useful for subnet-wide IP blocking or stricker control.

5. How do you configure AWS RDS, and what factors do you consider (size, requirements, etc.)?
    1. Choose the Database Engine
    Options include MySQL, PostgreSQL, MariaDB, Oracle, SQL Server, and Aurora
    2. Instance Size (Compute + Memory)
        Based on performance requirements:
        t3/t4 – for dev/test
        m6g/m5/r5 – for production workloads
    3. Storage Type
        General Purpose (gp3) – Balanced for most workloads
        Provisioned IOPS (io1/io2) – For I/O-intensive DBs
        Magnetic – Legacy, rarely used
    4. High Availability
    Enable Multi-AZ deployment for automatic failover (standby replica in another AZ)
    5. Network Configuration
        Place RDS in private subnets within a VPC

        Control access using:
        Security Groups
        Subnet NACLs
        IAM policies (for snapshot access)
    6. Backup and Maintenance
    Enable automated backups (retention: 7–35 days)

    7. Monitoring and Alerts
    8. Security Considerations
    Encrypt at rest with KMS
    Enable SSL in transit
    Restrict access to trusted IPs/VPCs
    Apply IAM authentication for users (PostgreSQL/MySQL)

6. How much data is stored in your RDS MySQL?
In production, our largest database holds approximately 150 GB of transactional data.

7. How many masters and slaves are in RDS?
In Amazon RDS, the standard setup supports one master (primary) instance and multiple read replicas (commonly referred to as slaves), depending on the database engine.
    You can have up to 5 read replicas per master in standard RDS MySQL.
In our setup, we use one RDS MySQL primary (master) for write operations and 2 read replicas for read-heavy reporting workloads. This helps distribute the load and also serves as a fallback in case of failover.

8. How comfortable with AWS and how much rate urself out of 5?
well, I'm very comfortable working with AWS — I've been using it extensively for over 4 years in a DevOps context. I've worked hands-on with services like:

EC2, S3, VPC, Security Groups, IAM, CloudWatch

EKS, ECS, ECR, and Load Balancers

RDS, DynamoDB, Lambda, Glue, Athena

I also handle multi-account access using IAM roles, set up monitoring and alerts with CloudWatch, and use S3 lifecycle policies, WAF, and Shield for security and cost optimization.
On a scale of 1 to 5, I would confidently rate myself 4.5/5 in AWS.

9. about RBAC 
RBAC (Role-Based Access Control) is a security mechanism that restricts system access based on the roles assigned to users or services. Rather than assigning permissions directly to individuals, you assign them to roles, and users are assigned to those roles.

10. What is log rotate job and how does it work
Log rotation is the process of automatically archiving, compressing, or deleting old log files to ensure that log directories don't consume too much disk space over time.
This is typically handled by tools like logrotate in Linux.
In one of my production systems, logs from an app running on EC2 grew over 2GB daily. I set up a logrotate job to rotate logs daily, compress them, and retain only the last 7 days to prevent disk space issues and to ensure older logs were still archived.

11. What is connection drain
Connection draining is the process of gracefully removing a backend server (like an EC2 instance or a pod) from a load balancer while allowing its existing active connections to complete, instead of terminating them immediately.

It's commonly used during:
    Deployments
    Auto-scaling
    Rolling updates
    Manual server removal

12. How to login VM if VM has private IP
If a VM has only a private IP and no public access, I typically use a bastion host (jump server) or VPN to securely access it from a public or management network

Session Manager (For AWS EC2)
In AWS, if SSM Agent is installed and IAM permissions are set, I use SSM Session Manager to connect to private instances without opening SSH ports

13. Types of load balancers
1. ALB
2. NLB
3. GWLB

14. How many NAT Gateways are needed for two public & two private subnets n a single VPC? Min & max?
So, we have two public and two private subnets
    one subnet is minimum but not enough as it not provide HA so have two private subnets in different Azs

15. Explain TTL in DNS- how does it work, and when do we use it? Explain the Flow.
TTL (Time To Live) in DNS is a setting that tells how long a DNS resolver should cache a DNS record before querying the authoritative DNS server again.

Flow:
A user tries to access example.com
Their local DNS resolver (e.g., ISP or OS cache) checks if it already has a cached record
If not, it queries the authoritative name server for the DNS record (e.g., A record → IP)
The record is returned with a TTL value, e.g., TTL = 300 (5 minutes)
The resolver caches the record for 5 minutes
Any future requests during this TTL will use the cached value
After TTL expires, the resolver will query the DNS server again for a fresh record

    This means: cache example.com → 192.168.1.10 for 300 seconds (5 min)

16. How does weighted routing work in LB?
Lb is useful for distributing the traffic across the instances, nodes. Weighted routing in a load balancer means traffic is distributed between multiple backend targets based on assigned weight rather than evenly. 

Each backend (server, pod, or version) is assigned a weight, and the load balancer distributes traffic proportionally.
    If you have two backends:
    Backend A: weight 80
    Backend B: weight 20
Use AWS Route53 or ALB to do weight routing

17. How does SSL work (Certbot, Let's Encrypt, AWS)? Explain the Flow.
SSL (Secure Sockets Layer), now replaced by TLS (Transport Layer Security), is a cryptographic protocol that ensures secure, encrypted communication between a client (browser) and a server.

    1. Client initiates HTTPS request to the server (e.g., a web app)
    2. Server responds with its SSL certificate, which contains:
        Public key
        Domain name
        Certificate Authority (CA) signature
    3. Client verifies the certificate using the CA's public key
    4. If valid, client and server use the public key to securely exchange a shared session key
    5. All further communication is encrypted with this session key

18. diff between IAM Users and Roles
Both IAM Users and IAM Roles are AWS Identity and Access Management (IAM) entities used to manage access permissions, but they are designed for different use cases.
IAM User
    An IAM User represents a permanent identity tied to a real human (or service) and has:
    A username
    Long-term credentials (password, access keys)
    Direct access to AWS services based on assigned policies
IAM Role
An IAM Role is a temporary identity that can be assumed by users, services, or other AWS accounts. It does not have permanent credentials.
Provides temporary security credentials
In our setup, we use IAM Users only for admins with MFA and strong password policies.
All EC2 instances and Lambda functions assume IAM Roles to access services like S3 and DynamoDB — this avoids hardcoding credentials and improves security.

19. Can you avoid the specific port traffic using SGs?
Security Groups (SGs) in AWS are stateful, allow-only firewalls. You can't explicitly deny traffic, but you can avoid traffic on specific ports by not allowing them at all.

20. What are the best password security practices used by your organisation?
1. Enforce Strong Password Policies
    Minimum length: 12+ characters
    Requires a mix of uppercase, lowercase, numbers, and special characters
    Disallows common or dictionary-based passwords
2. Enforce Multi-Factor Authentication (MFA)
    MFA is mandatory for all cloud portals, VPNs, admin tools, and code repositories.
    Supports TOTP apps (Google Authenticator, Duo, Okta) or hardware keys (YubiKey).   
3. Avoid Hardcoding Passwords
    Secrets are never hardcoded in code or CI/CD pipelines.
    We use Secrets Managers like:
    AWS Secrets Manager
    Azure Key Vault
    HashiCorp Vault 
4. Password Rotation & Expiry
    Admin and privileged account passwords are rotated every 30–60 days.
    Automated tools like AWS Secrets Manager rotation are used where possible.

21. What are the security parameters we must consider while we are creating an EC2 instance for production?
    Security parameters for EC2 in production include:
    Using security groups to control traffic
    Implementing IAM roles
    Enabling encryption for EBS volumes
    Regular security updates and patches
    Monitoring and logging

22. How can you protect the data in an AWS instance?
I protect data on an AWS instance by:
    Encrypting data at rest and in transit
    Restricting access using IAM, SGs, and VPC
    Using SSM over SSH
    Monitoring and auditing activity
    Taking regular, encrypted backups
This layered approach ensures both compliance and resilience against data breaches or loss.

23. How can you connect from AWS to on-prem servers?

24. Explain about the transit gateway and why do we use this?
    AWS Transit Gateway
    Acts as a central hub for routing between multiple VPCs and on-prem locations.

    ✅ Use Case:
    When connecting multiple VPCs and on-prem networks
    Supports both Direct Connect and VPN attachments
    Simplifies complex mesh network topologies

25. I have created an EC2 instance named A, and I want to create another instance B. It should create an instance without deleting instance A. What can I do during this?

You can create a new EC2 instance B by using a different resource name in Terraform or by creating a new instance in the AWS Console without affecting instance A.

26. How does the GSLB load balancer work?
GSLB (Global Server Load Balancer) is a DNS-based load balancing solution that distributes traffic across multiple geographically distributed data centers or cloud regions. Its main goal is to provide high availability, disaster recovery, and low-latency routing based on various factors.

🌍 How GSLB Works:
    Client makes a DNS request for a service (e.g., app.example.com)
    The GSLB system intercepts the DNS query and decides which data center or region should serve the request
    GSLB returns the IP address of the optimal site/server
    The client connects directly to that location

27. Can we connect two different VM that are in a different Vnet.
Yes, you can connect two VMs in different VNets, but they cannot communicate by default. You need to explicitly configure connectivity between the VNets 

28. Private Endpoints.
An endpoint is simply the entry point or destination through which a client communicates with a service, application, or resource over a network.
In cloud and networking contexts, an endpoint typically refers to a URL, IP address, or DNS name that is used to access a specific service.

29. Use of Route53
Amazon Route 53 is used for:

    Registering domains
    Routing internet traffic to AWS resources
    Smart DNS routing (weighted, latency, geo)
    Health checks & failover
    Managing private/internal DNS in VPCs
    It plays a critical role in building highly available, globally distributed systems on AWS.

30. Difference between S3 and EBS
Amazon S3 (Simple Storage Service) and Amazon EBS (Elastic Block Store) are both storage services offered by AWS, but they serve different purposes and are optimized for different access patterns and use cases.

Use Amazon S3 for object-based storage (files, media, backups) that is accessed via API or web.

Use Amazon EBS for block-level storage that behaves like a virtual disk and is mounted to EC2 for low-latency access.

31. What is the difference between vertical and horizontal scaling?
The key difference between vertical scaling and horizontal scaling lies in how resources are added to handle increased load or demand.
1. Vertical Scaling (Scale Up/Down)
Involves adding more resources (CPU, RAM, storage) to a single server or instance.
2. Horizontal Scaling (Scale Out/In)
Involves adding more instances/servers to distribute load.

32. Explain OSI models
The OSI Model breaks down network communication into 7 logical layers — helping engineers understand, design, and troubleshoot complex networks in a modular way.

33. How do you did cost optimization in AWS?
Cost optimization strategies include:

📏 Right-sizing EC2 instances
💳 Using Reserved Instances and Savings Plans
📈 Implementing auto-scaling
🎯 Using Spot Instances for non-critical workloads
💾 Optimizing storage (S3 lifecycle policies, EBS volume management)
🔍 Regular review of unused resources
📊 Using AWS Cost Explorer and Budgets for monitoring

34. How do you implement best security policies on AWS?
1. Identity & Access Management (IAM) Best Practices
    Follow the principle of least privilege: grant only required permissions
    Use IAM roles instead of IAM users for EC2, Lambda, etc.
    Enforce Multi-Factor Authentication (MFA) for all users
2. Data Protection (At Rest and In Transit)
    Enable encryption at rest using AWS-managed or customer-managed KMS keys (S3, EBS, RDS, DynamoDB)
    Force TLS/HTTPS for all data in transit
    Use Secrets Manager or Parameter Store to securely manage sensitive data
    Enable S3 Block Public Access and bucket policies to control access
3. Network Security
    Isolate workloads using VPCs, private subnets, and NACLs
    Use Security Groups as virtual firewalls with tight inbound/outbound rules 
4. Monitoring & Auditing
    Enable CloudTrail across all regions and send logs to S3
    Use CloudWatch Logs & Metrics for real-time monitoring and alerts

35. how many types of policy, IAM policy are there? IAM policies?
IAM Policies are JSON documents that define permissions in AWS — specifying what actions are allowed or denied, on which resources, and under what conditions.

They are attached to: IAM Users IAM Groups IAM Roles

Policy Type	           Attached To	                   Used For
Identity-based	IAM Users, Groups, Roles	Most common access control
Resource-based	AWS Resources (e.g., S3)	Sharing resources across accounts
Permissions boundaries	IAM Users, Roles	Limit maximum allowed permissions
Session policies	Temporary credentials	Scoped, time-limited access

36. what is the difference between the S3 bucket policies and acls?
Both S3 Bucket Policies and ACLs (Access Control Lists) are used to manage access to Amazon S3 resources, but they differ in control level, flexibility, and use cases.

S3 Bucket Policies:
    Are JSON-based resource policies applied at the bucket or object level
    Support fine-grained access control, including:
    Conditions (IP, VPC, time)
    Cross-account access
    AWS principals
S3 ACLs (Access Control Lists):
    Are legacy access control mechanisms
    Applied directly to individual buckets or objects
    Grant basic read/write permissions to specific AWS accounts or predefined groups (like AllUsers or AuthenticatedUsers)
    Don't support complex conditions or policy logic
AWS recommends using bucket policies and IAM policies over ACLs for all modern use cases. 

37. Expalin about fargate?
AWS Fargate is a serverless compute engine for containers that works with Amazon ECS (Elastic Container Service) and Amazon EKS (Elastic Kubernetes Service).
With Fargate, you don't need to provision or manage EC2 instances — AWS automatically runs containers for you based on CPU and memory requirements.

38. what is cloud watch uses cases.
Amazon CloudWatch is a monitoring and observability service that provides real-time visibility into resource utilization, application performance, and operational health.
It's commonly used for metrics, logs, alarms, dashboards, and automated responses.

1. Monitor EC2, RDS, Lambda, and other AWS resources
    Tracks CPU, memory, disk, network, and other performance metrics
    Auto-collects default metrics for most AWS services
    Helps identify bottlenecks and optimize resources
2. Log Collection and Analysis (CloudWatch Logs)
    Collects logs from:
    EC2 instances (via CloudWatch Agent)
    Lambda functions
    ECS tasks and containers
    Custom applications
3. Real-Time Alerting (CloudWatch Alarms)
Create alarms on metrics (e.g., CPU, memory, custom metrics)
4. Unified Dashboards for Visualization
    Build real-time visual dashboards with metrics and alarms
    Combine metrics from multiple services (e.g., EC2, Lambda, RDS)

39. what is limitations of lamda
AWS Lambda is a powerful serverless compute service, but it has certain limitations in terms of execution time, memory, package size, and environment constraints. These limits are important to consider when designing scalable, fault-tolerant serverless applications.
1. Execution Time (Timeout)
Maximum timeout: 15 minutes (900 seconds)
2. Memory and CPU
Memory allocation: 128 MB to 10,240 MB (10 GB)
3. Package Size Limits
Deployment package (zip):
50 MB (direct upload via CLI/API)
250 MB (uncompressed in /tmp)
Layer limit: Up to 5 layers, each max 50 MB (compressed)
4.Stateless, no GPU, and limited persistent storage

40. how lamda works containers
AWS Lambda supports running functions as container images. Instead of uploading code as ZIP files, you can package your Lambda as a Docker container image (up to 10 GB) and deploy it. This provides more flexibility in dependencies, tooling, and language support.

41. storage gateway in AWS
AWS Storage Gateway is a hybrid cloud storage service that enables on-premises applications to seamlessly use AWS cloud storage.
It acts as a bridge between on-prem infrastructure and AWS, allowing you to securely store data in the cloud while keeping frequently accessed data locally cached.

42. how you ensure the best possible security for high availability architectures for 3 tier applications.
To secure a high-availability 3-tier application architecture (Web, App, and DB tiers), I use layered security principles — combining network isolation, identity management, encryption, logging, and proactive threat detection across all tiers and environments.
🔹 1. Network Layer Security
Use VPC/Subnets to isolate tiers (web in public, app/db in private)
Security Groups:
    Web → App: allow only required ports (e.g., 443 → 8080)
    App → DB: strict DB port (e.g., 5432)
NACLs to add subnet-level controls
WAF in front of ALB/NLB to filter malicious traffic (SQLi, XSS)


🔹 2. Access Control & IAM
Use IAM roles and least-privilege policies
    App can read secrets, but not write
    EC2 roles scoped per tier
Enforce MFA and role assumption for engineers

🔹 3. Application Layer Security
    Use input validation and sanitization (against SQLi/XSS)
    Implement JWT or OAuth2 with expiration and signature validation
    Rotate secrets using AWS Secrets Manager or Vault
    Run SAST and DAST tools during CI/CD (e.g., SonarQube, OWASP ZAP)
🔹 4. Data Security
Encrypt at rest using KMS or CMK (DBs, S3, EBS)
Encrypt in transit using HTTPS/SSL and DB-level SSL

🔹 5. CI/CD & Image Security
Use image scanning (Trivy/Snyk) before deployment
Use signed and verified Docker images
Implement GitOps and approval gates for prod

🔹 6. Monitoring, Auditing, and Threat Detection
Enable VPC Flow Logs, CloudTrail, and Config
Use GuardDuty, Inspector, and Security Hub
Implement centralized logging (e.g., FluentBit → ELK or CloudWatch)


43. I'm an admin but I don't have access to the S3 bucket ? 
"Even as an admin, S3 access can be blocked by bucket policies, encryption settings, and Block Public Access. I'd review the bucket policy, check for explicit denies or conditions, use the IAM Policy Simulator, and verify if any KMS key policies or VPC/IP conditions are restricting access."


44. What are the top 5 infra attacks, and how do you mitigate them?- notAnswered

45. EC2 instance is unreachable, and it's not a security group issue. What's your next step?
1. Instance State and Status Checks
    ✅ Verify instance is in "Running" state
    ✅ Check system and instance status checks
    ✅ Review recent instance state changes
    ✅ Check system events for any issues
2. Network Configuration
    🌐 Verify public/private IP assignment
    🌐 Confirm subnet configuration (public/private)
    🌐 Check route table settings
    🌐 Verify Internet Gateway attachment (if needed)
3. System Logs
    📋 Access instance system logs
    📋 Review boot sequence
    📋 Check for system-level errors
    📋 Verify initialization completion
4. User Data Script
    📝 Review user data script content
    📝 Check script execution status
    📝 Look for script-related errors
    📝 Verify script completion
5. IAM and Permissions
    🔑 Verify IAM role attachment
    🔑 Check role permissions
    🔑 Review permission boundaries
    🔑 Ensure necessary AWS service access
6. Storage and Volumes
    💾 Check EBS volume attachment
    💾 Verify volume health
    💾 Monitor disk space
    💾 Review I/O performance
7. Network ACLs
    🔒 Review subnet NACL rules
    🔒 Verify required port access
    🔒 Check for explicit deny rules
    🔒 Validate both inbound/outbound rules
If none of these steps resolve the issue, you might want to consider: Creating an AMI of the instance for backup Launching a new instance from the AMI

46. An S3 bucket was made public by mistake. How do you secure and audit it?
Steps to secure and audit a public S3 bucket:

🔒 Remove public access settings

Audit: Identify What Was Exposed and When Use CloudTrail to check: Who made the bucket public Any PutBucketPolicy, PutBucketAcl, or object-level ACL actions Use S3 Server Access Logs or CloudTrail Lake to check: What files were accessed and from where

📜 Review and update bucket policies
📝 Enable versioning
🔐 Enable server-side encryption
🔍 Use AWS Config to monitor bucket settings
🗑️ Review and remove any public ACLs


47. RDS migration with minimal downtime – how would you approach it?
🔹 1. Pre-Migration Planning
Audit current RDS instance:
    Engine/version, storage type, performance metrics
Choose target RDS instance (same or upgraded engine/version
Ensure network connectivity (VPC/subnet/SG rules match)

✅ Tools: AWS DMS (Database Migration Service), native replication, dump/load for small DBs

🔹 2. Set Up Target RDS Instance
Create a new RDS instance with compatible schema and parameters
Apply necessary tuning and parameter groups
Ensure backups, monitoring, and IAM roles are set
✅ Use blue-green deployment to prepare it in parallel

48. How would you set up networking in vpc
🔹 1. Create a VPC
    Choose a CIDR block (e.g., 10.0.0.0/16) – large enough to subnet
    Enable DNS resolution and hostnames for EC2, EKS, etc.

🔹 2. Create Subnets (Multi-AZ)
    Public Subnets (for ALB, Bastion, NAT):
    10.0.1.0/24 in AZ1
    10.0.2.0/24 in AZ2

    Private Subnets (for EC2, RDS, EKS nodes):
    10.0.11.0/24 in AZ1
    10.0.12.0/24 in AZ2
✅ Subnets in multiple AZs support high availability

🔹 3. Set Up Route Tables
    Public Route Table:
        Associate with public subnets
        Add route: 0.0.0.0/0 → Internet Gateway

    Private Route Table:
        Associate with private subnets
        Add route: 0.0.0.0/0 → NAT Gateway
🔹 4. Internet Gateway (IGW)
    Create and attach to VPC
    Enables internet access for public subnets
🔹 5. NAT Gateway
    Place in public subnet for outbound internet access from private subnets (e.g., EC2 pulling updates)
    ✅ Allocate Elastic IP for the NAT
🔹 6. Configure Security Groups (SGs)
    Stateless virtual firewalls at instance level

    Allow:
    Inbound: ALB → EC2 (port 80/443)
    EC2 → RDS (port 5432)
    Outbound: allow required external traffic
🔹 7. Configure NACLs (Network ACLs)
    Optional, subnet-level firewall
    Allow/Deny rules — stateless


49. How you will direct traffic to and from a instance in private subnet

To enable outbound traffic, I use a NAT Gateway in a public subnet and route traffic from the private subnet through it. For inbound access, I use a Bastion Host or SSM. For app-level access, I expose an ALB in the public subnet which forwards traffic to EC2s in the private subnet. This keeps the infrastructure secure while still functional.

50. how did you managed security for application level? 
I implemented application-level security using a layered approach — including authentication, authorization, encryption, input validation, secure headers, and dependency scanning — integrated within CI/CD to enforce security throughout the SDLC."

51. What types of nodes did you deploy in AWS?
"In AWS, I've deployed various types of nodes depending on the workload — including EC2 instances for traditional workloads, EKS worker nodes (managed and Fargate), RDS instances, and auto-scaling groups behind load balancers. I choose instance types and node types based on compute, memory, storage, and scalability requirements."

52. What is the difference between Interface Endpoint and Gateway Endpoint?
The key difference is that Interface Endpoints use private IPs and Elastic Network Interfaces (ENIs) to connect to AWS services over the VPC network, while Gateway Endpoints update your route tables to route traffic to specific services like S3 and DynamoDB without using NAT or public IPs."

53. How did you set up ECS using EC2 instances?
To set up ECS using EC2:

🏗️ Create an ECS cluster
💻 Launch EC2 instances into the cluster
📝 Define task definitions and services
🚀 Deploy applications using ECS

54. Can't we configure Route 53?
Common reasons for not being able to configure Route 53 include:

🔒 Insufficient IAM permissions
⚙️ Incorrect DNS settings
🌐 Domain not registered with Route 53

55. If we want to configure third-party domains like GoDaddy in Route 53, how do we do that?
To configure a third-party domain with Route 53:

Create a Public Hosted Zone in Route 53 ✅ This will generate 4 Name Server (NS) records automatically.
Copy Route 53 Name Server Records
Update the domain's nameservers in godaddy
Create DNS Records in Route 53

"To configure a GoDaddy (or third-party) domain with Route 53, I first create a public hosted zone in Route 53, copy the AWS name servers, and update them in GoDaddy's DNS settings. Then I create the required A, CNAME, or Alias records in Route 53 to point traffic to my AWS resources like ALB, EC2, or S3 static websites."


56. What is the difference between AWS Config and AWS CloudTrail?
AWS CloudTrail logs every API call made in your account — including who made it, when, and from where. AWS Config continuously records the configuration state of resources and shows how they've changed over time. I use CloudTrail for auditing and security investigations, and AWS Config for compliance monitoring, drift detection, and historical resource tracking.

🔍 AWS Config Example Use Cases
    Track if an S3 bucket had public access enabled
    Detect when an EC2 instance type was changed
    Enforce compliance via AWS Config Rules (e.g., "EBS volumes must be encrypted")

🔍 AWS CloudTrail Example Use Cases
    Audit: "Who deleted an EC2 instance?"
    Investigate: "Which user accessed Secrets Manager at 2AM?"
    Monitor for unauthorized API calls



57. What are the node groups you used in AWS EKS?
Node groups in EKS are managed groups of EC2 instances that run Kubernetes nodes. They simplify the management of nodes in an EKS cluster.

58. What are the types of node groups in AWS EKS?
n AWS EKS, there are two main types of node groups: Managed Node Groups and Self-Managed Node Groups. Additionally, AWS offers Fargate Profiles for serverless Kubernetes pods. 

59. If a user wants to access the S3 bucket, what are the processes?

If a user wants to access an S3 bucket, the access depends on who the user is (IAM or external) and how the bucket is configured (private, cross-account, public, etc.).

"To allow a user to access an S3 bucket, I either attach an IAM policy (for same-account users), configure a bucket policy (for cross-account access), or use a pre-signed URL (for temporary access). I ensure that access is secure and follows the principle of least privilege.


Step 1: Attach an IAM Role to EC2
  a. create a role 
   ```bash
    {
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:PutObject",
      "Resource": "arn:aws:s3:::your-bucket-name/*"
    }
  ]
 }
```

60. How does VPC Peering work?
VPC peering is a networking connection between two VPCs that enables you to route traffic between them using private IP addresses.


61. How does Transit Gateway work and how did you configure it?
AWS Transit Gateway (TGW) acts as a central hub to connect multiple VPCs, on-premises networks and other AWS accounts via a scalable and simplified routing architecture.

🧭 How Transit Gateway Works
    It enables centralized routing between:
    VPCs in the same or different accounts
    AWS Direct Connect or Site-to-Site VPNs
    AWS Regions (via inter-region peering)

✅ 1. Create Transit Gateway
```bash
aws ec2 create-transit-gateway \
  --description "TGW for shared services" \
  --options AmazonSideAsn=64512
```
✅ 2. Attach VPCs to TGW
✅ 3. Update Route Tables in VPC
✅ 4. Configure Transit Gateway Route Table

Transit Gateway allowed me to simplify VPC-to-VPC communication across multiple accounts. I configured a TGW, attached each VPC using subnets in different AZs, updated their route tables, and managed segmentation using Transit Gateway route tables. This replaced dozens of peering connections and improved scalability and security.

62. If we connect VPCs to the Transit Gateway, what will you update in the VPC Route Table?
When you connect VPCs to a Transit Gateway (TGW), you must update the VPC route tables to route traffic to the Transit Gateway for any destination CIDRs (usually other VPCs or on-prem networks).

"When I attach a VPC to a Transit Gateway, I update the route table in each VPC to point the destination CIDR of peer VPCs to the TGW ID. I also ensure the TGW route table maps the CIDRs to the correct VPC attachments, and that security groups are configured to allow traffic between them."

63. For all VPCs, will you configure the Transit Gateway attachment with CIDR range?
No, we don't manually configure CIDR ranges during Transit Gateway attachment. The VPC's CIDR is automatically associated with the attachment. We control routing via the Transit Gateway route table, which maps destination CIDRs to the appropriate VPC attachment. This ensures that each VPC can route traffic to the correct peer via TGW.


65. What is WAF (Web Application Firewall) and AAF (Application Access Firewall)?
WAF is used to filter and block malicious web traffic at Layer 7 to protect apps from attacks like SQL injection and XSS. AAF, while not an AWS-native service, usually refers to access control at the application level — such as user identity checks, RBAC, or context-based access — often implemented with identity-aware proxies or API gateways

66. What is VPC Flow Logs and how will you track the IPs hitting the VPC?
VPC Flow Logs capture information about IP traffic going to and from network interfaces in your VPC. You can analyze these logs using CloudWatch Logs or Athena

67. How to filter a particular IP from AWS CloudWatch Log Group?
To filter logs for a specific IP in AWS CloudWatch, I use Logs Insights with a regex filter on @message

68. If you are storing logs in S3 Bucket, how will you track that particular IP?
To track IP addresses accessing logs stored in an S3 bucket, you need to enable access logging and monitoring tools

Enable S3 Server Access Logging 🛠 Steps: Go to your S3 bucket (e.g., log-storage-bucket) Go to Properties → Server access logging Enable logging and choose a target bucket (e.g., log-access-audit-bucket) Important: The target bucket must not be the same as the source bucket.

CloudTrail (for API-level tracking) Enable AWS CloudTrail to record S3 data events Steps: Go to CloudTrail > Trails > Create trail Enable Data events → S3 → log-storage-bucket Logs go to another S3 bucket or CloudWatch for analysis

69. How do you take the backup of AWS Services?
In AWS, backups can be taken using native backup tools like AWS Backup, service-specific snapshots, or third-party automation. 
1. EC2 Instances
Use EBS Snapshots (volume-level backup)
🔹 2. RDS (Relational Databases)
Automated Backups (enabled by default)
Manual Snapshots (for point-in-time recovery)
🔹 3. DynamoDB
On-demand Backups:
🔹 4. S3 Buckets
Versioning-enabled buckets (object-level rollback)
Cross-Region Replication (CRR) for disaster recovery
Use AWS Backup or scheduled scripts (e.g., sync to Glacier or another bucket)

70. I need to send a logs from EC2 to S3, create an automation part where we need to take the log file, check the CPU metrics and send an alarm through cloud watch to user's.
    ✅ Step 1: Create an IAM Role for EC2 with Required Permissions Policy Permissions: S3: PutObject CloudWatch: PutMetricData, GetMetricStatistics SNS: Publish Logs: Optional, if using CloudWatch Logs

    ✅ Step 2: Install and Configure CloudWatch Agent for CPU Monitoring

    ✅ Step 3: Create CloudWatch Alarm for CPU Usage

    ✅ Step 4: Create SNS Topic and Subscribe Email

    ✅ Step 5: Automate Log File Upload via Cron

71. If thre is an instance we have security group and web application firewall enabled, DDOS attack enabled will it protect from Bot attack.
Having Security Groups, WAF, and AWS Shield provides layered protection, but they don't fully protect against bot attacks on their own. For that, I use AWS WAF's Bot Control managed rule group or integrate third-party tools like Cloudflare Bot Management. I also implement CAPTCHA, request throttling, and JavaScript challenges to stop sophisticated bots.

72. What is DNS Resolution. Suppose I have new system with no cache what happens in the background.Step by step process.
DNS resolution is the process of converting a human-readable domain name (like example.com) into an IP address (like 93.184.216.34). If your system has no cached records, the resolver will query external DNS servers in a specific order until it finds the correct IP

🧩 Step 1: Application Layer Request
    Your browser (or OS) checks the local DNS cache.
    ✅ Since it's a new system, there's no local cache.
🌐 Step 2: Request Sent to Recursive Resolver (DNS Client → Resolver)
    Your system contacts the configured DNS resolver (usually your ISP's or public one like 8.8.8.8 or 1.1.1.1).
    This recursive DNS resolver takes responsibility for resolving the name fully and returning the IP.

🧠 Step 3: Query Root Name Server
    The resolver asks a Root DNS server:
        "Where can I find .com domain names?"

    Root server responds with the address of the TLD server for .com
🏛️ Step 4: Query TLD (Top-Level Domain) Server
    The resolver asks the .com TLD server:
    "Where is example.com hosted?"

    TLD server responds with the Authoritative Name Server for example.com

🔒 Step 5: Query Authoritative Name Server
    The resolver contacts the authoritative DNS server (e.g., ns1.exampledns.com)
    "What is the IP address of www.example.com?"
📦 Step 6: Response Returned to Original Client
    Resolver returns the IP address to your OS
    Your OS provides it to the browser
    Browser initiates a connection to 93.184.216.34

    ✅ Your system now caches this response for future use (TTL = Time to Live).

`Browser → OS Cache → DNS Resolver → Root Server → TLD Server → Authoritative DNS → IP Address`


73. If you're migrating a monolithic application from on-prem to Cloud and the system has its local file system, which file system you will use in AWS.
For a monolithic application relying on a local file system, I would use Amazon EFS (Elastic File System) if I need a scalable, shared file system with NFS compatibility across multiple instances. If only one instance needs access and performance is critical, I might use Amazon EBS instead."

74. How will you store all the configurations related to your monolithic app in Cloud,
I store application configurations in centralized and secure services like AWS Systems Manager Parameter Store or AWS Secrets Manager. This ensures externalized, versioned, and encrypted configuration management across environments — separate from the application code."

75.How would you secure an EC2 instance against unauthorized SSH access?

- Restrict SSH at the Network Level 🔸 Security Group (SG) Only allow port 22 from trusted IPs
- Disable Root Login Avoid direct root access. Create a non-root user (ec2-user, ubuntu) with sudo access.
- Use IAM + EC2 Instance Connect or AWS Systems Manager

76. You need to connect your DB running in private subnet, not using NAT gateway or NAT instance or bastion host, what are the other options,
"If I can't use a NAT Gateway, NAT instance, or bastion host, I'll use AWS Systems Manager Session Manager to connect to a managed EC2 instance inside the VPC that can access the private DB. I can also use VPC Interface Endpoints, AWS Client VPN, or SSM port forwarding to securely connect to the database without exposing it to the public or depending on traditional network hops."

77. what is tcp and udp
TCP (Transmission Control Protocol) and UDP (User Datagram Protocol) are two core protocols of the transport layer in the TCP/IP model. They are used to send data between devices over a network, but they work differently in terms of reliability, speed, and connection."

TCP:
Connection-oriented: Establishes a connection before transferring data 
Reliable: Ensures all data is received and in order
Use Cases:
    Web browsing (HTTP/HTTPS)
    Email (SMTP, IMAP, POP3)
    File transfer (FTP)
    SSH
UDP (User Datagram Protocol)
✅ Key Features:
    Connectionless: Sends data without a handshake
    Unreliable but faster: No guarantee of delivery or order
    No retransmission, no acknowledgment
Use Cases:
    Live video/audio streaming
    DNS queries
    Online gaming

TCP is a connection-based protocol that guarantees delivery and order of packets — ideal for web and database communication. UDP is a faster, connectionless protocol used where speed matters more than reliability, like video streaming or DNS lookups."


78. what is ipv4 and ipv6
IPv4 and IPv6 are versions of the Internet Protocol used to identify devices on a network

Pv4 is the traditional addressing system using 32-bit addresses and supports about 4.3 billion devices. Due to address exhaustion, IPv6 was introduced with 128-bit addresses, providing a vastly larger pool and built-in features like IPsec, efficient routing, and no NAT. While IPv4 is still dominant, IPv6 adoption is growing.
79. what is pid

80. what metrics is used to monitor ec2 instance cpu, memory in aws
In AWS, we use Amazon CloudWatch to monitor EC2 metrics. By default, CloudWatch provides CPU-related metrics, and memory metrics require custom configuration. These metrics help track performance, utilization, and health of EC2 instances."

81. Sending log files from EC2 to S3 – what are the steps?
Steps to send logs from EC2 to S3:

    🔑 Create an IAM role with S3 write permissions
    🔗 Attach the role to the EC2 instance
    📤 Use AWS CLI or SDK to upload logs to S3
    ⏰ Set up a cron job or use CloudWatch Logs agent to automate the process
82. Difference between subnet and NACL?
    Subnets are segments of a VPC's IP address range
    NACLs (Network Access Control Lists) are stateless firewalls that control traffic at the subnet level

83. Difference between NAT Gateway and Internet Gateway.
    Internet Gateway allows resources in a VPC to connect to the internet
    NAT Gateway allows resources in a private subnet to connect to the internet while maintaining their private IP
84. Difference between SGs and NACLs.
    Security Groups are stateful and operate at the instance level
    NACLs are stateless and operate at the subnet level
85. Where do you use firewalls, SGs, and NACLs?
    Firewalls: On-premises or in the cloud for network security
    Security Groups: For instance-level security in AWS
    NACLs: For subnet-level security in AWS
86. How do you connect private subnet to the internet?
    To connect a private subnet to the internet, you need:

    1.🌐 A NAT Gateway in a public subnet
    2. 🗺️ Route tables configured to route traffic from the private subnet to the NAT Gateway

    "To connect a private subnet to the internet, I create a NAT Gateway in a public subnet and update the private subnet's route table to forward internet-bound traffic through the NAT. This allows outbound internet access while keeping the subnet private. For more control, a NAT instance can also be used, but NAT Gateway is preferred for scalability and maintenance."

87. Design HA backend using AWS services.
For a highly available backend, use:

🌐 Multiple AZs
📈 Auto-scaling groups
⚖️ Load balancers
📊 RDS with Multi-AZ deployment
🚀 ElastiCache for caching