### In CloudWatch, what is the use of log groups and Cloud trails?
Log groups are used to logically organize and manage logs from AWS services and custom apps.

CloudTrail, on the other hand, captures API calls and account activity across AWS. It's used for auditing and security monitoring.

### What are Network ACLs and Security Groups, and how do they differ?
 IIn AWS, Security Groups and Network ACLs (NACLs) are both used to control network traffic, but they serve different purposes and operate at different levels.

 `Security Group` act as firewalls at the instance level - for EC2, RDS. They are stateful, meanining if you allow inbound traffic, the outbound traffic is automatically allowed, and vice versa. you can only define allow rules, not deny.

` NACLs`, on the other hand, operate at the subnet level. Theya are stateless, so you must explicitly define both inbound and outbound rules for bidirectional communication. NACLs support both allow and deny rules, making them useful for subnet-wide IP blocking or stricker control.

### How do you configure AWS RDS, and what factors do you consider (size, requirements, etc.)?
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

### How much data is stored in your RDS MySQL?
In production, our largest database holds approximately 150 GB of transactional data.

### How many masters and slaves are in RDS?
In Amazon RDS, the standard setup supports one master (primary) instance and multiple read replicas (commonly referred to as slaves), depending on the database engine.
    You can have up to 5 read replicas per master in standard RDS MySQL.
In our setup, we use one RDS MySQL primary (master) for write operations and 2 read replicas for read-heavy reporting workloads. This helps distribute the load and also serves as a fallback in case of failover.
### How comfortable with AWS and how much rate urself out of 5?
well, I’m very comfortable working with AWS — I’ve been using it extensively for over 4 years in a DevOps context. I’ve worked hands-on with services like:

EC2, S3, VPC, Security Groups, IAM, CloudWatch

EKS, ECS, ECR, and Load Balancers

RDS, DynamoDB, Lambda, Glue, Athena

I also handle multi-account access using IAM roles, set up monitoring and alerts with CloudWatch, and use S3 lifecycle policies, WAF, and Shield for security and cost optimization.
On a scale of 1 to 5, I would confidently rate myself 4.5/5 in AWS.

### about RBAC 
RBAC (Role-Based Access Control) is a security mechanism that restricts system access based on the roles assigned to users or services. Rather than assigning permissions directly to individuals, you assign them to roles, and users are assigned to those roles.

### What is log rotate job and how does it work
Log rotation is the process of automatically archiving, compressing, or deleting old log files to ensure that log directories don’t consume too much disk space over time.
This is typically handled by tools like logrotate in Linux.
In one of my production systems, logs from an app running on EC2 grew over 2GB daily. I set up a logrotate job to rotate logs daily, compress them, and retain only the last 7 days to prevent disk space issues and to ensure older logs were still archived.

### What is connection drain
Connection draining is the process of gracefully removing a backend server (like an EC2 instance or a pod) from a load balancer while allowing its existing active connections to complete, instead of terminating them immediately.

It’s commonly used during:
    Deployments
    Auto-scaling
    Rolling updates
    Manual server removal

### How to login VM if VM has private IP
If a VM has only a private IP and no public access, I typically use a bastion host (jump server) or VPN to securely access it from a public or management network

Session Manager (For AWS EC2)
In AWS, if SSM Agent is installed and IAM permissions are set, I use SSM Session Manager to connect to private instances without opening SSH ports

### Types of load balancers
1. ALB
2. NLB
3. GWLB

### How many NAT Gateways are needed for two public & two private subnets n a single VPC? Min & max?
So, we have two public and two private subnets
    one subnet is minimum but not enough as it not provide HA so have two private subnets in different Azs
### Explain TTL in DNS- how does it work, and when do we use it? Explain the Flow.
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

### How does weighted routing work in LB?
Lb is useful for distributing the traffic across the instances, nodes. Weighted routing in a load balancer means traffic is distributed between multiple backend targets based on assigned weight rather than evenly. 

Each backend (server, pod, or version) is assigned a weight, and the load balancer distributes traffic proportionally.
    If you have two backends:
    Backend A: weight 80
    Backend B: weight 20
Use AWS Route53 or ALB to do weight routing

