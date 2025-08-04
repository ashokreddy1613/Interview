# AWS Interview Questions and Answers

## 1. In CloudWatch, what is the use of log groups and Cloud trails?
Log groups are used to logically organize and manage logs from AWS services and custom apps.

CloudTrail, on the other hand, captures API calls and account activity across AWS. It's used for auditing and security monitoring.

## 2. What is the purpose of creating S3 bucket policies?
The purpose of creating policies is to control the access to S3 bucket and its contents. It defines who (principals) can do what (actions) on which resources, and under what conditions.

## 3. How do you maintain the lifecycle of an S3 bucket?
To maintain the lifecycle of an S3 bucket, I configure lifecycle policies that automate object transitions between storage classes and delete data after a defined period

This helps manage costs and data retention — for example, I might move logs to Glacier after 30 days and delete them after 1 year. I typically define these rules using Terraform or the AWS Console, and align them with business compliance requirements."

## 4. What are Network ACLs and Security Groups, and how do they differ?
In AWS, Security Groups and Network ACLs (NACLs) are both used to control network traffic, but they serve different purposes and operate at different levels.

`Security Group` act as firewalls at the instance level - for EC2, RDS. They are stateful, meanining if you allow inbound traffic, the outbound traffic is automatically allowed, and vice versa. you can only define allow rules, not deny.

` NACLs`, on the other hand, operate at the subnet level. Theya are stateless, so you must explicitly define both inbound and outbound rules for bidirectional communication. NACLs support both allow and deny rules, making them useful for subnet-wide IP blocking or stricker control.

## 5. How do you configure AWS RDS, and what factors do you consider (size, requirements, etc.)?
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

## 6. How much data is stored in your RDS MySQL?
In production, our largest database holds approximately 150 GB of transactional data.

7. How many masters and slaves are in RDS?
In Amazon RDS, the standard setup supports one master (primary) instance and multiple read replicas (commonly referred to as slaves), depending on the database engine.
    You can have up to 5 read replicas per master in standard RDS MySQL.
In our setup, we use one RDS MySQL primary (master) for write operations and 2 read replicas for read-heavy reporting workloads. This helps distribute the load and also serves as a fallback in case of failover.

## 8. How comfortable with AWS and how much rate urself out of 5?
well, I'm very comfortable working with AWS — I've been using it extensively for over 4 years in a DevOps context. I've worked hands-on with services like:

EC2, S3, VPC, Security Groups, IAM, CloudWatch

EKS, ECS, ECR, and Load Balancers

RDS, DynamoDB, Lambda, Glue, Athena

I also handle multi-account access using IAM roles, set up monitoring and alerts with CloudWatch, and use S3 lifecycle policies, WAF, and Shield for security and cost optimization.
On a scale of 1 to 5, I would confidently rate myself 4.5/5 in AWS.

## 9. about RBAC 
RBAC (Role-Based Access Control) is a security mechanism that restricts system access based on the roles assigned to users or services. Rather than assigning permissions directly to individuals, you assign them to roles, and users are assigned to those roles.

## 10. What is log rotate job and how does it work
Log rotation is the process of automatically archiving, compressing, or deleting old log files to ensure that log directories don't consume too much disk space over time.
This is typically handled by tools like logrotate in Linux.
In one of my production systems, logs from an app running on EC2 grew over 2GB daily. I set up a logrotate job to rotate logs daily, compress them, and retain only the last 7 days to prevent disk space issues and to ensure older logs were still archived.

## 11. What is connection drain
Connection draining is the process of gracefully removing a backend server (like an EC2 instance or a pod) from a load balancer while allowing its existing active connections to complete, instead of terminating them immediately.

It's commonly used during:
    Deployments
    Auto-scaling
    Rolling updates
    Manual server removal

## 12. How to login VM if VM has private IP
If a VM has only a private IP and no public access, I typically use a bastion host (jump server) or VPN to securely access it from a public or management network

Session Manager (For AWS EC2)
In AWS, if SSM Agent is installed and IAM permissions are set, I use SSM Session Manager to connect to private instances without opening SSH ports

## 13. Types of load balancers
1. ALB
2. NLB
3. GWLB

## 14. How many NAT Gateways are needed for two public & two private subnets n a single VPC? Min & max?
So, we have two public and two private subnets
    one subnet is minimum but not enough as it not provide HA so have two private subnets in different Azs

## 15. Explain TTL in DNS- how does it work, and when do we use it? Explain the Flow.
TTL (Time To Live) in DNS is a setting that tells how long a DNS resolver should cache a DNS record before querying the authoritative DNS server again.

Flow:
- A user tries to access example.com
- Their local DNS resolver (e.g., ISP or OS cache) checks if it already has a cached record
- If not, it queries the authoritative name server for the DNS record (e.g., A record → IP)
- The record is returned with a TTL value, e.g., TTL = 300 (5 minutes)
- The resolver caches the record for 5 minutes
- Any future requests during this TTL will use the cached value
- After TTL expires, the resolver will query the DNS server again for a fresh record

    This means: cache example.com → 192.168.1.10 for 300 seconds (5 min)

## 16. How does weighted routing work in LB?
Lb is useful for distributing the traffic across the instances, nodes. Weighted routing in a load balancer means traffic is distributed between multiple backend targets based on assigned weight rather than evenly. 

Each backend (server, pod, or version) is assigned a weight, and the load balancer distributes traffic proportionally.
    If you have two backends:
    Backend A: weight 80
    Backend B: weight 20
Use AWS Route53 or ALB to do weight routing

## 17. How does SSL work (Certbot, Let's Encrypt, AWS)? Explain the Flow.
SSL (Secure Sockets Layer), now replaced by TLS (Transport Layer Security), is a cryptographic protocol that ensures secure, encrypted communication between a client (browser) and a server.

    1. Client initiates HTTPS request to the server (e.g., a web app)
    2. Server responds with its SSL certificate, which contains:
        Public key
        Domain name
        Certificate Authority (CA) signature
    3. Client verifies the certificate using the CA's public key
    4. If valid, client and server use the public key to securely exchange a shared session key
    5. All further communication is encrypted with this session key

## 18. diff between IAM Users and Roles
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

## 19. Can you avoid the specific port traffic using SGs?

Security Groups (SGs) in AWS are stateful, allow-only firewalls. You can't explicitly deny traffic, but you can avoid traffic on specific ports by not allowing them at all.

## 20. What are the best password security practices used by your organisation?
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

