================================
Interview questions year 2025-:
===============================

Company:Techdome -:

### Kubernetes architecturees 
Kubernetes has two things
1. Control plane (Master Node)
    This is the brain of the kubernetes cluster, it manages the state of cluster.
    components in control plane
    1. API server:
        Entery point for all commands(via kubectl). It validates and process REST requests
        It exposes k8s to the external world, it takes all the incoming requests
    2. Etcd
        The cluster key-value store, it holds the state and configuration of cluster, in other words backing all clsuter data
    3. Kube scheduler 
        Assigs newly created pods to appropriate nodes based on the resources,availability, affinity rules.
    4. Control Manager
        It is resposible for running the controllers, k8s has many controllers like node controller, replica controller. 

2. Data plane (Worker Node)
    1. Kubelet
        Talks to API, and responsible for ensuring the pods are running. if pod isn't running it notify API server.
    2. Kube-proxy
        Maintain networking rules and forward traffic to the correct pods.
        It provides Networking, Ip addresses, and load balancing
    3. Container runtime
        you need a runtime to run containers or docker, its responsible for running the actual containers.

### Deployment vs stateful set
    In kubernetes, both deployment and stateful sets are used to manage pods, but they serve different purpose based on whether the application is stateless or stateful.

    A deployment is used for stateless application. it manages replica sets and ensures that any pod can be replaced with another as they are identical.
        Ideal for web servers, microservices,REST APIs.

    On the other hand, Stateful sets are dsigned for stateful applications that need stabel identiy, persistent storage and stable DNS, and order deployment. 
    Ex: databases like MongoDB kafka

### Explain Docker networking and types of network. What is the default network.
 Docker networking enables communication between containers, the host, and external networks (like the internet). Docker automatically sets up networking for containers using different network drivers.
    Types of Network:
    - Bridge (default)
    - Host
    - None
    - Overlay (Swarm only)
    - Macvlan
The default network is Bridge

### Terraform provisioners. 
Provisioners are used to execute scripts or commands on a local or remote machine after resourceis created.
Types of Provisioners:
1. File
    Used to upload a file a from local to remote instance
2. remote-exec
    Runs commands/scripts on the remote resource  via SSH or WinRM
    Used to install packages, modify files, run configuration tools (like Ansible, Chef).

3. Local-exec
    Runs commands locally (on the machine running terraform apply).

### Terraform statefile

Terraform uses a statefile to track the real infrastructure it manages.
Why it is needed?
    Keep track of what has been created and what's need to change
    Helps `Terraform plan` and `Terraform apply` know the difference between your.tf code and what's deployed.
    Without state, Terraform wouldn't know what resources it manages.
    It maps your Terraform configuration to real-world resources (e.g., AWS, Azure, GCP).

there is a disadvantage of statefile being stored in local as it contains sensitive data like:
    passwords, secrets, Accesskeys

solution is remote backend;
A remote backend is where Terraform stores your state file `Terrafrom.tfstate` remotely instead of saving it locally on you machine. 

In Aws, store the state file in an S3 bucket
    locks the state using using DynamoDB
    Encrypt state using server-side encrytption.

### Docker image vs container
    Docker Image is a standarized package of your application that includes all the files, binaries, libraries, and configuration to run a container

    Container is actually running instance of an image, where the application runing.

### Docker bind mount vs volume 
Docker supports two main types of persistent storage: volumes and bind mounts.

Volumes are fully managed by Docker and stored under /var/lib/docker/volumes. They’re ideal for production environments because they’re portable, secure, and easier to back up.

On the other hand, bind mounts directly map a host directory into the container, which is useful for local development when I want to reflect changes immediately. However, bind mounts are more prone to permission issues and tightly couple the container to the host filesystem, so I avoid using them in production.

==================================================================================>
Company:LTIMindtree -:

### Can we install docker inside a container.
Yes, we can install docker inside a container

### If we have created 3 instances using terraform script and the instance names are mentioned as a list
Suppose we removed 2nd instance name from the list and applied script again then what will happen to the already 3 instances created before.

It depends on how you create the instances whether using `count` or `for_each`. 

“If we use `count` and remove an item from the list, Terraform reindexes the resources. This causes existing instances to be destroyed and new ones created even if the instance name hasn’t changed.

To avoid this, I prefer using `for_each` which uses the instance name as a key — that way, only the specific removed instance gets deleted, and others remain untouched.”

### If we have 5 stages in a jenkins pipeline and 5th stage having syntax error then what will happen if we run the pipeline.

The pipeline will be failed even before any stage is executed, because Jenkins parse the whole Jenkins file at the start,syntax errors caught during parse phrase, not at runtime.

### What are some main differences between scripted and declarative pipeline.
The main difference between Declarative and Scripted pipelines is in their syntax and flexibility.

Declarative pipilines use a structured , easy to write, read, and validate. 

Scripted pipelines are written in `groovy` language, provide more control and flexibility, which is useful for more complex logic and dynamic behaviour. 

### Difference between code quality and code coverage.
Code quality refers to how clean, maintainable, and bug-free the code is. It’s usually measured through static analysis tools like SonarQube, which detect issues such as code smells, complexity, or security vulnerabilities

Code coverage, on the other hand, measures how much of the codebase is exercised by automated tests. Tools like JaCoCo or Istanbul help calculate what percentage of code is tested.

### What is default qualitygate in sonar
The default qualitygate in soanr is called `sonarway`
A quality gate is a set of conditions that a project must meet inorder to be considered as have passed code quality standard.
There are default conditions in `sonar way` quality gate.

which includes conditions like at least 80% code coverage on new code, no critical bugs or vulnerabilities, and no more than 3% duplicated code.

### How to run a yaml manifest without a yaml manifest file created.
To run a Kubernetes YAML manifest without creating a file, I use kubectl apply -f - and pass the YAML content directly through stdin
===============================================================================================================================================================================================================>
Company:HCL 
exp--> 4-5 

### What is git branching strategy used in your organisation
### How deployment is done in different environment using git repo

### How and from where to clone repo, is there any local repo you are using and then transferring from local to remote or how? 
In my workflow, I usually clone repositories directly from a remote Git server such as GitHub, GitLab, or Azure Repos, depending on the organization.

### What is PAT
PAT (Personal Access Token) is a secure, token-based alternative to passwords used to authenticate with Git hosting platforms like: GitHub,GitLab etc

### How to configure sonarqube
There are couple of things to configure SonarQube
    1. Download and install sonarqube then start it
       Access it on localhost:9000, generate token
    2. Install SonarQube plugin in Jenkins
       Configure SonarQube in Jenkins
        -configure system-
        add sonarQube and provide Name, Server URL, sonar token 
        then create sonar step in jenkins pipeline

### How to handle merge conflict in git. If 2 people working on same file and did the commit and got conflict err, in how many ways it can be solved.
Merge confilcts occurs when git can't automatically resolve changes to the same part of file. I usaullly resolve them manually or using a merge tool, and then staging and commiting the resolve file.

There are multiple ways to resolve:
    Keep current (ours)
    Keep incoming (theirs)
    Merge both manually
    Use a visual merge tool

### What is the output of sonarqube, how to fix if any smell code/vurnabilities found
SonarQube outputs a detailed report with metrics like bugs, code smell, vulnerabilities, duplicaitions, code coverages. It also provides quality gate status that determines if the code meets predefined quality standards.

When SonarQube flags code smells or vulnerabilities, I examine the specific rule that triggered it, refactor the code accordingly (e.g., modularizing long methods, removing hardcoded secrets, fixing security flaws), and then re-run the analysis to confirm the issue is resolved. This workflow helps ensure high-quality, maintainable, and secure code before deployment.”

### Where do you write the code/yaml file for pipeline
I write our pipeline code in the root of the Git repository, using a `Jenkinsfile` for Jenkins or .yml files for GitLab/GitHub/Azure

### What is inside docker file
A Dockerfile contains a set of instructions to automate the creation of a Docker image. It includes steps like setting the `base image (FROM)`, `installing packages (RUN)`, `copying code (COPY)`, `defining environment variables (ENV)`, `exposing ports (EXPOSE)`, and specifying the default command using `CMD or ENTRYPOINT`.

When we build the image with docker build, these instructions are executed sequentially to create a layered, reusable image.”

### how to schedule pipeline, lets say i have validated the pipeline with some update and i want to schedule it to stage/main branch, how to do? 
In Jenkins, I schedule pipelines using the `cron` trigger inside the jenkinsfile. After validaitng feature branch, we use scheduled jobs to promote to staging or main branch.

For example, we might set a job to run every night at 2 AM using cron('H 2 * * *'), pulling from the main branch and deploying it to staging.


==========================================================================================================================================================================================================================>
 Company:Wipro-:

### What is your day to day activity

### If there is file which is being used by 2 customers, and need to deploy that file in k8s cluster and on prem as well, how to do that?
If I need to deploy the same file to k8s and on-prem, I will store the file in a central location like git repo, nexus, S3 etc.
For Kubernetes, I use a ConfigMap or volume mount in the deployment spec, and manage deployments using Helm or GitOps.
For on-prem, I use Ansible, SCP, or custom scripts to copy the file and restart the app if needed.

