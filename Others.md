### What to do if an application is down
Step 1: Confirm the Outage
    Is the issue real or user-specific?
    Try accessing the app via:
    UI (URL, app)
    curl or ping from CLI
    Check uptime monitor or alerting tools (e.g., Prometheus, New Relic, Datadog, ELK)
Step 2: Check the Application Logs
Logs are your first clue.
Step 3: Check Service Status and Resource Health
    Is the app container running?
    Are pods healthy (in Kubernetes)?
    Any OOM, CrashLoopBackOff, or restarts?
Step 4: Check Dependencies
    Is the DB, Redis, external APIs, or Vault reachable?
    Is there a network issue, DNS issue, or certificate expiry?
Step 5: Check Deployment/Release History
    Did someone deploy new code or config recently?
    Was there a recent infrastructure change (e.g., load balancer, secrets, volumes)?
Step 6: Check Monitoring and Alerts

If an application goes down, I start by confirming the outage through logs and monitoring tools. I check pod or service health, investigate recent deployments or configuration changes, and verify if any dependent services like databases or APIs are unreachable.

Based on the findings, I either restart the app, rollback a bad deployment, fix environment/config issues, or escalate if needed. I always make sure the fix is verified and follow up with RCA and preventive measures.”

### what steps to take so that the issue will not happen in future 

After resolving an incident, I always conduct a root cause analysis and take preventive steps like adding proper monitoring, readiness/liveness probes, CI/CD safety gates, and better logging. I also fix any infrastructure misconfigurations, update runbooks, and automate recovery where possible.

These measures help ensure the same issue doesn’t occur again and improve the overall resilience of the system.

### If you're facing performance issues on a server, how do you troubleshoot?

1. Define the Problem Clearly
    What kind of performance issue?
    High CPU?
    Memory usage?
    Disk I/O?
    Network latency?
    When did it start?
    Is it impacting users or services?
2. Check System Resource Usage
``` bash
top          # Live CPU and memory usage
htop         # (Better interactive view)
free -m      # Memory stats
df -h        # Disk space usage
iostat -xz 1 # Disk I/O performance
vmstat 1     # CPU/memory/load summary
netstat -tulnp  # Listening ports and connections
```
3. Check Application-Specific Logs
4. Review Recent Changes
5. Use Monitoring Tools

Based on the root cause, I will fix the issue like scaling resources, optimizing code, or reverting configs. 

### How do you enable passwordless authentication between two servers?
To enable passwordless SSH authentication between two Linux servers, I use SSH key-based authentication. This avoids manual password entry and is commonly used for automation, CI/CD, and secure server-to-server communication.

### explain about blue green deployment strategy 
Its a powerful pattern for zero-downtime deployment strategy where you maintain two identical environments:
`Blue`: The current live/production environment
`Green`: The new version of the application
How it works:
- Blue is live and serving producition workload
- you create a exactly same env, and deploy new verions to GREEN
- after validation, like tests,health checks, simply swtich traffic to GREEN
- if something goes wrong, you rollback instantly by switching traffic back to Blue.

### have you worked on production deployment activity ?
So yes, I have hands-on experience in planning, executing, and validating production deployments with a strong focus on zero downtime, observability, and rollback readiness.


### how frequently do we deploy to production in your current project
Completely depends on project to project, we do have quatrely deployement, every two weeks for few projects.

### You have deployed a web app and it was working fine but application went down, and all networking are fine and related ports are open, how to troubleshoot.
In this scenario, since networking and ports are verified, I would take a layered troubleshooting approach focusing on application, service, and infrastructure layers to isolate the root cause quickly.

1. Check application logs
First, I check the web application logs for errors such as crashes, memory leaks, unhandled exceptions, or DB connectivity issues.
2. Check Service Status (Web Server / App Process)
Ensure the application process or web server (e.g., Nginx, Apache, Node, Tomcat) is still running.
3. Health Checks & Probes
In Kubernetes, I check liveness/readiness probes:
4. Resource Exhaustion (CPU, Memory, Disk)


### App is down and throwing 503 err code, what steps should be taken
An HTTP 503 Service Unavailable error usually means the server is temporarily unable to handle the request — often due to the app being unreachable, overloaded, or misconfigured behind a load balancer