## 21. What are the security parameters we must consider while we are creating an EC2 instance for production?
    Security parameters for EC2 in production include:
    Using security groups to control traffic
    Implementing IAM roles
    Enabling encryption for EBS volumes
    Regular security updates and patches
    Monitoring and logging

## 22. How can you protect the data in an AWS instance?
I protect data on an AWS instance by:
    Encrypting data at rest and in transit
    Restricting access using IAM, SGs, and VPC
    Using SSM over SSH
    Monitoring and auditing activity
    Taking regular, encrypted backups
This layered approach ensures both compliance and resilience against data breaches or loss.

## 24. Explain about the transit gateway and why do we use this?
    AWS Transit Gateway
    Acts as a central hub for routing between multiple VPCs and on-prem locations.

    ✅ Use Case:
    When connecting multiple VPCs and on-prem networks
    Supports both Direct Connect and VPN attachments
    Simplifies complex mesh network topologies

## 25. I have created an EC2 instance named A, and I want to create another instance B. It should create an instance without deleting instance A. What can I do during this?

You can create a new EC2 instance B by using a different resource name in Terraform or by creating a new instance in the AWS Console without affecting instance A.

## 26. How does the GSLB load balancer work?
GSLB (Global Server Load Balancer) is a DNS-based load balancing solution that distributes traffic across multiple geographically distributed data centers or cloud regions. Its main goal is to provide high availability, disaster recovery, and low-latency routing based on various factors.

🌍 How GSLB Works:
    Client makes a DNS request for a service (e.g., app.example.com)
    The GSLB system intercepts the DNS query and decides which data center or region should serve the request
    GSLB returns the IP address of the optimal site/server
    The client connects directly to that location

## 27. Can we connect two different VM that are in a different Vnet.
Yes, you can connect two VMs in different VNets, but they cannot communicate by default. You need to explicitly configure connectivity between the VNets 

## 28. Private Endpoints.
An endpoint is simply the entry point or destination through which a client communicates with a service, application, or resource over a network.
In cloud and networking contexts, an endpoint typically refers to a URL, IP address, or DNS name that is used to access a specific service.

## 29. Use of Route53
Amazon Route 53 is used for:

    Registering domains
    Routing internet traffic to AWS resources
    Smart DNS routing (weighted, latency, geo)
    Health checks & failover
    Managing private/internal DNS in VPCs
    It plays a critical role in building highly available, globally distributed systems on AWS.

## 30. Difference between S3 and EBS
Amazon S3 (Simple Storage Service) and Amazon EBS (Elastic Block Store) are both storage services offered by AWS, but they serve different purposes and are optimized for different access patterns and use cases.

Use Amazon S3 for object-based storage (files, media, backups) that is accessed via API or web.

Use Amazon EBS for block-level storage that behaves like a virtual disk and is mounted to EC2 for low-latency access.

## 31. What is the difference between vertical and horizontal scaling?
The key difference between vertical scaling and horizontal scaling lies in how resources are added to handle increased load or demand.
1. Vertical Scaling (Scale Up/Down)
Involves adding more resources (CPU, RAM, storage) to a single server or instance.
2. Horizontal Scaling (Scale Out/In)
Involves adding more instances/servers to distribute load.

## 32. Explain OSI models
The OSI Model breaks down network communication into 7 logical layers — helping engineers understand, design, and troubleshoot complex networks in a modular way.

## 33. How do you did cost optimization in AWS?
Cost optimization strategies include:

📏 Right-sizing EC2 instances
💳 Using Reserved Instances and Savings Plans
📈 Implementing auto-scaling
🎯 Using Spot Instances for non-critical workloads
💾 Optimizing storage (S3 lifecycle policies, EBS volume management)
🔍 Regular review of unused resources
📊 Using AWS Cost Explorer and Budgets for monitoring

## 34. How do you implement best security policies on AWS?
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

## 35. how many types of policy, IAM policy are there? IAM policies?
IAM Policies are JSON documents that define permissions in AWS — specifying what actions are allowed or denied, on which resources, and under what conditions.

They are attached to: IAM Users IAM Groups IAM Roles

Policy Type	           Attached To	                   Used For
Identity-based	IAM Users, Groups, Roles	Most common access control
Resource-based	AWS Resources (e.g., S3)	Sharing resources across accounts
Permissions boundaries	IAM Users, Roles	Limit maximum allowed permissions
Session policies	Temporary credentials	Scoped, time-limited access

## 36. what is the difference between the S3 bucket policies and acls?
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

## 37. Expalin about fargate?
AWS Fargate is a serverless compute engine for containers that works with Amazon ECS (Elastic Container Service) and Amazon EKS (Elastic Kubernetes Service).
With Fargate, you don't need to provision or manage EC2 instances — AWS automatically runs containers for you based on CPU and memory requirements.

## 38. what is cloud watch uses cases.
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
## 39. what is limitations of lamda
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

## 40. how lamda works containers
AWS Lambda supports running functions as container images. Instead of uploading code as ZIP files, you can package your Lambda as a Docker container image (up to 10 GB) and deploy it. This provides more flexibility in dependencies, tooling, and language support.

## 41. storage gateway in AWS
AWS Storage Gateway is a hybrid cloud storage service that enables on-premises applications to seamlessly use AWS cloud storage.
It acts as a bridge between on-prem infrastructure and AWS, allowing you to securely store data in the cloud while keeping frequently accessed data locally cached.

## 42. how you ensure the best possible security for high availability architectures for 3 tier applications.
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


## 43. I'm an admin but I don't have access to the S3 bucket ? 
"Even as an admin, S3 access can be blocked by bucket policies, encryption settings, and Block Public Access. I'd review the bucket policy, check for explicit denies or conditions, use the IAM Policy Simulator, and verify if any KMS key policies or VPC/IP conditions are restricting access."


## 44. What are the top 5 infra attacks, and how do you mitigate them?- notAnswered

## 45. EC2 instance is unreachable, and it's not a security group issue. What's your next step?
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

## 46. An S3 bucket was made public by mistake. How do you secure and audit it?
Steps to secure and audit a public S3 bucket:

🔒 Remove public access settings

Audit: Identify What Was Exposed and When Use CloudTrail to check: Who made the bucket public Any PutBucketPolicy, PutBucketAcl, or object-level ACL actions Use S3 Server Access Logs or CloudTrail Lake to check: What files were accessed and from where

📜 Review and update bucket policies
📝 Enable versioning
🔐 Enable server-side encryption
🔍 Use AWS Config to monitor bucket settings
🗑️ Review and remove any public ACLs