### What is the issue with using large file image in dockerfile
Using large files or images in a Dockerfile can slow down the build, push/pull, and deployment process. It also increases storage usage on Docker hosts and CI agents, and makes caching less efficient.

Large base images may introduce unnecessary vulnerabilities, so I prefer using slim or Alpine-based images.

### How to deploy an app to k8s cluster in terms of app deploy only ( basically explain CD part)
Deploying an app to cluster is depends on how you set your CD
 1. You need to update the new version of image in yaml file
 2. Apply to the Kubernetes Cluster using   `kubectl apply`

 I use GitOps approach to achieve CD using tool called ArgoCd.

### If a secret is stored in a Vault (like HashiCorp Vault) and the Pod consuming that secret is down, you’ll need to perform TSG (Troubleshooting Guide)
1. Check Pod Status and Logs
2. Check Vault Access Method
Depending on how your app accesses Vault secrets, your TSG will vary.
3. Check Vault Itself
Ensure the Vault server is:
    ✅ Running
    ✅ Unsealed
    ✅ Accessible from the pod's namespace
4. Restart Pod or Trigger Re-deploy

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

### If any service is down for more than 2 weeks and customer is asking for update, what will you tell to customer?

###  How to troubleshoot the issue and what will be checked during the process
 answer same as  ` What to do if an application is down`

### what steps to take so that the issue will not happen in future 
After resolving an incident, I always conduct a root cause analysis and take preventive steps like adding proper monitoring, readiness/liveness probes, CI/CD safety gates, and better logging. I also fix any infrastructure misconfigurations, update runbooks, and automate recovery where possible.

These measures help ensure the same issue doesn’t occur again and improve the overall resilience of the system.


=============================================================================================================================================================================================================================>
L1 Questions:

### In Git, explain the push and pull commands.
Push: use to push code to remote repository
Pull: Download or pull from remote repo to local development env

### What is the use of Git tags?
1. Marking Releases
Git tags are commonly used to mark stable versions of code.
2. Versioning
Helps associate a Git commit with a semantic version 

Git tags are used to mark specific commits, typically for versioned releases like v1.0.0. Unlike branches, tags don’t change and are ideal for pointing to stable snapshots of code.

We use tags in our CI/CD pipelines to trigger deployments, track production versions, and simplify rollbacks. I prefer annotated tags for releases since they include metadata like author and message.

### What are the different types of branches in Git?
Usaully, long live branches and short live branches
Long live branches are Master/ main and develop
Short live branches are feature, hotfixes,

###how do you write an Ansible playbook, and what client requirements do you consider?
### In Python, what are lists and tuples, and how do they differ?

### In CloudWatch, what is the use of log groups and Cloud trails?
Log groups are used to logically organize and manage logs from AWS services and custom apps.

CloudTrail, on the other hand, captures API calls and account activity across AWS. It's used for auditing and security monitoring.

### In Terraform, what is the purpose of init, plan, and apply commands?
`Terraform inti`- Initilizes the working directory by downloading necessary plugins and setting up the backend
`Terraform plan`- Shows proposed changes, whata will be created, changed, destroyed
`Terraform apply`- actually provisions the resources as described in the plan.

### What happens if the Terraform state file is accidentally deleted?
If `Terraform State` file is accidentally deleted, it loss the track of the existing infrastructure.
any subsequent apply, plan commands will recreate all the resources as new, causing duplication or failures.

The first step is to check for the backup- either in `.terraform/`, S3 versioning, or Terraform Cloud state history. 
Use terraform import (Rebuild State)
If no backup is available, we can use terraform import to rebuild the state resource

### What is the purpose of creating S3 bucket policies?
The purpose of creating policies is to control the access to S3 bucket and its contents. It defines who (principals) can do what (actions) on which resources, and under what conditions.

### How do you maintain the lifecycle of an S3 bucket?
To maintain the lifecycle of an S3 bucket, I configure lifecycle policies that automate object transitions between storage classes and delete data after a defined period

This helps manage costs and data retention — for example, I might move logs to Glacier after 30 days and delete them after 1 year. I typically define these rules using Terraform or the AWS Console, and align them with business compliance requirements.”

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

🔥 L2 Questions:
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

### How do you configure a Grafana dashboard?
1. Connect a Data Source
    Go to Configuration → Data Sources
    Add a supported source like:
    Prometheus (most common for DevOps/K8s)
2. Create a Dashboard
    Go to + → Dashboard → Add new panel
    Choose a data source (e.g., Prometheus)
3. Customize Panels
Choose panel type: Graph, Gauge, Table, Stat, Heatmap

4. Add Alerts (if needed)

In a recent Kubernetes setup, I created a Grafana dashboard using Prometheus data to monitor node CPU, pod memory usage, and container restarts. I added a variable for namespace so teams could filter their services. I also set alerts to Slack for high CPU usage using Webhooks.

### What kind of CI/CD pipelines are you familiar with?
I am familiar with couple of pipelines but I have been wide working on Jenkins

### In Kubernetes, if a pod is in a pending state, how do you troubleshoot?
If pod is in a pending state, it means its not assigning to any node. Firstly, I do  `kubectl describe pod <pod-name> -n <namespce>` to check the reason why pod is not scheduling to any node. 
I also check pod resource request like does it need more resources, check node conditions, or any affinity rules, node selector.

Depends on the issue, I will trouble shoot.

### If Docker containers are consuming too much disk space, how do you fix it?
If Docker containers are consuming too much disk space, I usually troubleshoot and fix the issue by cleaning up unused resources like dangling images, stopped containers, unused volumes, and build cache. 

### In Linux, how do you attach and detach a filesystem?
In Linux, attaching and detaching a filesystem refers to mounting and unmounting a storage device (like a disk partition, USB drive, or network volume). 

### How do you print the last 15 lines of a file in Linux?
You can print the last 15 lines of a file in Linux using the tail command

### How do you enable passwordless authentication between two servers?
To enable passwordless SSH authentication between two Linux servers, I use SSH key-based authentication. This avoids manual password entry and is commonly used for automation, CI/CD, and secure server-to-server communication.9705734265

=============================================================================================================================================>

Company:startup company -:

### Whats ur organisation current cicd process and tools

### How comfortable with AWS and how much rate urself out of 5?
well, I’m very comfortable working with AWS — I’ve been using it extensively for over 4 years in a DevOps context. I’ve worked hands-on with services like:

EC2, S3, VPC, Security Groups, IAM, CloudWatch

EKS, ECS, ECR, and Load Balancers

RDS, DynamoDB, Lambda, Glue, Athena

I also handle multi-account access using IAM roles, set up monitoring and alerts with CloudWatch, and use S3 lifecycle policies, WAF, and Shield for security and cost optimization.
On a scale of 1 to 5, I would confidently rate myself 4.5/5 in AWS.

### Can u pls write terraform file to provision the Ec2 instance in a public subnet in a VPC?

###R u using Dockerfile? u r build the dockerfile by codebuild?
Yes I do use Dockerfile in most of our applications to containerize our applications. But I use Docker platform to build the images. If something comes up with codeBuild, I can do that.

### How many containers can run in a pod?
A kubernetes pod runs one or more containers in a pod but in most cases we run a single container per pod to follow the single-responsibility principle.

### in ur projects how many containers u ran? can u give me the use case where can run 4-5 containers in a pod?
Typiclaly, we run one container a pod, we run two containers in a pod, which is sdie car container. I implemented service mesh like envoy as a side car container,

### did u configure Prometheus and Grafana?
Yes, we have setup monitoring across our organization using monitoring with prometheus and grafana including for microservices applications.

### What securities measures/tools u were taken in ur cicd pipeline?
Security is most important for any application or organization, I do take it as one of the top priority whenever I build or create something, could be in cloud or outside of cloud.

Talking of CICD, I enforced many security measures
firstly, Credential & secret management:
    We use Jenkins secret credentials to store the API keys, password, and never store plain text secrets.
2. SAST(Static code analysis)
    Integrated with Jenkins pipeline to scan for:
        Code quality issues, vulnarabilities
3. Image scaning
    use trivy to scan all the docker images before push ECR or DockerHub
4. Access Control
    Implememts RBAC, provide least privilege priciples 

### about RBAC 
RBAC (Role-Based Access Control) is a security mechanism that restricts system access based on the roles assigned to users or services. Rather than assigning permissions directly to individuals, you assign them to roles, and users are assigned to those roles.

=================================================================================================================================================================================================>
Company:Infosys -:

###Introduce yourself 
### git commands used in day to day activities
As a DevOps engineer, I use Git daily for version control, CI/CD workflows, infrastructure automation, and collaboration.
git clone <repo-url>
git pull origin <branch>
git checkout -b feature/xyz
git checkout main
git branch -d old-branch
git push origin <branch>
git merge <branch>
git rebase main
git status
git diff
git log --oneline --graph