### What is SLI, SLO, SLA
1. SLI – Service Level Indicator
An SLI is a quantitative metric that measures a specific aspect of service performance.

    ✅ Examples:
    Availability: 99.9% success rate
    Latency: 95% of requests served under 300ms
    Error rate: 0.01% request failure rate

    👉 Think of SLI as the "What we measure"
2. SLO – Service Level Objective
An SLO is a target or goal set for an SLI — the performance level you want to maintain over a period of time.

    ✅ Examples:
    Availability SLO: "Our API should be available 99.95% of the time over a rolling 30-day window."
    Latency SLO: "90% of login requests should respond within 200ms."

    👉 SLOs help define acceptable reliability and guide engineering priorities.
3. SLA – Service Level Agreement
An SLA is a formal, contractual agreement between a provider and customer that defines minimum acceptable service levels — usually with penalties for violations.

    ✅ Examples:
    "99.9% uptime monthly; if we fall below that, you get a 10% billing credit."

    👉 SLAs are externally facing, legally binding, and enforced with consequences.
### A sudden surge in traffic causes a web application to become unresponsive what will be the steps you will take to mitigate
A. Check Health of Components
2. Scale Resources

### what is scrapper?
A scraper is an automated tool that extracts data from websites or APIs, commonly used for monitoring, data mining, or aggregation. It can be extremely useful, but must be used ethically and legally.


## have you written any automation scripts in your daily tasks-notAnswered

## How would you structure disaster recovery for your applciation?

Disaster Recovery (DR) is a strategy and set of processes to ensure that your systems and data can be recovered quickly and reliably after a catastrophic failure — such as data loss, infrastructure failure, region outage, or cyberattack.

I structure disaster recovery by first defining RTO/RPO, then architecting for fault tolerance using multi-AZ and cross-region strategies. I use automated backups, cross-region replication, Route 53 failover, and infrastructure-as-code to ensure fast recovery. I also run regular failover tests to validate that recovery plans are effective.

✅ 2. Application Layer Resilience
🔹 Stateless Services
    Deploy across multiple AZs behind an Application Load Balancer (ALB)
    Use Auto Scaling Groups (ASG) for failover
    Health checks to detect unhealthy instances and redirect traffic
✅ 3. Database DR Strategy
    Type	           Method
    RDS	            Multi-AZ for HA, cross-region read replica for DR
    DynamoDB	    Global Tables for real-time replication
    S3	            Versioning + Cross-region replication
    EFS	            Backed up with AWS Backup or replicated manually

✅ 4. Backup and Snapshots
    Automated EBS and RDS snapshots
    Use AWS Backup with retention policies
    Store backups in multiple regions (or replicate them)

✅ 5. Infrastructure-as-Code for Recovery
    Use Terraform or CloudFormation to:
    Re-create VPCs, EC2s, Security Groups, etc.
    Quickly spin up infrastructure in a DR region

✅ 6. DNS-Based Failover
    Use Amazon Route 53 for:
    Health checks
    Failover routing to DR region
    Active-passive or active-active setup


## What is DNS? When you type google.com. What exactly happening in the background?
DNS (Domain Name System) is like the internet's phonebook — it translates human-readable domain names (like google.com) into IP addresses (like 142.250.182.14) so that computers can communicate with each other

## Check the status of nginx/apache service that was used to host the web p
```bash
sudo systemctl status nginx
```
```bash
sudo systemctl status apache2     # Debian/Ubuntu

sudo systemctl status httpd       # RHEL/CentOS/Amazon Linux

```
## Check if any firewall/security group rules are blocking the flow of traffic
 - check instance security inbound rules
 - check NACls in VPC
 -on the instance, check iptables or UFW status

## Ping/telnet won't make sense since networking side is fine
I will check if the application is running or active, also check logs 


## Do nslookup for dns resolution if that works fine

Absolutely! Using nslookup is a great way to verify DNS resolution

✅ What Is nslookup?
nslookup (Name Server Lookup) is a network utility to query DNS records for a domain and verify that the name resolution is working correctly.
`nslookup google.com` -if you see Ip address, then DNS working fine

##Check SSL certificate if it has gone corrupt and is working fine

## Lack of memory on the web app server due to which it is failing to load the resources for the app

When a web app fails due to low memory, I start by checking memory usage using top, vmstat, or docker stats. If an app is over-consuming, I profile for memory leaks and restart the service if needed. I also look at system logs for OOM kills. Long-term, I either optimize memory usage, add swap space, or increase instance size—and always set alerts to catch memory pressure early."