## 47. RDS migration with minimal downtime – how would you approach it?
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

## 48. How would you set up networking in vpc
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


## 49. How you will direct traffic to and from a instance in private subnet

To enable outbound traffic, I use a NAT Gateway in a public subnet and route traffic from the private subnet through it. For inbound access, I use a Bastion Host or SSM. For app-level access, I expose an ALB in the public subnet which forwards traffic to EC2s in the private subnet. This keeps the infrastructure secure while still functional.

## 50. how did you managed security for application level? 
I implemented application-level security using a layered approach — including authentication, authorization, encryption, input validation, secure headers, and dependency scanning — integrated within CI/CD to enforce security throughout the SDLC."

## 51. What types of nodes did you deploy in AWS?
"In AWS, I've deployed various types of nodes depending on the workload — including EC2 instances for traditional workloads, EKS worker nodes (managed and Fargate), RDS instances, and auto-scaling groups behind load balancers. I choose instance types and node types based on compute, memory, storage, and scalability requirements."

## 52. What is the difference between Interface Endpoint and Gateway Endpoint?
The key difference is that Interface Endpoints use private IPs and Elastic Network Interfaces (ENIs) to connect to AWS services over the VPC network, while Gateway Endpoints update your route tables to route traffic to specific services like S3 and DynamoDB without using NAT or public IPs."

## 53. How did you set up ECS using EC2 instances?
To set up ECS using EC2:

🏗️ Create an ECS cluster
💻 Launch EC2 instances into the cluster
📝 Define task definitions and services
🚀 Deploy applications using ECS

## 54. Can't we configure Route 53?
Common reasons for not being able to configure Route 53 include:

🔒 Insufficient IAM permissions
⚙️ Incorrect DNS settings
🌐 Domain not registered with Route 53

## 55. If we want to configure third-party domains like GoDaddy in Route 53, how do we do that?
To configure a third-party domain with Route 53:

Create a Public Hosted Zone in Route 53 ✅ This will generate 4 Name Server (NS) records automatically.
Copy Route 53 Name Server Records
Update the domain's nameservers in godaddy
Create DNS Records in Route 53

"To configure a GoDaddy (or third-party) domain with Route 53, I first create a public hosted zone in Route 53, copy the AWS name servers, and update them in GoDaddy's DNS settings. Then I create the required A, CNAME, or Alias records in Route 53 to point traffic to my AWS resources like ALB, EC2, or S3 static websites."


## 56. What is the difference between AWS Config and AWS CloudTrail?
AWS CloudTrail logs every API call made in your account — including who made it, when, and from where. AWS Config continuously records the configuration state of resources and shows how they've changed over time. I use CloudTrail for auditing and security investigations, and AWS Config for compliance monitoring, drift detection, and historical resource tracking.

🔍 AWS Config Example Use Cases
    Track if an S3 bucket had public access enabled
    Detect when an EC2 instance type was changed
    Enforce compliance via AWS Config Rules (e.g., "EBS volumes must be encrypted")

🔍 AWS CloudTrail Example Use Cases
    Audit: "Who deleted an EC2 instance?"
    Investigate: "Which user accessed Secrets Manager at 2AM?"
    Monitor for unauthorized API calls



## 57. What are the node groups you used in AWS EKS?
Node groups in EKS are managed groups of EC2 instances that run Kubernetes nodes. They simplify the management of nodes in an EKS cluster.

## 58. What are the types of node groups in AWS EKS?
n AWS EKS, there are two main types of node groups: Managed Node Groups and Self-Managed Node Groups. Additionally, AWS offers Fargate Profiles for serverless Kubernetes pods. 

## 59. If a user wants to access the S3 bucket, what are the processes?

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

## 60. How does VPC Peering work?
VPC peering is a networking connection between two VPCs that enables you to route traffic between them using private IP addresses.


## 61. How does Transit Gateway work and how did you configure it?
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

## 62. If we connect VPCs to the Transit Gateway, what will you update in the VPC Route Table?
When you connect VPCs to a Transit Gateway (TGW), you must update the VPC route tables to route traffic to the Transit Gateway for any destination CIDRs (usually other VPCs or on-prem networks).

"When I attach a VPC to a Transit Gateway, I update the route table in each VPC to point the destination CIDR of peer VPCs to the TGW ID. I also ensure the TGW route table maps the CIDRs to the correct VPC attachments, and that security groups are configured to allow traffic between them."

## 63. For all VPCs, will you configure the Transit Gateway attachment with CIDR range?
No, we don't manually configure CIDR ranges during Transit Gateway attachment. The VPC's CIDR is automatically associated with the attachment. We control routing via the Transit Gateway route table, which maps destination CIDRs to the appropriate VPC attachment. This ensures that each VPC can route traffic to the correct peer via TGW.


## 65. What is WAF (Web Application Firewall) and AAF (Application Access Firewall)?
WAF is used to filter and block malicious web traffic at Layer 7 to protect apps from attacks like SQL injection and XSS. AAF, while not an AWS-native service, usually refers to access control at the application level — such as user identity checks, RBAC, or context-based access — often implemented with identity-aware proxies or API gateways

## 66. What is VPC Flow Logs and how will you track the IPs hitting the VPC?
VPC Flow Logs capture information about IP traffic going to and from network interfaces in your VPC. You can analyze these logs using CloudWatch Logs or Athena

## 67. How to filter a particular IP from AWS CloudWatch Log Group?
To filter logs for a specific IP in AWS CloudWatch, I use Logs Insights with a regex filter on @message

## 68. If you are storing logs in S3 Bucket, how will you track that particular IP?
To track IP addresses accessing logs stored in an S3 bucket, you need to enable access logging and monitoring tools

Enable S3 Server Access Logging 🛠 Steps: Go to your S3 bucket (e.g., log-storage-bucket) Go to Properties → Server access logging Enable logging and choose a target bucket (e.g., log-access-audit-bucket) Important: The target bucket must not be the same as the source bucket.

CloudTrail (for API-level tracking) Enable AWS CloudTrail to record S3 data events Steps: Go to CloudTrail > Trails > Create trail Enable Data events → S3 → log-storage-bucket Logs go to another S3 bucket or CloudWatch for analysis

## 69. How do you take the backup of AWS Services?
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