### write sample docker file
``` bash
    FROM node:14
    WORKDIR /app
    COPY package.json
    RUN npm install
    COPY . .
    EXPOSE 3000
    CMD ["npm", "start"]
```

### write sample terraform resource file
### Difference between git rebase and git merge
Git merge:
```bash
    git checkout feature
    git merge main
```
    Merges `main` into `feature`, adds a merge commit, and keeps both histories.

Git Rebase:
```bash
    git checkout feature
    git rebase main
```
    Replays feature commits on top of main, creating a clean linear history.
`git merge` is safer and preserves history with a merge commit, making it ideal for shared branches. `git rebase` creates a cleaner, linear history by rewriting commits — perfect for feature branches before merging via pull request. I typically use rebase in local development and merge during collaborative work or CI/CD integration.


### difference between cmd and entry point 
Both CMD and ENTRYPOINT define the default behavior of a Docker container at runtime, but they differ in purpose and flexibility.

CMD provides default commands for the container, which can be overridden at runtime. ENTRYPOINT defines the executable that will always run when the container starts, and CMD arguments are passed to it.

 Use **ENTRYPOINT** when your container is meant to run only one main process, like:
   nginx, python app.py, java -jar app.jar, bash script.sh

🔹 Use **CMD** to provide default options for that main command, like:
    Default flags, file paths, or environment settings

### explain about Prometheus and grafana
Prometheus and Grafana are two of the most widely used open-source tools for monitoring and observability in cloud-native, containerized, and DevOps environments.

What is Prometheus?
Prometheus is a metrics-based monitoring system that collects and stores time-series data (numeric metrics over time).

What is Grafana?
Grafana is a visualization and dashboard tool used to display data from Prometheus and other sources like InfluxDB, MySQL, CloudWatch, Elasticsearch, etc.

### what will be your approach if pod.yaml failed
1. Check the Immediate Error
I first look at the error returned by `kubectl apply -f pod.yaml` — it usually points to YAML syntax issues, missing fields, or schema validation errors.
2. Validate the YAML
```bash
kubectl apply --dry-run=client -f pod.yaml
```
3. Inspect the Pod Events
```bash
kubectl describe pod <pod-name>
```
4. View Container Logs
```bash
kubectl logs <pod-name>
```
Based on the issue, I take action:

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

======================================================================================================================================================================================================================>
Company:Infosys -:

Introduction 
### Current Company project related questions
### What task and activities do you do on daily basis

### What is Kibana 
Kibana is an open-source data visualization and exploration tool that's used primarily with Elasticsearch. It provides a web-based interface to search, view, analyze, and visualize log and time-series data stored in an Elasticsearch index.

### How does Prometheus collect metrics
Prometheus pulls metrics from targets at defined intervals, using an HTTP GET to the /metrics endpoint. These metrics are stored in its time-series database and can be queried using PromQL or visualized with Grafana.

### What is log rotate job and how does it work
Log rotation is the process of automatically archiving, compressing, or deleting old log files to ensure that log directories don’t consume too much disk space over time.
This is typically handled by tools like logrotate in Linux.
In one of my production systems, logs from an app running on EC2 grew over 2GB daily. I set up a logrotate job to rotate logs daily, compress them, and retain only the last 7 days to prevent disk space issues and to ensure older logs were still archived.

### What is Terraform and how do you use terraform in your project and what all resources have you provisioned 

### What are data sources for Grafana, Kibana
Grafana supports multiple data sources, primarily used for metrics and time-series data. 
Prometheus,Loki, CloudWatch,MySQL / PostgreSQL.

Kibana is tightly integrated with Elasticsearch and is used primarily for log analytics and full-text search.

### How do you receive alerts in your project and how is it setup
In our project, we have a centralized monitoring and alerting setup using Prometheus + Alertmanager + Grafana, and in some cases, CloudWatch Alarms for AWS services. The alerts are integrated with collaboration tools like Slack, Microsoft Teams, or email to ensure quick response from the team.

### How do you handle disk, CPU alerts 
I handle CPU and disk alerts by combining real-time metrics collection, smart alerting rules, and automated or manual remediation steps. The key is to act before users are impacted and to prevent recurring issues through automation and scaling strategies.

=====================================================================================================================================================================================================================>
 Company:CTS-: 

### What is connection drain
Connection draining is the process of gracefully removing a backend server (like an EC2 instance or a pod) from a load balancer while allowing its existing active connections to complete, instead of terminating them immediately.

It’s commonly used during:
    Deployments
    Auto-scaling
    Rolling updates
    Manual server removal

### Backend.tf file is showing in repo but it is not showing in storage account, what may be the issue
Could be any reason:
1.Terraform is not initialized
2.State is still local not remote 
3.Incorect backend configuration
4. Authentication or Permission Issue

### How to login VM if VM has private IP
If a VM has only a private IP and no public access, I typically use a bastion host (jump server) or VPN to securely access it from a public or management network

Session Manager (For AWS EC2)
In AWS, if SSM Agent is installed and IAM permissions are set, I use SSM Session Manager to connect to private instances without opening SSH ports

### You have deployed a web app and it was working fine but application went down, and all networking are fine and related ports are open, how to troubleshoot.
In this scenario, since networking and ports are verified, I would take a layered troubleshooting approach focusing on application, service, and infrastructure layers to isolate the root cause quickly.
1. Check application logs
First, I check the web application logs for errors such as crashes, memory leaks, unhandled exceptions, or DB connectivity issues.
2. Check Service Status (Web Server / App Process)
Ensure the application process or web server (e.g., Nginx, Apache, Node, Tomcat) is still running.
3. Health Checks & Probes
In Kubernetes, I check liveness/readiness probes:
4. Resource Exhaustion (CPU, Memory, Disk)
### Types of load balancers
1. ALB
2. NLB
3. GWLB

### App is down and throwing 503 err code, what steps should be taken
An HTTP 503 Service Unavailable error usually means the server is temporarily unable to handle the request — often due to the app being unreachable, overloaded, or misconfigured behind a load balancer

========================================================================================================================================================================================================================>
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

### How Docker is operable on a Linux machine? Explain the docker architecture components
###  There are 1 Master & 3 Worker nodes- if the master fails, what happens? Will pods keep running or they will crash?
If master node fails, the existing pods will keep running, they do nort crash immediatley but cluster management stops that being said, no new pods, no scaling, no healinig. 

But the existing workloads continue running because the worker nodes and Kubelets are still alive.

###  In K8s, as etcd is a key-value store db, can write something manually on it?
- you can technically write to etcd manually, it is not recommended
- Use Kubernetes APIs (kubectl, controllers, CRDs, etc.) to update the cluster state. The API server handles validation and then persists the data to etcd safely.

The only time I’ve accessed etcd directly was during disaster recovery or low-level debugging, such as:

    Backing up etcd
    Restoring the cluster from a snapshot
    Checking orphaned or corrupted entries (read-only)

### How to roll back a failed deployment in Docker & K8s?
For Docker, we switched the image tag back and redeployed using Docker Compose.
Kubernetes has built-in support for rollback in Deployments.
    ```bash
    kubectl rollout undo deployment <deployment-name>
```
We used `kubectl rollout undo` to instantly roll back to the previous stable version while logs were investigated.
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

### What are the top 5 infra attacks, and how do you mitigate them?- notAnswered

### In Linux systems there is the term Load Average? what does that mean? how it is being calculated? and in what format the load average output is?
Load Average in Linux shows the average number of runnable processes over 1, 5, and 15 minutes. It's calculated using an exponentially weighted moving average, and helps evaluate whether the CPU is under stress, saturated, or idle.

### One of your worker nodes is not joining the cluster. How would you debug the issue?- notAswered

============================================================================================================================================================================================================================>

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

### What are the provisioners available in Terraform and can you explain the use cases?
    local-exec: Run local commands
    remote-exec: Run commands on remote resource
    file: Copy files to remote resource

### I have created an EC2 instance named A, and I want to create another instance B. It should create an instance without deleting instance A. What can I do during this?

You can create a new EC2 instance B by using a different resource name in Terraform or by creating a new instance in the AWS Console without affecting instance A.

### I have created an EC2 instance through Terraform. I don't have a backup of the Terraform state file, it is not in the remote state and locally not available. Now when I do apply, what can I do?

If the Terraform state file is lost and there's no remote or local copy, Terraform treats the infrastructure as non-existent. So if you run terraform apply, it will attempt to create everything from scratch, which can lead to:

    Resource duplication (e.g., new EC2 instance)
    Drift between actual infrastructure and declared config
In one project, a teammate deleted the local .tfstate and forgot to configure remote S3 state. We recovered the infrastructure by importing all critical resources (EC2, SG, EIP, IAM) and re-established state tracking — without recreating anything.


### What is the difference between a map of objects in Terraform and how can you write an example?-notAnswered
### Suppose in your DevOps team, new team members are added to your team. How can you provide AWS access to your new users, what is the behavior of login to the console?  
When new users join the DevOps team, I follow a secure and role-based approach to provide them least-privilege AWS access using IAM Users or IAM Identity Center (SSO), depending on the organization’s identity strategy.