### How does SSL work (Certbot, Let's Encrypt, AWS)? Explain the Flow.
SSL (Secure Sockets Layer), now replaced by TLS (Transport Layer Security), is a cryptographic protocol that ensures secure, encrypted communication between a client (browser) and a server.

    1. Client initiates HTTPS request to the server (e.g., a web app)
    2. Server responds with its SSL certificate, which contains:
        Public key
        Domain name
        Certificate Authority (CA) signature
    3. Client verifies the certificate using the CA's public key
    4. If valid, client and server use the public key to securely exchange a shared session key
    5. All further communication is encrypted with this session key
### how you ensure the best possible security for high availability architectures for 3 tier applications.- notAnswered

### diff between IAM Users and Roles
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

### Can you avoid the specific port traffic using SGs?
Security Groups (SGs) in AWS are stateful, allow-only firewalls. You can’t explicitly deny traffic, but you can avoid traffic on specific ports by not allowing them at all.

### What are the best password security practices used by your organisation?
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


###What are the security parameters we must consider while we are creating an EC2 instance for production?
    Security parameters for EC2 in production include:
    Using security groups to control traffic
    Implementing IAM roles
    Enabling encryption for EBS volumes
    Regular security updates and patches
    Monitoring and logging

### How can you protect the data in an AWS instance?
I protect data on an AWS instance by:
    Encrypting data at rest and in transit
    Restricting access using IAM, SGs, and VPC
    Using SSM over SSH
    Monitoring and auditing activity
    Taking regular, encrypted backups
This layered approach ensures both compliance and resilience against data breaches or loss.


### How can you connect from AWS to on-prem servers?
### Explain about the transit gateway and why do we use this?
    AWS Transit Gateway
    Acts as a central hub for routing between multiple VPCs and on-prem locations.

    ✅ Use Case:
    When connecting multiple VPCs and on-prem networks
    Supports both Direct Connect and VPN attachments
    Simplifies complex mesh network topologies

### I have created an EC2 instance named A, and I want to create another instance B. It should create an instance without deleting instance A. What can I do during this?

You can create a new EC2 instance B by using a different resource name in Terraform or by creating a new instance in the AWS Console without affecting instance A.

### How does the GSLB load balancer work?
GSLB (Global Server Load Balancer) is a DNS-based load balancing solution that distributes traffic across multiple geographically distributed data centers or cloud regions. Its main goal is to provide high availability, disaster recovery, and low-latency routing based on various factors.

🌍 How GSLB Works:
    Client makes a DNS request for a service (e.g., app.example.com)
    The GSLB system intercepts the DNS query and decides which data center or region should serve the request
    GSLB returns the IP address of the optimal site/server
    The client connects directly to that location
### Can we connect two different VM that are in a different Vnet.
Yes, you can connect two VMs in different VNets, but they cannot communicate by default. You need to explicitly configure connectivity between the VNets 

### Private Endpoints.
An endpoint is simply the entry point or destination through which a client communicates with a service, application, or resource over a network.
In cloud and networking contexts, an endpoint typically refers to a URL, IP address, or DNS name that is used to access a specific service.

### Use of Route53
Amazon Route 53 is used for:

    Registering domains
    Routing internet traffic to AWS resources
    Smart DNS routing (weighted, latency, geo)
    Health checks & failover
    Managing private/internal DNS in VPCs
    It plays a critical role in building highly available, globally distributed systems on AWS.
### Difference between S3 and EBS
Amazon S3 (Simple Storage Service) and Amazon EBS (Elastic Block Store) are both storage services offered by AWS, but they serve different purposes and are optimized for different access patterns and use cases.

Use Amazon S3 for object-based storage (files, media, backups) that is accessed via API or web.

Use Amazon EBS for block-level storage that behaves like a virtual disk and is mounted to EC2 for low-latency access.

### What is the difference between vertical and horizontal scaling?
The key difference between vertical scaling and horizontal scaling lies in how resources are added to handle increased load or demand.
1. Vertical Scaling (Scale Up/Down)
Involves adding more resources (CPU, RAM, storage) to a single server or instance.
2. Horizontal Scaling (Scale Out/In)
Involves adding more instances/servers to distribute load.

### Explain OSI models
The OSI Model breaks down network communication into 7 logical layers — helping engineers understand, design, and troubleshoot complex networks in a modular way.