## 70. I need to send a logs from EC2 to S3, create an automation part where we need to take the log file, check the CPU metrics and send an alarm through cloud watch to user's.
    ✅ Step 1: Create an IAM Role for EC2 with Required Permissions Policy Permissions: S3: PutObject CloudWatch: PutMetricData, GetMetricStatistics SNS: Publish Logs: Optional, if using CloudWatch Logs

    ✅ Step 2: Install and Configure CloudWatch Agent for CPU Monitoring

    ✅ Step 3: Create CloudWatch Alarm for CPU Usage

    ✅ Step 4: Create SNS Topic and Subscribe Email

    ✅ Step 5: Automate Log File Upload via Cron

## 71. If thre is an instance we have security group and web application firewall enabled, DDOS attack enabled will it protect from Bot attack.
Having Security Groups, WAF, and AWS Shield provides layered protection, but they don't fully protect against bot attacks on their own. For that, I use AWS WAF's Bot Control managed rule group or integrate third-party tools like Cloudflare Bot Management. I also implement CAPTCHA, request throttling, and JavaScript challenges to stop sophisticated bots.

## 72. What is DNS Resolution. Suppose I have new system with no cache what happens in the background.Step by step process.
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


## 73. If you're migrating a monolithic application from on-prem to Cloud and the system has its local file system, which file system you will use in AWS.
For a monolithic application relying on a local file system, I would use Amazon EFS (Elastic File System) if I need a scalable, shared file system with NFS compatibility across multiple instances. If only one instance needs access and performance is critical, I might use Amazon EBS instead."

## 74. How will you store all the configurations related to your monolithic app in Cloud,
I store application configurations in centralized and secure services like AWS Systems Manager Parameter Store or AWS Secrets Manager. This ensures externalized, versioned, and encrypted configuration management across environments — separate from the application code."

## 75.How would you secure an EC2 instance against unauthorized SSH access?

- Restrict SSH at the Network Level 🔸 Security Group (SG) Only allow port 22 from trusted IPs
- Disable Root Login Avoid direct root access. Create a non-root user (ec2-user, ubuntu) with sudo access.
- Use IAM + EC2 Instance Connect or AWS Systems Manager

## 76. You need to connect your DB running in private subnet, not using NAT gateway or NAT instance or bastion host, what are the other options,
"If I can't use a NAT Gateway, NAT instance, or bastion host, I'll use AWS Systems Manager Session Manager to connect to a managed EC2 instance inside the VPC that can access the private DB. I can also use VPC Interface Endpoints, AWS Client VPN, or SSM port forwarding to securely connect to the database without exposing it to the public or depending on traditional network hops."

## 77. what is tcp and udp
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


## 78. what is ipv4 and ipv6
IPv4 and IPv6 are versions of the Internet Protocol used to identify devices on a network

Pv4 is the traditional addressing system using 32-bit addresses and supports about 4.3 billion devices. Due to address exhaustion, IPv6 was introduced with 128-bit addresses, providing a vastly larger pool and built-in features like IPsec, efficient routing, and no NAT. While IPv4 is still dominant, IPv6 adoption is growing.

## 79. what is pid
PID stands for Process ID, and it is a unique number assigned by the operating system to every running process. It helps the OS and users identify, monitor, and manage individual processes."

## 80. what metrics is used to monitor ec2 instance cpu, memory in aws
In AWS, we use Amazon CloudWatch to monitor EC2 metrics. By default, CloudWatch provides CPU-related metrics, and memory metrics require custom configuration. These metrics help track performance, utilization, and health of EC2 instances."

## 81. Sending log files from EC2 to S3 – what are the steps?
Steps to send logs from EC2 to S3:

    🔑 Create an IAM role with S3 write permissions
    🔗 Attach the role to the EC2 instance
    📤 Use AWS CLI or SDK to upload logs to S3
    ⏰ Set up a cron job or use CloudWatch Logs agent to automate the process
## 82. Difference between subnet and NACL?
    Subnets are segments of a VPC's IP address range
    NACLs (Network Access Control Lists) are stateless firewalls that control traffic at the subnet level

## 83. Difference between NAT Gateway and Internet Gateway.
    Internet Gateway allows resources in a VPC to connect to the internet
    NAT Gateway allows resources in a private subnet to connect to the internet while maintaining their private IP
## 84. Difference between SGs and NACLs.
    Security Groups are stateful and operate at the instance level
    NACLs are stateless and operate at the subnet level
## 85. Where do you use firewalls, SGs, and NACLs?
    Firewalls: On-premises or in the cloud for network security
    Security Groups: For instance-level security in AWS
    NACLs: For subnet-level security in AWS
## 86. How do you connect private subnet to the internet?
    To connect a private subnet to the internet, you need:

    1.🌐 A NAT Gateway in a public subnet
    2. 🗺️ Route tables configured to route traffic from the private subnet to the NAT Gateway

    "To connect a private subnet to the internet, I create a NAT Gateway in a public subnet and update the private subnet's route table to forward internet-bound traffic through the NAT. This allows outbound internet access while keeping the subnet private. For more control, a NAT instance can also be used, but NAT Gateway is preferred for scalability and maintenance."

## 87. Design HA backend using AWS services.
For a highly available backend, use:

🌐 Multiple AZs
📈 Auto-scaling groups
⚖️ Load balancers
📊 RDS with Multi-AZ deployment
🚀 ElastiCache for caching


## How are you connecting client's environment from your AWS environment.
To connect our AWS environment to a client's environment, we typically use secure networking options depending on the client's setup and requirements. The most common approaches are Site-to-Site VPN, AWS Direct Connect, or VPC Peering if the client is also on AWS."

## how to manages certificate in aws. If the certificate expires how are you managing it and what's the action you are taking over here.
In AWS, I use AWS Certificate Manager (ACM) to provision, manage, and renew TLS/SSL certificates for services like ELB, CloudFront, and API Gateway. ACM handles automatic renewal for public certificates, which simplifies lifecycle management."

*** ACM (Public Certificates)***
“For ELB, CloudFront, or API Gateway, I request the certificate through ACM, validate it via DNS or email, and attach it to the resource.
ACM then auto-renews the certificate, usually 30 days before expiry, as long as domain validation is still in place.”

✅ No manual renewal
✅ Auto-integrates with AWS services


## How do you perform load testing for your application?
I perform load testing using tools like Apache JMeter, Locust, or k6 to simulate real-world traffic patterns and evaluate application performance under stress. The goal is to understand system behavior under peak load, identify bottlenecks, and ensure the app meets SLAs."