### What is the difference between an EBS-backed instance and a non-EBS-backed instance?
EBS-Backed Instance
    The root volume is stored on Amazon EBS (Elastic Block Store) — which is a network-attached, persistent storage.
    You can stop, start, reboot, and snapshot the instance without losing data.
    The root volume can persist even after termination (if DeleteOnTermination=false).
    You can resize the root volume or move it to another instance.
✅ Used for: Long-running workloads, persistent applications, and systems requiring backup/recovery.

Non-EBS (Instance Store)–Backed Instance
    The root volume is stored on ephemeral instance store, which is physically attached to the host machine.
    Data is lost when the instance is stopped, terminated, or crashes.
    These instances cannot be stopped, only rebooted or terminated.
    Snapshotting is not supported for instance store root volumes.

✅ Used for: Temporary workloads, buffers, caches, or stateless processing.


### I have 3 nodes (small, medium, and large), and I want only data load to go to the large node. How can I do that?

To ensure that data load pods go only to the large node, First label the node and I would use Kubernetes scheduling rules like node selectors, node affinity, or taints and tolerations. These mechanisms control where pods are scheduled based on node labels and characteristics.


### I am getting the following error, how can I debug that and what does the error mean?
Pods fail to schedule
0/5 nodes are available: insufficient memory.

The error 0/5 nodes are available: insufficient memory means the pod's memory request cannot be satisfied by any node.
    I would:
    Inspect pod memory requests
    Check node memory capacity and usage
    Adjust requests or scale the cluster accordingly

### What is the difference between scaling and autoscaling in Kubernetes?
1. Scaling (Manual Scaling)
Scaling is when you manually increase or decrease the number of pod replicas or nodes based on your own analysis or requirement.
2. Autoscaling (Dynamic Scaling)
Autoscaling is automatic — Kubernetes adjusts resources based on real-time metrics like CPU, memory, or custom metrics.

### If I don't specify TargetPort in the service object, what is it going to do?
If you don’t specify targetPort in a Kubernetes Service object, Kubernetes will default the targetPort to the same value as the port field.

### What are the different types of secrets in Kubernetes?
In my experience, I mostly work with
1. Opaque (default)
    This is the most commonly used type. It stores generic key-value pairs such as:
    API keys
    DB passwords
    Tokens

🔹 2. kubernetes.io/tls
    Used for storing TLS certificates and private keys in PEM format. It’s typically used for Ingress TLS termination or mutual TLS between services.

🔹 3. kubernetes.io/dockerconfigjson
    Stores Docker registry credentials to pull images from private container registries.

### I have an Ingress object that is not routing the traffic to the Kubernetes cluster. What are the reasons and how do you troubleshoot that?
1. Check if the Ingress Controller is Installed and Running
2. Check the Ingress Resource Syntax and Annotations
3. Verify DNS Resolution and External IP
4. Check Backend Service and Endpoints
5. Check Target Pod Readiness Probes
6. Check TLS Configuration (if HTTPS is used)
7. Check Logs of the Ingress Controller

### I have created a service object that is not mapped to a deployment. What could be the reason and how do you debug it?
    If a Service isn’t mapping to a Deployment:
        Check label-selector mismatch
        Inspect endpoints
        Verify ports and namespaces
        Ensure pods are running and ready

### What are the different ways to specify the probes in Kubernetes?
In Kubernetes, probes are used to monitor the health of containers and ensure traffic is only sent to healthy pods. There are three types of probes, and each can be configured in different ways to check the application status.

Probe Type	      Purpose
Liveness Probe	Checks if the container is alive; restarts it if it fails
Readiness Probe	Checks if the container is ready to serve traffic
Startup Probe	Checks if the container is still starting up; useful for slow-starting apps

Kubernetes supports Liveness, Readiness, and Startup probes. Each can be implemented using:

HTTP GET — for web health endpoints

TCP Socket — for checking if a port is open

Exec Command — for custom shell checks



### What is the difference between git push --force-with-lease vs --force?
Both git push --force and git push --force-with-lease are used to forcefully update a remote branch, typically after rewriting history (e.g., rebasing or squashing commits).
The key difference is safety.

`git push --force`
    This command blindly overwrites the remote branch, regardless of whether someone else has pushed changes after your last pull.

    ⚠️ Risk:
    It can overwrite others' work if changes were pushed by teammates after your last pull — destructive in team environments.

git push --force-with-lease ✅ Recommended
    This command is safer. It tells Git:

    "Only force-push if the remote branch hasn't changed since I last fetched it."

    If someone else has pushed to the branch after your last fetch/pull, the push fails.
    Protects you from accidentally deleting someone else’s commits.

### If I select the restart policy as Never, what is it going to do?
If the restart policy is set to Never, Kubernetes will not restart the container in the pod after it exits, regardless of whether it exited successfully or with an error.


### What is an init container and why do we need to use it?
An Init Container in Kubernetes is a special type of container that runs before the main application containers in a Pod. It is used to prepare or set up the environment for the main container and must complete successfully before the main containers start.
They ensure that the environment is ready, dependencies are met, and responsibilities are cleanly separated — making pods more reliable and modular.

### How can you delete the last 2 git commits?

git reset --soft HEAD~2	Deletes commits, keeps changes staged
git reset HEAD~2	    Deletes commits, keeps changes unstaged
git reset --hard HEAD~2	Deletes commits and all changes permanently


================================================================================================================================================================================================================>

### How to use the secrets in kubernetes? What encryption methods do you use?
In Kubernetes, I use Secrets to securely store sensitive data such as API keys, passwords, tokens, TLS certs.I integrate them into workloads via environment variables, volumes, or reference in other resources. 
while ensuring they're encrypted at rest in etcd using AES or KMS, and access-controlled via RBAC. For advanced scenarios, I integrate with external secret managers for rotation and dynamic access.

### How does the GSLB load balancer work?
GSLB (Global Server Load Balancer) is a DNS-based load balancing solution that distributes traffic across multiple geographically distributed data centers or cloud regions. Its main goal is to provide high availability, disaster recovery, and low-latency routing based on various factors.

🌍 How GSLB Works:
    Client makes a DNS request for a service (e.g., app.example.com)
    The GSLB system intercepts the DNS query and decides which data center or region should serve the request
    GSLB returns the IP address of the optimal site/server
    The client connects directly to that location

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

###  How can you create the extensions in Grafana
o create an extension in Grafana, use the plugin development framework to build custom panels, data sources, or apps. Extensions are built using React/TypeScript and can optionally include backend logic in Go. They enhance Grafana’s capabilities for custom visualizations and integrations.

### Can you design the Istio Setup for your k8 cluster?- notAnswered
Yes I can
### AWS event bridge creation and setup via terraform- notAnswered

### What will be the command to add the annotation and the labels for the existing pod?
You can use the kubectl annotate and kubectl label commands to add or modify annotations and labels on an existing pod.
Annotations are used for metadata consumed by tools/controllers (e.g., sidecar.istio.io/inject=false)
Labels are used for selection and grouping (e.g., in Services, Deployments, etc.)

### Design the kubernetes cluster with Ingress.-notAnswered

### A sudden surge in traffic causes a web application to become unresponsive what will be the steps you will take to mitigate
A. Check Health of Components
2. Scale Resources

### Design the deployment of the pod with replica set set as 3 and having apache httpd image running as a container.

### How do you reduce the size of Dockerfile
I reduce Docker image size by:

    Using slim/minimal base images
    Leveraging multi-stage builds
    Cleaning up cache/temp files
    Minimizing layers and build context
    This results in faster builds, smaller attack surface, and efficient deployments.


### Write a shell script to find and delete all files in a directory that are older than 30 days.
### Create a script to monitor the disk usage of a server. If usage exceeds 80%, log the details to a file and send an alert email.
### Write a script that renames all .txt files in a directory by appending the current date to the filename.-not Answered

===========================================================================================================================================================================================================>

### What is the difference between COPY and ADD command in Docker File.
Both COPY and ADD are used in Dockerfiles to copy files from your build context into the image.
The key difference is that ADD offers more features, while COPY is more predictable and preferred for simplicity.

    Use COPY whenever possible because it’s explicit and easier to maintain.
    Use ADD only when you need features like:

        Extracting tar archives
        Downloading from remote URLs (though not recommended — better to use curl or wget)

### What is Taint/Tolerent.
In Kubernetes, Taints and Tolerations work together to control which pods can be scheduled on which nodes.
What is a Taint?
    A taint is a property applied to a node that tells the Kubernetes scheduler:

    "Do NOT schedule pods here unless they tolerate this taint."
What is a Toleration?
    A toleration is a property added to a pod that tells Kubernetes:

    "I’m allowed to run on nodes with this specific taint."

### Use case of Node-Port and Cluster IP service Type in Kubernetes.
n Kubernetes, ClusterIP and NodePort are two common Service types used to expose applications, and they serve different purposes depending on the scope of accessibility.