### How do you did cost optimization in AWS?
Cost optimization strategies include:

📏 Right-sizing EC2 instances
💳 Using Reserved Instances and Savings Plans
📈 Implementing auto-scaling
🎯 Using Spot Instances for non-critical workloads
💾 Optimizing storage (S3 lifecycle policies, EBS volume management)
🔍 Regular review of unused resources
📊 Using AWS Cost Explorer and Budgets for monitoring

### How do you implement best security policies on AWS?
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
### how many types of policy, IAM policy are there? IAM policies?
Policy Type	           Attached To	                   Used For
Identity-based	IAM Users, Groups, Roles	Most common access control
Resource-based	AWS Resources (e.g., S3)	Sharing resources across accounts
Permissions boundaries	IAM Users, Roles	Limit maximum allowed permissions
Session policies	Temporary credentials	Scoped, time-limited access

### what is the difference between the S3 bucket policies and acls?
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

### Expalin about fargate?
AWS Fargate is a serverless compute engine for containers that works with Amazon ECS (Elastic Container Service) and Amazon EKS (Elastic Kubernetes Service).
With Fargate, you don’t need to provision or manage EC2 instances — AWS automatically runs containers for you based on CPU and memory requirements.

###. what is cloud watch uses cases.
Amazon CloudWatch is a monitoring and observability service that provides real-time visibility into resource utilization, application performance, and operational health.
It’s commonly used for metrics, logs, alarms, dashboards, and automated responses.

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

9.what is limitations of lamda
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

### how lamda works containers
AWS Lambda supports running functions as container images. Instead of uploading code as ZIP files, you can package your Lambda as a Docker container image (up to 10 GB) and deploy it. This provides more flexibility in dependencies, tooling, and language support.

### storage gateway in AWS
AWS Storage Gateway is a hybrid cloud storage service that enables on-premises applications to seamlessly use AWS cloud storage.
It acts as a bridge between on-prem infrastructure and AWS, allowing you to securely store data in the cloud while keeping frequently accessed data locally cached.

### I’m an admin but I don’t have access to the S3 bucket ? 
### What are the top 5 infra attacks, and how do you mitigate them?- notAnswered
### EC2 instance is unreachable, and it’s not a security group issue. What’s your next step?
### An S3 bucket was made public by mistake. How do you secure and audit it?
### RDS migration with minimal downtime – how would you approach it?
How would you set up networking in vpc
How you will direct traffic to and from a instance in private subnet
 how did you managed security for application level? 
What types of nodes did you deploy in AWS?
What is the difference between Interface Endpoint and Gateway Endpoint?
How did you set up ECS using EC2 instances?
Can't we configure Route 53?
If we want to configure third-party domains like GoDaddy in Route 53, how do we do that?
What is the difference between AWS Config and AWS CloudTrail?
What are the node groups you used in AWS EKS?
What are the types of node groups in AWS EKS?
If a user wants to access the S3 bucket, what are the processes?
How does VPC Peering work?
How does Transit Gateway work and how did you configure it?
If we connect VPCs to the Transit Gateway, what will you update in the VPC Route Table?
For all VPCs, will you configure the Transit Gateway attachment with CIDR range?
What is Route 53? 
What is WAF (Web Application Firewall) and AAF (Application Access Firewall)?
What is VPC Flow Logs and how will you track the IPs hitting the VPC?
How to filter a particular IP from AWS CloudWatch Log Group?
If you are storing logs in S3 Bucket, how will you track that particular IP?
How do you take the backup of AWS Services?
I need to send a logs from EC2 to S3, create an automation part where we need to take the log file, check the CPU metrics and send an alarm through cloud watch to user's.
If thre is an instance we have security group and web application firewall enabled, DDOS attack enabled will it protect from Bot attack.
.What is DNS Resolution. Suppose I have new system with no cache what happens in the background.Step by step process.
If you’re migrating a monolithic application from on-prem to Cloud and the system has its local file system, which file system you will use in AWS.
How will you store all the configurations related to your monolithic app in Cloud,
DB migration, how will you sync the data
You need to connect your DB running in private subnet, not using NAT gateway or NAT instance or bastion host, what are the other options,
what is tcp and udp
what is ipv4 and ipv6
what is pid
what metrics is used to monitor ec2 instance cpu, memory in aws