## What is the primary difference between ECS and EKS?
The primary difference between ECS and EKS is that ECS is a fully managed container orchestration service built by AWS, while EKS is AWS’s managed version of Kubernetes."

ECS (Elastic Container Service) is proprietary to AWS, simpler to manage, and tightly integrated with AWS services.

EKS (Elastic Kubernetes Service) is based on open-source Kubernetes, offering portability and more flexibility, especially in hybrid or multi-cloud environments.


## Cloud watch & how do you create a custom metric in AWS CloudWatch.
AWS CloudWatch is a monitoring and observability service that collects logs, metrics, and events from AWS resources and applications. I use it to monitor system performance, set up alarms, and trigger automated actions like scaling or notifications."

What CloudWatch Does
Monitors CPU, memory, disk, network (EC2, RDS, Lambda, etc.)
Collects and queries logs from apps (via CloudWatch Logs)
Supports custom metrics for app-specific data (e.g., user count, queue length)
Sends alerts via alarms + SNS, triggers Lambda or Auto Scaling

To create a custom metric in AWS CloudWatch, I use the put-metric-data API, either through the AWS CLI, SDK (like Python’s Boto3), or CloudWatch Agent. This allows me to push any application-specific data — such as active users, queue length, or error counts — into CloudWatch for monitoring and alerting.

```bash
aws cloudwatch put-metric-data \
  --namespace "MyApp" \
  --metric-name "ActiveUsers" \
  --value 150 \
  --dimensions "Environment=Prod" \
  --unit Count
```
This creates a metric called ActiveUsers under the MyApp namespace, with a dimension Environment=Prod. Once published, I can view it in the CloudWatch Console, graph it on a dashboard, or create an alarm to notify or trigger actions.”

## how do you access s3 in a Account from Account B
There are two options to access

Option1:
Use Bucket Policy in Account A
This is the simplest and most common way.

📍 In Account A, add this policy to the S3 bucket:
```bash
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowAccountBAccess",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::<ACCOUNT_B_ID>:root"
      },
      "Action": [
        "s3:GetObject",
        "s3:PutObject",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::my-data-bucket",
        "arn:aws:s3:::my-data-bucket/*"
      ]
    }
  ]
}
```

✅ Option 2: Use IAM Role (Recommended for Fine-Grained Control)
This is more secure and flexible — especially in production and enterprise environments.

🔧 In Account A: Create a Role that Account B Can Assume
1. Create an IAM Role in Account A
    Name: CrossAccountS3AccessRole
    Trusted entity:
```bash
{
  "Effect": "Allow",
  "Principal": {
    "AWS": "arn:aws:iam::<ACCOUNT_B_ID>:root"
  },
  "Action": "sts:AssumeRole"
}
```
2. Attach this policy to the role:
```bash
{
  "Effect": "Allow",
  "Action": [
    "s3:ListBucket",
    "s3:GetObject",
    "s3:PutObject"
  ],
  "Resource": [
    "arn:aws:s3:::my-data-bucket",
    "arn:aws:s3:::my-data-bucket/*"
  ]
}
```
🔧 In Account B: Assume the Role

```bash
aws sts assume-role \
  --role-arn arn:aws:iam::<ACCOUNT_A_ID>:role/CrossAccountS3AccessRole \
  --role-session-name s3access
```
This returns temporary credentials (AccessKeyId, SecretAccessKey, SessionToken) that can be used to access the bucket.


## How do you login to the ec2 instance if you've lost the .pem key?

Step-by-Step Recovery Approach:
1. Stop the instance (do not terminate it — that would delete data).
2. Detach its root EBS volume (usually /dev/xvda) from the original instance.
3. Attach the volume to a temporary EC2 instance (that I have access to).
4. SSH into the temp instance, then mount the attached volume:
```bash
sudo mkdir /mnt/recovery
sudo mount /dev/xvdf1 /mnt/recovery
```
5. Replace the authorized_keys file:
```bash
sudo nano /mnt/recovery/home/ec2-user/.ssh/authorized_keys
```
Paste in the public key from a new key pair I’ve created via AWS.
Or copy from the temp instance if that user already has access.

6. Unmount the volume, detach it from the temp instance, and reattach it to the original instance as the root volume.

7. Start the original EC2 instance, and now I can SSH using the new .pem file.

Optional: Use EC2 Instance Connect (if Amazon Linux 2 or Ubuntu)
"If EC2 Instance Connect is enabled and supported, I may also use it temporarily via the AWS Console to connect and add new SSH keys without needing the original .pem file."


## how do you implement to block an particular domain in application gateway 
In ALB listernes rules, I will create rule

```bash
IF Host header == blocked.example.com
THEN Return fixed-response 403 (text/plain, "Access Denied")
```
✅ This tells the ALB:
If an incoming request has a Host header of blocked.example.com
Immediately return a 403 Forbidden response

But place this rule before your default forwarding rule, so it matches and blocks the request before it reaches your application.

## which metrics is being used when cpu or memory  goes beyond 75% in vm
CPUUtilization and MemoryUtilization

## where do you store the application gateway (TLS/SSL certificate) ?
So in AWS, I always use ACM to store and manage TLS certificates. It’s tightly integrated with ALB, supports automatic renewals for public certs, and simplifies certificate lifecycle management in a secure and auditable way.


## What are autoscaling policies and their uses?
Auto Scaling policies in AWS define how and when to automatically scale your EC2 instances (or other resources) up or down based on metrics or scheduled events. They help optimize cost, maintain performance, and respond dynamically to load changes

1. Target Tracking
"Best for maintaining consistent performance automatically. For example, keeping average CPU at 50% across instances."

🔹 2. Step Scaling
"Useful for more control when load spikes vary — e.g., scale faster at 90% CPU than at 70%."

🔹 3. Scheduled Scaling
"Ideal for predictable workloads — like business hours scaling or nightly scale-down."

🔹 4. Predictive Scaling (Optional, advanced)
"Uses machine learning to forecast demand and scale proactively."


## What is a target probe in autoscaling?
In the context of auto scaling, a 'target probe' typically refers to the mechanism (often a health check or readiness check) that determines whether a target instance or service is healthy and can receive traffic. It's not a formal AWS term, but it's closely related to how auto scaling groups, load balancers, and Kubernetes determine target health before scaling or routing traffic."

## How do you ensure the least privilege access to the IAM users
To ensure least privilege access for IAM users in AWS, I follow a disciplined process: I grant only the permissions necessary for the user to perform their job, nothing more. I enforce this using scoped IAM policies, role-based access control, resource-level restrictions, and continuous auditing."