1.ClusterIP:
    ClusterIP is default servie type which exposes service only inside the kubernetes cluster
    ✅ When to Use:
        Microservices talking to each other (e.g., frontend → backend)
        Internal-only databases, APIs, or message queues
        Backends accessed via Ingress or LoadBalancer

2. NodePort
🔧 Use Case: Expose Application on Static Port on Every Node
    NodePort exposes the service on the same port (e.g., 30080) on every node in the cluster.
When to Use:
    For development, testing, or POC environments where you don’t need a load balancer

### Can we connect two different VM that are in a different Vnet.
Yes, you can connect two VMs in different VNets, but they cannot communicate by default. You need to explicitly configure connectivity between the VNets 

### Private Endpoints.
An endpoint is simply the entry point or destination through which a client communicates with a service, application, or resource over a network.
In cloud and networking contexts, an endpoint typically refers to a URL, IP address, or DNS name that is used to access a specific service.

### What is PDB in Kubernetes.
PDB stands for Pod Distrubtion Budget in Kubernetes.

It is used to control the number of pods that can be voluntarily disrupted at any given time — helping ensure high availability during planned operations like node maintenance, cluster upgrades, or scaling events
Why Use PDB?
Without a PDB, Kubernetes might evict all pods of a Deployment or StatefulSet during a rolling update or node drain.
A PDB protects against too many pods being unavailable at once.
 How It Works
    You define a PDB with either:
    minAvailable: Minimum number of pods that must remain available
    maxUnavailable: Maximum number of pods that can be disrupted

### Difference between PV/PVC in Kubernetes.
In Kubernetes, PV (PersistentVolume) and PVC (PersistentVolumeClaim) work together to provide persistent storage for pods
1. PV – PersistentVolume
    A PV is a pre-provisioned piece of storage in the cluster.
    It represents a physical volume (like an EBS volume, NFS share, or CSI-backed disk) that is managed by the admin or dynamically provisioned.
 2. PVC – PersistentVolumeClaim
    A PVC is a request for storage made by a user or a pod.
    The PVC defines how much storage the pod needs, and what access mode it requires.
    
There are two ways:
   - Static provisioning: Admin manually creates a PersistentVolume object.

   - Dynamic provisioning: Kubernetes automatically provisions a PV using a StorageClass when a PVC is created.

PV is a storage is the cluster, developer creates a PVC which defines how storage, access mode. so kubernetes mactches the PVC to a suitable PV then Pod uses the PVC to mount the volume. finally data is persisted independently of pod, If the pod is deleted or restarted, the data remains intact on the PV

### How you can use the existing Image into the YAML file to deploy a POD.
To use an existing image in a YAML file, define a pod with image: <image-name> in the container spec.
Kubernetes pulls the image and runs the container inside the pod — enabling fast deployment using prebuilt Docker images.

### What is POD in Kubernetes.
A POD is a smallest deployable unit in the cluster where you the container.

### Types of Service in Kubernetes.
1. ClusterIP
2.NodePort
3.LoadBalancer

### Namespaces in Kubernetes.
In Kubernetes, a Namespace is a logical partition within a cluster that allows you to organize, isolate, and manage resources like pods, services, and deployments. It’s especially useful in multi-tenant environments or when you want to separate environments (e.g., dev, test, prod) within the same cluster.
---
### Use of Route53
Amazon Route 53 is used for:

    Registering domains
    Routing internet traffic to AWS resources
    Smart DNS routing (weighted, latency, geo)
    Health checks & failover
    Managing private/internal DNS in VPCs
    It plays a critical role in building highly available, globally distributed systems on AWS.


### Git stash
git stash is a Git command used to temporarily save (stash) changes in your working directory that are not yet committed, so you can switch branches or perform other operations without losing your progress.

### Difference between S3 and EBS
Amazon S3 (Simple Storage Service) and Amazon EBS (Elastic Block Store) are both storage services offered by AWS, but they serve different purposes and are optimized for different access patterns and use cases.

Use Amazon S3 for object-based storage (files, media, backups) that is accessed via API or web.

Use Amazon EBS for block-level storage that behaves like a virtual disk and is mounted to EC2 for low-latency access.

### difference between git fetch and git pull
Both git fetch and git pull are used to update your local repository from a remote, but they behave differently
1. git fetch
 git fetch downloads commits, files, and references from the remote, but it does not merge them into your current branch
2. git pull
    git pull is essentially git fetch + git merge.

    It downloads new commits and immediately tries to merge them into your current branch.

### What is the difference between vertical and horizontal scaling?
The key difference between vertical scaling and horizontal scaling lies in how resources are added to handle increased load or demand.
1. Vertical Scaling (Scale Up/Down)
Involves adding more resources (CPU, RAM, storage) to a single server or instance.
2. Horizontal Scaling (Scale Out/In)
Involves adding more instances/servers to distribute load.
---
AWS Cloud Engineer role -:

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
---
From one of the linkedin post > 
interview experience happened yesterday (08-02-2025). 

### Write a Terraform code to create multiple S3 buckets - notAnswered

### how are you managing the conflict? State file conflicts
    To manage Terraform state file conflicts:
        I use remote backends with state locking
        Follow a single source of truth principle
        Avoid parallel applies, and integrate Terraform with CI/CD pipelines responsibly

### Do you know what is HPA?
Yes I do know, HPA is a Horizontal Pod Autoscaler. It is a Kubernetes resource that automatically scales the number of pod replicas in a Deployment, StatefulSet, or ReplicaSet based on observed CPU utilization or custom metrics.

### suppose you deploy one application okay and you found some issue, you wanted to roll back using the kubernetes how you roll back to the particular version, what is the command?

```bash
   kubectl rollout undo deployment <deployment-name> --to-revision=<number>
```

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

--- 

Company:Oracle  
Experience - 8 years's

### Explain me all the steps for multi stage docker image
1. First Stage: Build Environment
 Use a base image with build tools, like compilers or SDKs.
 ```bash
 # Stage 1: Build stage
FROM node:18-alpine AS builder

WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build
```
2. Second Stage: Runtime Environment
Use a lightweight base image (e.g., alpine, nginx, distroless) to copy only the final output from the builder.
```bash
# Stage 2: Production image
FROM nginx:alpine

COPY --from=builder /app/dist /usr/share/nginx/html

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```
### Cron expression to schedule a job in Linux
In Linux, a cron expression is a 5-field time format used in crontab to schedule recurring jobs.
Each field defines when a command should run: minute, hour, day of month, month, and day of week.
```bash
* * * * * <command>
│ │ │ │ │
│ │ │ │ └─ Day of week (0–7) → Sun/Sat (0 or 7)
│ │ │ └─── Month (1–12)
│ │ └───── Day of month (1–31)
│ └─────── Hour (0–23)
└───────── Minute (0–59)
```

### What are the layer's you will get in Docker while building 
When building a Docker image, each instruction in the Dockerfile creates a new image layer. These layers form a stacked filesystem where each layer represents a filesystem delta — and together, they build the final image.

### Explain me all the components present under deployment.yaml file- notAnswered

---

Company: HCL-: 



### what is calico 
Calico is a CNI plugin and network security engine for Kubernetes that provides high-performance networking, fine-grained network policy enforcement, and scalability across cloud and on-prem environments. It's ideal for teams looking to implement secure, scalable, and observable network infrastructure in Kubernetes.