## /var partition is 90% full. What’s your immediate action?

If /var is 90% full, I start by finding the top disk consumers using du and df. I usually see large log files or Docker artifacts. I clean up logs and caches, and prune Docker if needed. If it’s a recurring issue, I configure logrotate, add monitoring alerts, and consider resizing the disk or isolating /var on a dedicated volume."


## You’re locked out via SSH with no root access. How do you recover?
If I'm locked out of an EC2 instance, I stop the instance, detach the root EBS volume, mount it to a helper instance, fix the SSH keys or system configs, and then reattach and restart the instance. I follow AWS's rescue pattern to avoid data loss and regain access safely.

## Add 50GB to /opt using LVM without any downtime. What are the steps?
To extend /opt by 50GB using LVM without any downtime, I would attach a new EBS volume, create a physical volume on it, extend the volume group, then resize the logical volume and the filesystem—all live, without unmounting or rebooting



## Describe a situation where you had to improve the reliability of a critical system.
At my previous company, we had a critical microservice that handled authentication across multiple applications. It was hosted on an EC2 instance with no redundancy, and occasionally went down due to memory leaks or instance failures, causing outages for thousands of users. I was tasked with improving its reliability without impacting ongoing development."

✅ Steps I Took
🔹 1. Containerized the Service
    I dockerized the service to ensure consistency across environments
    This also helped eliminate issues caused by manual config drift

🔹 2. Migrated to Kubernetes (EKS)
    Deployed the service to Amazon EKS with:
    2 replicas across multiple availability zones
    Readiness and liveness probes to auto-restart unhealthy pods
    This added high availability and self-healing

🔹 3. Implemented Resource Limits & Auto-scaling
    Set memory/cpu requests and limits in the deployment manifest
    Enabled HPA (Horizontal Pod Autoscaler) based on CPU metrics
5. Added Monitoring & Alerts
    Integrated Prometheus + Grafana to track

## What is Web Application Firewall
A Web Application Firewall (WAF) is a security layer that protects web applications by monitoring, filtering, and blocking malicious HTTP/S traffic — specifically attacks that exploit vulnerabilities in your app like SQL injection, XSS, CSRF, etc.

Purpose of WAF
    Protects against OWASP Top 10 threats
    Acts like a shield in front of your web app
    Helps with zero-day mitigation while developers patch the code
    Prevents bot traffic, DDoS, and excessive crawling
    
A Web Application Firewall protects my web applications by inspecting and filtering HTTP/S traffic. It helps mitigate common attacks like SQL injection, XSS, and bot abuse. I typically use AWS WAF with custom rules to protect APIs and frontend services behind ALBs and API Gateway."

## which are the production incidents you had attended , pls explain

✅ 1. Application Downtime Due to Exhausted Disk Space

🔍 Issue:
Root volume (/var) filled up to 100% due to unrotated log files. The application crashed and health checks failed.

🛠️ Action Taken:

    Logged in via SSH and ran du -sh /var/* to identify large directories
    Cleared old logs and enabled logrotate
    Increased EBS volume size and used LVM to extend the affected mount without downtime

📈 Outcome:

    Services restored within 15 minutes
    Implemented CloudWatch alerting for future disk usage

✅ 2. High Latency Due to Kubernetes Pod Memory Leaks

🔍 Issue:
    Auth service pods were being OOM-killed every few hours, causing API timeouts.

🛠️ Action Taken:
    Analyzed metrics in Prometheus → memory spike over time
    Set resource limits and added horizontal pod autoscaling
    Updated app version with memory leak fix

📈 Outcome:
    Latency dropped by 70%
    Added alerts for container restarts and OOM events

✅ 3. Jenkins Pipeline Failing Randomly During Deployments
🔍 Issue:
    kubectl apply would randomly timeout during deployment to EKS due to kube-apiserver throttling.

🛠️ Action Taken:

    Added retry logic in Jenkins pipeline with exponential backoff
    Moved heavy deployment jobs to off-peak hours
    Tuned kubectl QPS/burst values

📈 Outcome:
    Deployment success rate reached 100%
    Reduced rollout time by 40% 

## How does SSL certs works
## Check SSL certificate if it has gone corrupt and is working fine
## What is SSL/TLS Handshake.
## I've a client and remote machine, how do certs communicate b/w them