## Is it possible to take AMI details from Snapshots
So while you can’t extract full AMI info from a snapshot directly, you can use the snapshot to recreate the AMI. This is a common recovery method when an AMI has been deleted but the snapshot still exists.

## How to check LB health details (monitoring) through AWS service
To monitor the health of a Load Balancer in AWS, I primarily use CloudWatch for metrics, target health checks in the Load Balancer console, and VPC Flow Logs or AWS X-Ray for deep traffic analysis
1. CloudWatch Metrics
Go to CloudWatch > Metrics > LoadBalancer.

## Development team changed the AMI configuration to ASG launch template, how will you make sure that the new version is deployed properly.

First I go to ec2 and check the launch template to confirm the latest version
Also check ASG using the correct version
I will trigger the deployment of new AMI by manually terminating one ec2 instance

## Post connecting diff VPCs through TGW, I want to black the traffic of A to B and B to C, how to perform it

1. Use Multiple Transit Gateway Route Tables
Create separate route tables for each VPC and control which VPCs they can reach.

2. Optional: Use Security Groups or NACLs
Even if routes exist, security groups or network ACLs can still block traffic.
Remove inbound rules from B that allow traffic from A’s CIDR
Block at subnet level using NACL deny rules


## EC2 in a private subnet should receive inbound traffic, how to enable it ? → NOT NAT Gateway
By design, a private subnet has no direct access to or from the internet. However, if I want an EC2 instance in a private subnet to receive inbound traffic — without using a NAT Gateway — I can enable it by setting up routing through a Load Balancer, VPN, or VPC Peering, depending on the source of the traffic
1. Application/Network Load Balancer (Recommended)
Use a Public ALB or NLB in a public subnet to forward traffic to the EC2 instance in the private subnet.
Steps:
    Place ALB/NLB in a public subnet (with Internet Gateway)
    Register the private EC2 instance as a target in a Target Group
    Ensure:
        Security group of ALB allows incoming traffic (e.g., port 80 or 443)
        EC2 security group allows traffic from the ALB's security group

    ✅ Result: Internet → Load Balancer → Private EC2

## Is it possible to add multiple LBs to my sub web pages
You can’t assign multiple Load Balancers directly to specific subpages (like /app1, /app2), but you can use one Application Load Balancer (ALB) with multiple routing rules to forward requests to different target groups or services based on the URL path.

## userdata in EC2
User Data in EC2 is a script or set of commands that you can pass to an instance at launch time to perform automated configuration tasks. It runs once when the instance boots for the first time, allowing you to automate provisioning — like installing software, applying patches, or pulling application code.

## How to segregate the critical details from VPC flow logs
To segregate critical information from VPC Flow Logs, I use log filtering, tagging, and analysis tools like Amazon Athena, CloudWatch Logs Insights
1. Enable VPC Flow Logs
Destination: CloudWatch Logs or S3

Choose granular level (VPC, subnet, or ENI)

2. Use Filters in CloudWatch Logs Insights


I analyze VPC Flow Logs using CloudWatch Logs Insights or Athena, applying filters to isolate critical patterns like denied access, abnormal port usage, or unknown IPs.

## Diff b/w Fargate vs EKS worker nodes
The primary difference between AWS Fargate and EKS worker nodes is that Fargate is a serverless compute engine where you don't manage any EC2 instances, while EKS worker nodes are self-managed EC2 instances that run your Kubernetes workloads. Fargate provides simplicity and scaling out-of-the-box, whereas worker nodes offer more control, flexibility, and customization

## ASG is active and the load is heavy, due to that two EC2 instances are launched, but EC2 provision is taking 2 to 3 mins of time because of the ASG terminating the instance, how to avoid it.
If Auto Scaling Group (ASG) launches new EC2 instances but terminates them too quickly before they’re fully provisioned, it's usually because of health checks or readiness issues. To avoid this, I tune the instance lifecycle and health check configurations to give new instances enough time to become healthy before being evaluated for termination.

1. Increase HealthCheckGracePeriod
ASG uses health checks (EC2 or ELB). If an instance doesn't pass within the grace period, it's marked unhealthy and terminated.

```bash
aws autoscaling update-auto-scaling-group \
  --auto-scaling-group-name my-asg \
  --health-check-grace-period 300
```


## I've huge traffic coming between 5 PM to 8 PM daily, how to configure this in ASG
To handle predictable traffic spikes in AWS Auto Scaling Groups (ASG), like daily heavy traffic from 5 PM to 8 PM, I configure a Scheduled Scaling Policy. This lets me proactively increase the instance count before the peak and reduce it after the period ends — ensuring better performance and cost control.


## Can we create two diff CICR blocks 172. && 192. series in same VPC
Yes, AWS allows you to associate multiple CIDR blocks 

## Customizing WAF
To customize AWS WAF, I define my own Web ACL rules using conditions like IP addresses, HTTP headers, rate limits, geo-locations, and custom string patterns. This allows me to tailor the WAF to protect against specific threats such as bot traffic, SQL injection, or abuse from certain IP ranges.
I create a ACLS and attach to ALB, CLoudFront


## Cloud Front configuration
To configure AWS CloudFront, I set up a distribution that defines how content is delivered to users through AWS edge locations. The configuration includes the origin (like S3, ALB, or EC2), caching rules, SSL settings, behaviors, and security controls like WAF or signed URLs. This allows me to deliver static and dynamic content globally with low latency and high performance.

1. Choose Your Origin
The origin is where CloudFront fetches content from.
✅ S3 bucket (static website or private content)
✅ ALB / EC2 (for dynamic websites)
✅ API Gateway or any HTTP endpoint