### how to take bakcup of kubenretes cluster
There are number of ways to backup the kubernetes cluster.
1. Backup Cluster Resources (YAML Manifests)
✅ Manual Backup Using kubectl
2. Backup Persistent Volumes
Use storage-level snapshots (EBS, Azure Disk, GCE PD)
3. Full Cluster Backup with Velero (Recommended)
Velero is a popular open-source tool for backup and recovery of Kubernetes cluster state and volumes.
4. Etcd Backup (For Control Plane/Stateful Clusters)
If you manage a self-hosted control plane, backing up etcd (Kubernetes' internal key-value store) is crucial.

Since I am using AWS EKS the Kubernetes control plane is fully managed by AWS, so you don’t need to back up etcd or the control plane manually.
    Instead, you focus on backing up:
        Cluster configuration (manifests, policies, etc.)
        Application data (persistent volumes)

The most reliable and AWS-integrated way to back up EKS is using Velero with AWS S3 and EBS snapshots.
1. Backup Kubernetes Resources with Velero
    Velero can back up:
    Deployments, Services, ConfigMaps, Secrets, etc.
    Persistent volumes via AWS EBS volume snapshots

✅ Install Velero:
 2. Create a Backup
 ```bash
 velero backup create eks-daily-backup --include-namespaces=default,prod
```
3. Schedule Automatic Backups

On EKS:

    Control plane is managed by AWS — no etcd backup needed
    Use Velero to back up workloads + persistent volumes to S3
    Use IAM roles (IRSA) for secure access
    Optionally schedule backups and restore via Velero CLI or CRDs


### how to upgrade eks clusetr
In Amazon EKS, I upgrade the Kubernetes cluster in two stages: first the control plane, then the data plane (nodes or Fargate). I ensure zero downtime by using managed rolling upgrades, readiness probes, and PodDisruptionBudgets to keep services highly available throughout."

1. Upgrade the Control Plane (Managed by AWS)
    `eksctl upgrade cluster --name <cluster-name>`
  ✅ AWS handles the upgrade with no impact on running workloads
  ✅ Compatible API versions must be ensured first
2. Upgrade Node Groups (Managed Node Groups)
Upgrade worker nodes after the control plane to maintain compatibility.

  Option 1: Rolling upgrade using eksctl
    `eksctl upgrade nodegroup --cluster <name> --name <nodegroup-name> --kubernetes-version <version> --approve`
    - AWS replaces old nodes one by one
    - Uses cordon + drain + replace pattern
  Option 2: Replace with a new node group
      Create a new MNG with updated version/AMI 
      Migrate workloads using labels or taints
      Delete old node group when empty
    ✅ Safer for large or sensitive workloads
- Fargate profiles are automatically upgraded behind the scenes by AWS

### can we run 1 conatiner with 2 pods
No, you can't run 1 container in 2 pods.

### what is ingress controller 
An Ingress Controller is a Kubernetes component that manages external HTTP/HTTPS access to services running inside a cluster. It watches Ingress resources and routes external traffic to the appropriate backend services based on rules like hostnames, paths, or headers.

### Supoose you have taked etcd backup and old vm corrupted ,can we create new vm with backup etcd ?
Yes, you can create a new VM and restore your Kubernetes cluster using the etcd snapshot you previously took.
This is a standard disaster recovery strategy for clusters managed via kubeadm or self-hosted control planes.

### Suppose you have  in satetfull 3 pods which having name mongo-0 , mongo-1, mongo-2 what happen if mongo-0 dies when new pod will create what will be pod new name ?
In a StatefulSet, the pod names are stable and unique, based on the StatefulSet name and an ordinal index. So if mongo-0 dies, Kubernetes will recreate it with the same name — that is, it will come back as mongo-0, not a new name.

### suppose we have pods 2 running in rolling updates some are in deployments set and some pods are in statefull set , how rolling updates strategy will work here  ? 
In Kubernetes, rolling updates are handled differently for Deployments and StatefulSets, because of the nature of pod identity and ordering.

1. Rolling Update Strategy for a Deployment
Deployments manage stateless applications and prioritize speed and availability.
Pods are updated in parallel or batches
No guaranteed order
Can update multiple pods at once based on:
    maxUnavailable
    maxSurge
```bash
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxUnavailable: 1
    maxSurge: 1
```
 2. Rolling Update Strategy for a StatefulSet
StatefulSets manage stateful applications like databases (e.g., MongoDB, Kafka), so updates are more conservative.
Pods are updated one at a time in reverse ordinal order
Pod pod-N is terminated and updated only after pod-(N+1) is Running and Ready
Ensures data safety and service continuity
```bash
updateStrategy:
  type: RollingUpdate
```
--- 
```Company:CTS-:

Also for question 4 is the answer the below?
Check the status of nginx/apache service that was used to host the web app
Check if any firewall/security group rules are blocking the flow of traffic
Ping/telnet won't make sense since networking side is fine
Clearing cache/cookies from browser for stale enteries
Do nslookup for dns resolution if that works fine
Check SSL certificate if it has gone corrupt and is working fine
Lack of memory on the web app server due to which it is failing to load the resources for the app

---

Company:uk ---:

/var partition is 90% full. What’s your immediate action?
You’re locked out via SSH with no root access. How do you recover?
Add 50GB to /opt using LVM without any downtime. What are the steps?
Jenkins is failing to push a Docker image to the registry. How do you troubleshoot?
Ansible playbook times out on one host out of twenty. What do you check?
EC2 instance is unreachable, and it’s not a security group issue. What’s your next step?
An S3 bucket was made public by mistake. How do you secure and audit it?
RDS migration with minimal downtime – how would you approach it?
CI/CD pipeline needs rollback capability. How would you implement it?
Write a shell script that checks if a service is running, restarts it if not, and logs the event.
Terraform script to provision an EC2 instance with a custom security group and user data script.
Design a highly available backend on AWS – what services and architecture would you use?

---
Company:Nisum Technologies:


### what is scrapper?
A scraper is an automated tool that extracts data from websites or APIs, commonly used for monitoring, data mining, or aggregation. It can be extremely useful, but must be used ethically and legally.

### can we deploy services on master node?
Yes! technically we can deploy but not at all recommended and I did not deploy any services on master node.
### what is difference between content and tuple in terraform?
1. content — Used in Dynamic Blocks
The content block is used inside a dynamic block to define what to render for each item in a loop.

2. tuple — A Type System Term
A tuple is a sequence of values with different types and fixed length — similar to a list, but each element may have a different type.

### Any experience on phython/shell scripting? can you explain one file?-notAnswered

### Expalin about fargate?
AWS Fargate is a serverless compute engine for containers that works with Amazon ECS (Elastic Container Service) and Amazon EKS (Elastic Kubernetes Service).
With Fargate, you don’t need to provision or manage EC2 instances — AWS automatically runs containers for you based on CPU and memory requirements.

### what is diffrence between list and string in terraform?
1. string – A Single Text Value
A string is a single sequence of characters
2. list – An Ordered Collection of Values
A list holds multiple values in a specific order
Enclosed in square brackets []

### How to assign memory to pod and how to make sure if pod should not get memory constraint. What to do if it happens.
In Kubernetes, you assign memory to a pod by setting requests and limits for memory in the pod or container spec.

    ```bash
    resources:
    requests:            //Minimum amount of memory the pod needs. Used by the scheduler.
        memory: "256Mi"
    limits:              // Maximum amount of memory the pod can use. If exceeded, it gets OOMKilled.
        memory: "512Mi"
    ```
--- 

Company: Commonwealth Bank
Exp-4-5 Yrs
Role: Principal SRE

1) What is observability architecture? Can you explain it?- notAnswered
2) 
3) What is the difference between observability
and monitoring?
4) When we have logs, why we need trace?
5) How SLA, SLO are set in an application?
Don't give me the formula. Just explain it in from business prospective?
6) How do you decide SLI in an application?
7) Explain metrics, log and trace including all used tools?
8) How observability will help in maintainingside reliability?

========================================================================================================================================================================>
Company:Rapidsoft 
Exp-: 10.5 years work ex. Devops. 4 -5 years of devops experience

### What things have you worked and your experience in brief?- notAnswered
### Terraform has errors while provisioning infrastructure. How to do investigate those? Basically how do you validate the terraform file
1. Validate the Configuration
 ```bash
terraform validate
```
✅ Checks for syntax correctness and basic internal consistency (e.g., missing arguments, wrong blocks, type mismatches).
 2. Format for Readability
 ```bash
 terraform fmt -recursive
```
Ensures your code follows standard formatting, which helps identify misplaced or nested blocks easily.

3. Run a Terraform Plan
```bash 
terraform plan
```
✅ This simulates changes and often surfaces runtime errors,
4. Check for Specific Error Types

This structured approach helps in quickly identifying and resolving Terraform provisioning errors.


### In ansible if you need to execute something as root user how do you that?-notAnswered

### How would you structure disaster recovery for your applciation?-notAnswered

### How would you perform database migration for your database application?-notAnswered

### You have a crashbackloop error. How would you fix this error?-notAnswered


==============================================================================================================================================================================================================>
Company:Epam 
Exp--> 9 years devops -4yrs 


### what is provider?
In Terraform, a provider is a plugin that allows Terraform to interact with APIs of external platforms or services like AWS, Azure, Google Cloud, Kubernetes, GitHub, and many more.
It acts as a bridge between Terraform and the infrastructure you're managing.

### Do you store the data in statefile locally or remotely. What is the block you use while storing the statefile.
I store the statefile remotely, I use  `terraform` and `backend` block to store in s3.

### what is terraform module.
A Terraform module is a reusable, self-contained package of Terraform configurations that manages a specific set of resources. Modules help:

    Organize code
    Reuse infrastructure
    Maintain consistency
    Share infrastructure patterns
    Encapsulate complexity
    Enable versioning

5.how to manage multiple env management in terrfaform?
I manage multiple environments in Terraform using a combination of workspaces, directory structure, and variable overrides to ensure clean separation and controlled deployment across dev, staging, and production.
 1. Directory Structure per Environment (Most Common)
```bash
terraform/
├── modules/
│   └── network/
├── envs/
│   ├── dev/
│   │   ├── main.tf
│   │   ├── vars.tf
│   │   └── terraform.tfvars
│   ├── stage/
│   └── prod/
```
✅ Benefits:
    Full isolation per environment
    Environment-specific configs, variables, and backends

2. Terraform Workspaces (Logical Separation)
```bash 
terraform workspace new dev
terraform workspace new prod
```

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

### what is run and exec command
1. RUN (used in Dockerfile)
The RUN command is used inside a Dockerfile to execute commands during the image build process.

2. docker exec (used on a running container)
The exec command is used in the CLI to run a command inside an already running container.
Used for debugging, inspecting, or running maintenance tasks


### what contains inside vat and opt in linux.

1. /var – Variable Data Directory
/var stands for “variable” and contains data that changes frequently during normal system operation.
```bash
/var/log/	System and application log files
/var/spool/	Queued tasks: print jobs, cron jobs, mail
/var/lib/	Dynamic state info (e.g., package dbs, databases)
/var/cache/	Temporary cached data
/var/tmp/	Temporary files preserved between reboots
```
2. /opt – Optional Software Directory
/opt stands for “optional” and is used for third-party software that’s not part of the default Linux distribution.
```bash
opt/myapp/	A custom or third-party application
/opt/google/chrome/	Chrome installed from a .deb/.rpm file
/opt/<vendor>/<app>	Vendor-specific software
```

### writ a script to search a pattern as 'error' and warning in test.log file. store the pattern with error in one file and  'warning' in another file. pass test.log in the argument-notAnswered
---
Company:NUOS INFO Systems 
Exp--4yrs 
Tech stack— mainly around Terraform, Azure, DevOps, Docker, and Git
---
Company:Netcracker-: 
Exp- 7 Years - DevOps role

### Diff between mount and directories in Linux
A directory is just a folder, part of your existing filesystem.
A mount is the process of attaching a separate filesystem to that directory, making its contents accessible there.

### How will you restart http service from VM
To restart the HTTP service on a Linux VM, I typically use sudo systemctl restart httpd (for Apache) or apache2 (on Ubuntu), depending on the OS.

### Disk I/O
Disk I/O (Input/Output) refers to the read and write operations that occur between the system and the storage device (e.g., hard disk, SSD, or block storage like EBS).
n one case, a PostgreSQL database was slow due to EBS burst credits being exhausted.
iostat showed 100% disk utilization and high await times — we moved to a higher throughput volume (gp3), and performance improved instantly.

###What is meant by CPU throttling
CPU throttling refers to the process of intentionally limiting the CPU usage of a process or system

CPU throttling is the act of limiting CPU speed or usage — either to protect hardware (thermal) or to enforce resource limits (like in containers or VMs).
While it helps with control and stability, excessive throttling can cause application performance issues.

### Diff between Role and Role binding
In Kubernetes, Role and RoleBinding are used together to control who can do what within a namespace (RBAC – Role-Based Access Control).
1. Role – Defines Permissions
A Role is a Kubernetes object that defines a set of allowed actions (like get, list, create, etc.) on specific resources (like pods, secrets, services) within a namespace.

2. RoleBinding – Grants Access
A RoleBinding binds a Role to a user, group, or service account, effectively granting the permissions defined in the Role.




### While updating your worker node, you're trying to perform drain out the PODs  but some PODs are not removed from the node, what you will do
1. Identify Why Pods Are Stuck
3. Check Pod Disruption Budget (PDB)
```bash
kubectl get pdb -A
```
If PDB is blocking eviction (e.g., minAvailable=1 and only 1 pod running), either:
    Scale the app temporarily
    Modify or delete the PDB
4. Force Delete the Pod (Last Resort)
Only do this after confirming the pod can be safely removed (e.g., it's stateless):
5. Use kubectl cordon + Monitor Manually
If drain fails, cordon the node to prevent new pods from scheduling and manually move/scale pods off it.
```bash
kubectl cordon <node-name>
```
### What command will you give for view access for the cluster → READ in rolebinding.yaml file
To grant read-only (view) access to a user, group, or service account in a Kubernetes cluster, you can use the built-in view ClusterRole in a RoleBinding or ClusterRoleBinding, depending on the scope.

### What's the purpose of using storage class in k8s
A StorageClass in Kubernetes defines the "class" or type of storage you want to use for dynamic provisioning of PersistentVolumes (PVs).

1. Enable Dynamic Volume Provisioning
Without a StorageClass, you have to manually create PVs before claims (PVCs) can use them.
    ✅ With a StorageClass:
    A PersistentVolume is automatically created when a user creates a PersistentVolumeClaim (PVC)
    It saves time and avoids manual intervention

In our Kubernetes cluster, we used multiple StorageClasses:
    gp3 for general apps
    io2 for databases
    efs-sc for shared access with ReadWriteMany

Each PVC specified its desired class, and volumes were automatically provisioned based on performance and availability needs.

### I've two PODS in diff worker nodes, can they communicate ?
Yes, by default in Kubernetes, any pod can communicate with any other pod, regardless of whether they are running on the same or different worker nodes, as long as they are:
    In the same cluster
    Not restricted by Network Policies

This is because Kubernetes uses a flat, routable network with pod-to-pod communication enabled across nodes by the CNI plugin (like Calico, Cilium, Flannel).

### How to restrict the communication between them ? 
To restrict that, use NetworkPolicies with supported CNI plugins
Start with a deny-all policy, then explicitly allow required traffic

### What component should be added in network policy YAML file
The key components in a NetworkPolicy YAML are:
    podSelector – selects target pods
    policyTypes – defines traffic direction (Ingress/Egress)
    ingress/egress – rules that allow specific sources/destinations and ports
These components work together to control network access to and from pods.

---
Company:Hexaware 
Exp--4-5yrs 

### .write yaml pipeline for ci/cd (overall structure) 

### what is the command for auto approval in terraform 
 ```bash
 terraform apply -auto-approve
```
What It Does:
Applies the Terraform plan without prompting you to type yes
Useful in automation, CI/CD pipelines, and scripting where manual approval is not possible

 ### what is deployment.yml
 A deployment.yaml file in Kubernetes is a YAML manifest used to define a Deployment resource.
A Deployment manages the declarative deployment, scaling, and updates of a group of replicated pods. It's one of the most common and powerful Kubernetes objects.

What Does deployment.yaml Contain?
It includes the desired configuration for:
    Number of replicas
    Pod template (containers, images, ports, env vars)
    Rolling update strategy
    Labels & selectors
    Optional: Probes, resource limits, volumes, etc.
    
### what is service.yml
 A service.yaml file in Kubernetes is a YAML manifest that defines a Service resource, which provides a stable network endpoint to expose a set of pods.
It enables internal or external access to pods, load balances traffic, and abstracts dynamic pod IPs.

📦 Why Do We Need a Service?
Pods in Kubernetes are ephemeral (can restart or move across nodes)
A Service provides a permanent DNS name and IP to reach those pods
It load balances traffic to all matching pods using label selectors

 ### what is replica set
A ReplicaSet in Kubernetes ensures that a specified number of identical pod replicas are running at all times.
It automatically creates new pods if some fail or get deleted, and removes extra pods if there are too many.

 ### what is the output of helm chart
The output of a Helm chart is a set of rendered Kubernetes resource manifests (YAML files) based on templates and user values.
Helm simplifies deployment by packaging infrastructure and automating configuration management.

 ### how do you set approval in cd pipeline
 Approvals in a CD pipeline are used to pause the pipeline at critical stages (usually before deploying to production), requiring a manual review or sign-off by an authorized user or team.
They help ensure compliance, security, and change control before code goes live.

Jenkins – Input Step in Declarative Pipeline
```bash
stage('Approval') {
  steps {
    input message: 'Deploy to production?', ok: 'Approve'
  }
}
```
 ### what are the branching strategies you use-notAnswered
 ### have you written any automation scripts in your daily tasks-notAnswered
 ### how do you moved the code from one environment to other environment-notAnswered
 
### what are the files available inside helm chart
A Helm chart is a structured directory that contains a set of files and folders used to define, configure, and deploy Kubernetes applications.
These files help Helm template and render Kubernetes manifests dynamically based on user input.

```bash
mychart/
├── Chart.yaml
├── values.yaml
├── templates/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── _helpers.tpl
│   └── ingress.yaml
├── charts/
├── README.md (optional)
└── .helmignore
```
---
Company:Arrise Solutions 
Exp--7 yrs

### Explain me about three tier architecture- notAnswered

### CoreDNS in k8s
CoreDNS is the default DNS server for Kubernetes clusters.
It provides name resolution for services, pods, and external domains within the cluster, allowing applications to communicate with each other using DNS names instead of IP addresses.

Purpose of CoreDNS in Kubernetes
Enables service discovery by translating service names to cluster IPs
Resolves pod DNS names in the format:
pod-name.namespace.svc.cluster.local
Handles external DNS resolution (e.g., google.com)
Supports custom DNS plugins and configuration

### What’s the purpose of CNI
CNI (Container Network Interface) is a specification and plugin system used by Kubernetes to manage container networking.
The purpose of CNI is to ensure that each pod gets a network interface, IP address, and routing rules so that pods can communicate with each other and with the external world.

How CNI Works:
Kubernetes tells the kubelet to create a pod.
Kubelet calls the CNI plugin (e.g., Calico, Flannel, Cilium) to:
Attach a network interface to the pod
Assign an IP address
Configure routing and firewall rules

### How does kube-proxy communicates with nodes
kube-proxy runs on every node in a Kubernetes cluster and is responsible for maintaining network rules to route traffic to the correct pod IPs or services.
It doesn’t need to talk to other nodes directly — instead, it communicates with the Kubernetes API Server and programs routing rules on its local node.

### Diff b/w using VM vs Docker-notAnswered

### Types of storage drivers in Docker

### Does Docker have kernel in place

C name and namespace in docker
### Which network will be used to isolate a communication b/w two containers
### How to check the linux process
### Booting in Linux
###How to check load of linux machine
While rebooting a Linux machine, what are the stages / layers will be restarted
Kernel logs are stored under which directory 
How to kill the running process
States of Linux machine
When you type google.com, what will happen at backend in browser 
When you type TOP command, what are the components will be displayed

I've a client and remote machine, how do certs communicate b/w them ?
How does SSL certs works


================================================================================================================================>
Company:CTS -: 
Exp->3-4yr
How do you manage terrform state file
How would you design an architecture for a 2 tier application 
Difference between subnet and nacl
Difference between nat gateway and internet gateway
How would trigger pipeline B in jenkins automatically after pipeline B
How will you know if a network policy is enabled or not in k8s
Difference between cluster role and cluster role binding
What will happen when a IaC managed resource is modified manually, how would you avoid it
Difference between daemonset and state full set
How would you set up networking in vpc
How you will direct traffic to and from a instance in private subnet


====================================================================
Company: Cisco
Exp--->6+
++++++++++++++++++++++++
1st Round
+++++++++++++++++
 Describe a situation where you had to improve the reliability of a critical system.
 What proactive monitoring solutions have you implemented in your projects?
 Write a playbook to deploy an Nginx server and ensure the service is started and enabled on boot. How would you manage secrets in Ansible? 
 How would you migrate a Terraform backend from local to a remote backend like S3 with DynamoDB locking?
 What happens if the Terraform state becomes corrupted, and how would you recover from it?
 Write Terraform code to provision an EC2 instance with a security group allowing only SSH access. 
 Explain how you would set up a multi-branch Jenkins pipeline for a GitHub repository.
 How would you implement dynamic stages in a Jenkinsfile based on environment variables? 
 Explain the upgrade process for a Kubernetes cluster with zero downtime.
 What key things should you verify post-upgrade? 
 Write a script to monitor a directory and automatically copy any new files to a remote server using SCP.
++++++++++++++++++
2nd Round-:
++++++++++++++++++
 write a playbook to install apache in VM?
 How do you update the statefile from local to S3 bucket,what will you do if it gets lost.
 Terraform scripts for creating AWS services Jenkinsfile EKS and On Prem Kubernetes cluster upgrade steps.
 Write a shell script where you have one virtual machine ubuntu1, auto ssh enabled, ssh -i for private key, directory path /nobackup to be copied in another VM. 
 Jenkins pipeline setup Kubernetes If a pod is getting restarted constantly, what steps are you going to follow? 
 For deployment if you get timeout issue,  what kind of api gateway you used? 
 And how did you managed security for application level? 
 How to secure public api for on prem setup? 
 Where and How to check application performance metrics

========================================================================
Company: Deloitte
Exp-:

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
Can we create AWS backup using Shell Scripting?
Once the backup is created, where will you store the log files?


=================================================
Company -Amazon 
Exp-9years(Devops Consulatnt)

Sending log files from EC2 to S3, what are the steps ?
Limiting the resource usage in k8s not through deployment.yam → Through namespace
Three tier architecture
Updating worker nodes in k8s
I’m an admin but I don’t have access to the S3 bucket ? → IAM permission boundary
Various stages of CI/CD
How will you build the image during CI and how will you manage it ?
You have an S3 bucket at us-south-1, is it possible to access that bucket from us-east-1 ?
Write a Terraform code to create VPC, subnet, EC2, S3 bucket.
Purpose of using CNI in K8s
I need to send a logs from EC2 to S3, create an automation part where we need to take the log file, check the CPU metrics and send an alarm through cloud watch to user's.
Is it possible to create NAT gateway in private subnet ?
How to configure Cluster auto-scaling, how to do that


=================================================================================================================================================================================
Company - IBM
Exp-7years (Devops Enginner)

Encrypting the EBS volume 
A Jenkins pipeline is randomly failing at the deployment stage to EKS. Logs show timeouts during kubectl apply.
How do you securely manage TF state files, secrets, and environment isolation?
How to design an event-driven architecture using S3, Lambda, and SNS for data ingestion
How will you create HPA
If I've three Master Node, one is down, what will happen
Global LB in Kube
AB Testing
How will you check the vulnerability of your code
If there are two clusters running in diff regions, if there is an issue with one cluster, how to shift the traffic to another cluster.
In Log file, how to fetch 200 status code
If you type kubectl get pods, what will happen in the backend.
How to check the kubectl logs of a POD before it restarted
Reverse proxy
How will you resolve the git conflict automatically?

=======================================
Company -F5 
 Exp--(Associate Consultant )

What is Web Application Firewall,
How would you secure the web app running in cloud from oswsap10 attacks,
What happens if tfstate file gets deleted,
What is terraform lock hcl file,
What are best practices to be followed on terraform,
If we have security group configured in the instance do we really need nacl.,
Difference between Transit gateway and VPC,
Best practices to be followed for cloud security.,
HTTP request header and HTTP Methods,
10.If thre is an instance we have security group and web application firewall enabled, DDOS attack enabled will it protect from Bot attack.
There is db I made same entry(Name, Location) through PUT method twice what will happen.,
Why is PUT request called idempotency in nature. If I made another entry and name is same but location is differnet then what will db store.,
What happens if I type www.google.com in the background.
What is SSL/TLS Handshake.
15.What is DNS Resolution. Suppose I have new system with no cache what happens in the background.Step by step process.
What is K8. Explain the architecture.,
Can I run POD inside master-node itself?,
Have you deployed any security application on Kubernetes?


==========================================================================================================================================================================================
Company -Amazon 
 Exp--7 years ( DevOps Consultant) 

If you’re migrating a monolithic application from on-prem to Cloud and the system has its local file system, which file system you will use in AWS.,
How will you store all the configurations related to your monolithic app in Cloud,
What are the observibility needed for app —> Monitoring, Alerting, Logging, Remediation, PD,
If you’re not allowed to install Filebeat in your worker nodes for logging, then what will be possible option,
What are the security protocols will be taken into consideration while designing three tier architecture.
DB migration, how will you sync the data,
If DB POD is down, will it affect the data it gets stored,
How about the sticky session data if POD gets down → Yes the session data will be lost,
What service to use sticky session as an alternative option → redis,
During a sticky session problem what will be the cause for LB.,
Do you create clusters in multi regions, is it possible? If yes, then how will you manage them,
Onboarded trading app into AWS, how will you make sure availability, scalability, security,
How will you take a back of your entire cluster regulary ?
Write a python script to list the EC2 instances running in your cloud which has the tag of PROD,
You have been tasked to create 20 EC2 per account and you been provided with 10 AWS accounts, so totally you need to create 200 EC2 machines, how will connect all these machines. Which service will be used?,
You need to connect your DB running in private subnet, not using NAT gateway or NAT instance or bastion host, what are the other options,
Explain about OSI model,
Diff between directory & mount,
Diff between local and variable in Terraform,
You created couple of resources using Terraform, how will you make sure that resources are not modified through UI, how will you automate this check

===============================================================================================================================================================================================================================
Company -CMT
Exp-  4-5 yrs(SRE)
which are all the services you used in AWS
design an high availability, fault tolerance system in aws
what is dns
what is tcp and udp
what is ipv4 and ipv6
what is pid
how will you troubleshoot if a system goes down in Linux - tell the commands
which are the production incidents you had attended , pls explain
architecture of Kubernetes 
terraform structure
how would you maintain high availability in ecs + fargate or eks 
what is the difference between alb and nlb, in which scenario you use alb and nlb
in webserver/app server which metrics is used to monitor the high availability
did you use any automation in your daily work
what metrics is used to monitor ec2 instance cpu, memory in aws
if there is slowness issue in decouple (SQS), how would you handle it 
what are the best practices can be used to keep the systems highly available

===================================================================================================================>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

Company-- TCS
Exp-- 4-5 yrs (DEVOPS)
1.what is role in ansible 
2.how do you encrypt in ansible 
3.what is idempotent in ansible 
4.what is module in ansible 
5.what is libraries in python 
6.what are deployment group in azure devops
7.how wl you set approvals in pipeline 
8.difference between microsoft hosted agent and self hosted agent 
9.what is the difference between monolithic and microservices 
10.explain did u done any automation in your project 
11.pls explain the flow how the pipeline will trigger across different environments 
12.if developer is working on a code, what are all the next steps he has to do for running the pipeline along wd devops engineer 
13.what are all the deployment startgies you use in deployments in k8s , explain canary and blue green strategies 
14.what is devsecops  , did you use any tools for scanning image etc 
15.what is the difference between classic pipeline and yml
16.tell me pipeline steps for angular or java or .net 