2. Configure Cache Behavior
Path Pattern: e.g., /images/*, /api/*
Viewer Protocol Policy: Redirect HTTP to HTTPS
Allowed Methods: GET, POST, etc.
Forwarded Values: Cookies, query strings, headers
TTL Settings: Default, min, and max cache times

3. Enable HTTPS with SSL/TLS
Use an ACM certificate (in us-east-1 for CloudFront)

## AWS Image builder
AWS Image Builder is a fully managed service that automates the creation, maintenance, testing, and deployment of secure and up-to-date Amazon Machine Images (AMIs) and container images. It helps standardize image creation across environments, reduce manual effort, and improve compliance and security posture

## If you're storing all your VPC Flow logs in S3 bucket, how to see that
If VPC Flow Logs are being stored in an S3 bucket, I typically use tools like Amazon Athena, AWS Glue, or S3 Select to query and view the logs in a structured way. This enables efficient filtering, search, and analysis of network traffic data directly from S3.

2. Use Amazon S3 Select (Quick Lookups)
For simple filtering inside S3 console without full Athena setup.
Go to the S3 bucket
Select a log file
Click “Select from” → Choose CSV or text format
Use SQL-like syntax to filter records

3. Download and View Logs Manually
If data volume is small or for debugging.
Go to S3 → Locate the file → Download .log or .gz
Open locally with a text editor or tools like grep, awk, Excel, etc.

## How many objects can a S3 bucket can store?
An Amazon S3 bucket can store an unlimited number of objects. There is no limit on the number of objects per bucket. The only limitations are on object size and request rates
Bucket limit	100 buckets per account (soft limit, can request more)
Object size	Up to 5 TB (single object), multipart upload recommended for >100 MB

## Static web hosting in S3 how it works?
1. Create an S3 Bucket
Bucket name must match the website domain (e.g., www.example.com) if you plan to use custom domains.

🔹 2. Upload Website Files
Upload your index.html, style.css, app.js, etc., to the bucket.

3. Enable Static Website Hosting
Go to the S3 bucket → Properties
Enable "Static website hosting"
Set:
Index document: index.html
Error document: error.html (optional)

You get an S3 website endpoint `http://<bucket-name>.s3-website-<region>.amazonaws.com`

4. Make Files Public (Optional)
S3 buckets are private by default. To allow public access:

S3 static website hosting works by enabling a special mode on an S3 bucket that allows it to serve content over HTTP. It's great for low-cost, serverless hosting of static sites, and can be enhanced with CloudFront and Route 53 for custom domains, HTTPS, and CDN-level performance."


## Users are uploading data in S3... The S3 owner wanted to know who were all accessing the bucket.  Note - not to use S3 event notification
To track who accessed an S3 bucket without using S3 event notifications, I enable S3 server access logging and/or AWS CloudTrail data events. These services allow the bucket owner to monitor and audit access to objects, including user identity, IP, time, and action taken."

## If developer sets private subnet to public, what should you do?
If a private subnet is misconfigured as public, I would immediately remove the IGW route, audit any impacted resources, and enforce controls through tagging standards, IAM permissions, and AWS Config rules to prevent future occurrences.

## .you have an application gateway, services which is in backend is good, you are getting 404 error ,how do you troubleshoot further
A 404 from Application Gateway often points to a path mismatch or missing route in the backend app. I troubleshoot by checking listener and routing rules, validating backend target group associations. I test directly against the backend to isolate whether the issue is in App Gateway or the application itself.

## what is A record and what it will do in DNS
An A record (Address Record) in DNS maps a domain name to an IPv4 address. It’s one of the most fundamental DNS record types and is used to tell clients — like web browsers — where to find your server by resolving a domain name into an IP address.

## what is the value of A record
The value of an A record in DNS is an IPv4 address. It tells DNS resolvers what IP address to return when someone looks up a domain name."

## what is CName record in DNS
A CNAME (Canonical Name) record is a type of DNS record that maps an alias domain name to another domain name — rather than directly to an IP address. It allows you to point multiple domain names to a single canonical domain.

## what is MXRecord in DNS
An MX (Mail Exchange) record is a DNS record that specifies the mail server responsible for receiving email on behalf of a domain. It directs email traffic to the correct email server using the domain name, not an IP address."

## application gateway TLS certificate is expired, what steps you will follow to renew it
✅ If using ACM Public Certificate:
ACM auto-renews it if DNS or email validation is still in place

You don’t need to manually renew, but you must replace it on the ALB if the certificate ARN changes



## aws lambda where to use use case 



## 1. You application is not able to access s3 bucket? how to trobleshoot

**1. Check IAM Permissions (Most Common Issue)**

Make sure the IAM **role or user** the app is using has the correct permissions

**2. Check S3 Bucket Policy (If Used)**

If the bucket has a **bucket policy**, make sure it’s not **explicitly denying** access or missing allow rules.

## 2. If you enter www.example, and enter username and password what will happen in context of AWS?

**1. DNS Resolution via Route 53**

- The browser resolves `www.example.com` to an IP address.
- If using **Route 53**, it routes to:
    - An **ALB** or **CloudFront** distribution

**2. Web Page Is Served**

- Static frontend (HTML, CSS, JS) is served from:
    - **Amazon S3** (with CloudFront)
    - Or **EC2 / container backend** behind a Load Balancer

**3. User Enters Credentials and Clicks "Login"**

The frontend sends an HTTP(S) **POST** request to your authentication API:

**4. Authentication Logic Executes**

**5. Token or Session Is Stored in Browser**

**6. User Gains Access to Protected Resources**

With a valid token/session:

- Frontend makes requests to protected endpoints (e.g., `/dashboard`)

## 3. There are two applications in private subnet in vpc, they are not communicating each other how do you check the issue?

- All subnets within the same **VPC** are connected using the **VPC's internal routing**.
- So **network-wise**, any instance in the same VPC can reach any other instance **via private IP**.

I will trobleshoot 

1. **First test connectivity**
    
    From **App A**, try reaching **App B**: 
    
    ping <ip-add-App B>
    
    if ping fails then it might be network level issue
    
2. **Check Security Groups**

> Security Groups must explicitly allow inbound and outbound traffic between the two instances.
> 

  Even if routing allows it, **security groups must explicitly allow traffic** between instances.

1. **Check Network ACLs (NACLs)**

> NACLs are stateless — both inbound and outbound rules must allow traffic.
> 

**4. Check Route Tables**

- Go to **VPC → Route Tables**
- Ensure both subnets:
    - Are associated with a route table
    - Have routes to each other (usually via `local`)
    

## 4. Sometime user accessing the application, sometime they can’t how can you troubleshoot?

if sometime is accessible sometime not, its likely a intermittent issue.

Firstly I would be identify the scope and pattern of the issue.

1. **check Load Balancer(if using)**
    
    I would check the load balancing target groups are healthy because unhealty will give bas response
    
    aws elbv2 describe-target-health --target-group-arn <your-tg-arn>
    
2. **Check Application Logs and Web Server Logs**
    
    I will check the logs of the application and particurarly looks for issues:
    
    - Database timeouts
    - Connection resets
    - 5xx responses
    - Crashes or memory issues

mostly check the logs in cloudWatch to understand the root cause of the problem. 

## 5. If you're facing performance issues on a server, how do you troubleshoot?

Your application/server is **slow** — users report **high latency**, or requests are **timing out**.

1. **Check CPU, Memory, and Load Average**

top         # Live view of CPU, memory, and running processes
uptime      # Check load average (last 1, 5, 15 minutes)
free -m     # Memory usage in MB

1.  **Find Top Offenders (High CPU / Memory)**
    
    ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%cpu | head 
    
2. **Check Disk Space and I/O**
    
    df -h                       # Disk usage
    du -sh /var/log/*           # Big log files?
    iotop                      # Disk I/O in real time (if installed)
    
- Check **application logs** for latency
- Use **APM tools** (Datadog, CloudWatch, X-Ray) for tracing slow parts
- Run **load testing** to observe where degradation starts

Depends on the bottleneck, I would be resolve the issue.

## 6. You have deployed a web app and it was working fine but application went down, and all networking are fine and related ports are open, how to troubleshoot

That means the **issue is likely inside the instance or application layer** — not the infra.

First I will ssh into the app and check if the applicatin still running using

ps aux | grep node or java

Secondly, I will restart the application and check the app logs for crash or error.

and 

Look for:

- Stack traces
- "Port in use"
- "Out of memory"
- Database connection errors

Also confirm the app is listening on correct port

sudo netstat -tuln | grep <port>

Check memory, RAM, disk usage, → If memory or disk is full, app may crash on start

Finally I do check for any recent changes made.

Depends on the issue I will move forward and solve issue

## 7. how will you troubleshoot if a system goes down in Linux - tell the commands

uptime

top       # Live view of CPU/memory
htop      # (if installed, better view)
free -m   # Memory status

df -h                       # Disk usage
du -sh /var/log/*           # Big log files?

journalctl -xe
journalctl --since "15 minutes ago"

systemctl --failed

journalctl -u <app-name>

## 8. Suppose you are having an e-commerce application  and slowness is there how will you troubleshoot

“I'd troubleshoot by first isolating whether the slowness is in the frontend, backend, or DB. On the backend, I'd check system resource usage, app logs, and slow queries. I'd inspect load balancer health and scaling activity, and use tracing tools like X-Ray or Datadog to identify bottlenecks down to the route or service call level.”

## 9. A sudden surge in traffic causes a web application to become unresponsive what will be the steps you will take to mitigate

### 10. EC2 instance is unreachable, and it’s not a security group issue. What’s your next step?

first check ec2 instance is running, and 2/3 checks passed, and Might be Nacls issue

## 11. How have you resolved high-performance issues or critical incidents?

We had a critical incident during a product launch. Our backend API hosted on EC2 behind an ALB started responding slowly, and users reported timeouts. CPU was spiking, and latency had tripled.

> I started with a layered approach:
> 
1. **Checked CloudWatch metrics** → saw **high CPU and memory** usage on backend EC2s.
2. **Verified ALB target health** — 2/4 targets were marked unhealthy.
3. SSH’ed into one EC2 and found the **Node.js app had memory leaks** and was not closing DB connections properly.
4. **Restarted app services** to temporarily mitigate.
5. **Scaled out** Auto Scaling Group to 2x instances for immediate relief.
6. Worked with devs to **patch the app** — we added connection pooling and fixed in-memory cache overflow.
7. **Deployed via blue/green**, monitored response times.

## 12. You ALB, some request are failing with gateway timeout ? how to trobleshoot?

If some requests through an **AWS Application Load Balancer (ALB)** are failing with **504 Gateway Timeout**, that means the ALB **couldn't get a response from the target (EC2, container, Lambda, etc.) within the idle timeout period**.

1. **Understand 504 Gateway Timeout**

- The target **did not respond in time**.
- ALB default idle timeout = **60 seconds** (can be adjusted).

2. **Check Target Health**

- Go to **EC2 > Target Groups**:
    - Are targets showing as **healthy**?
    - If unhealthy:
        - Check health check path, protocol, port.
        - Check if target is accepting connections.

3. **Increase ALB Timeout (if needed)**

- If your backend needs more time:
    - Update **ALB idle timeout** (in Load Balancer settings).
    - Update **server/app timeouts** to be **greater than or equal** to ALB timeout.

4. **App-Level Issues**

- Slow database queries, external API calls, or app bottlenecks can delay response.
    - Review application logs.
    - Add profiling to trace long-running parts.
    - Enable ALB access logs for request timing and delays.

5. **Use CloudWatch Metrics**

- ALB metrics:
    - `TargetResponseTime`
    - `HTTPCode_Target_5XX_Count`
    - `HealthyHostCount`
- App metrics:
    - CPU, memory, disk, etc. (check if the instance is overloaded)

### 13. What are the all the issues you had faced in your project, please explain

## how do you access s3 in a Account from Account B
There are two options to access

Option1:
Use Bucket Policy in Account A
This is the simplest and most common way.

📍 In Account A, add this policy to the S3 bucket:
```bash
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowAccountBAccess",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::<ACCOUNT_B_ID>:root"
      },
      "Action": [
        "s3:GetObject",
        "s3:PutObject",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::my-data-bucket",
        "arn:aws:s3:::my-data-bucket/*"
      ]
    }
  ]
}
```

✅ Option 2: Use IAM Role (Recommended for Fine-Grained Control)
This is more secure and flexible — especially in production and enterprise environments.

🔧 In Account A: Create a Role that Account B Can Assume
1. Create an IAM Role in Account A
    Name: CrossAccountS3AccessRole
    Trusted entity:
```bash
{
  "Effect": "Allow",
  "Principal": {
    "AWS": "arn:aws:iam::<ACCOUNT_B_ID>:root"
  },
  "Action": "sts:AssumeRole"
}
```
2. Attach this policy to the role:
```bash
{
  "Effect": "Allow",
  "Action": [
    "s3:ListBucket",
    "s3:GetObject",
    "s3:PutObject"
  ],
  "Resource": [
    "arn:aws:s3:::my-data-bucket",
    "arn:aws:s3:::my-data-bucket/*"
  ]
}
```
🔧 In Account B: Assume the Role

```bash
aws sts assume-role \
  --role-arn arn:aws:iam::<ACCOUNT_A_ID>:role/CrossAccountS3AccessRole \
  --role-session-name s3access
```
This returns temporary credentials (AccessKeyId, SecretAccessKey, SessionToken) that can be used to access the bucket.
