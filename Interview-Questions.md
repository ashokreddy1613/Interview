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
To enable passwordless SSH authentication between two Linux servers, I use SSH key-based authentication. This avoids manual password entry and is commonly used for automation, CI/CD, and secure server-to-server communication.

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

9.explain about blue green deployment strategy 
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

1) What will happen if the k8 master node and worker node firewall gets broken? Will the existing deployments work or impact on any new deploymentsHow will you communicate to people
2)How to use the secrets in kubernetes? What encryption methods do you use?
3) How does the GSLB load balancer work?
4) What is SLI, SLO, SLA
5) How can you create the extensions in Grafana
6) How does the ELK setup has been done and what type of agents have you collected
7) Can you tell one scenario where you have done the RCA wrt linux
8) You have the JSON data could you please let me know how would you ingest and collect the data in the keys format.
9) Can you design the Istio Setup for your k8 cluster?
10) AWS event bridge creation and setup via terraform
11) What will be the command to add the annotation and the labels for the existing pod?
12) Design the kubernetes cluster with Ingress.
13) A sudden surge in traffic causes a web application to become unresponsive what will be the steps you will take to mitigate
14) Design the deployment of the pod with replica set set as 3 and having apache httpd image running as a container.
15) How do you reduce the size of Dockerfile
Write a shell script to find and delete all files in a directory that are older than 30 days.
Create a script to monitor the disk usage of a server. If usage exceeds 80%, log the details to a file and send an alert email.
Write a script that renames all .txt files in a directory by appending the current date to the filename.

===========================================================================================================================================================================================================>
1)What is the difference between NSG and Firewall.
2)What is the difference between COPY and ADD command in Docker File.
3)What is Taint/Tolerent.
4)What is stateful set.
5)Architecture of Kubernetes.
6)Use case of Node-Port and Cluster IP service Type in Kubernetes.
7)Explain GITHUB Action workflow file.
8)Difference between entry point and CMD in Docker File.
9)Can we connect two different VM that are in a different Vnet.
10)Private Endpoints.
11)Express route in Azure Cloud.
12)What is PDB in Kubernetes.
13)Difference between PV/PVC in Kubernetes.
14)What is state file in Terraform.
15)What is lock file in Terraform.
16)Why Kube-let and Kube-proxy is used for in Kubernetes.
17)How you can build an Image and push it to the ACR.
18)How you can use the existing Image into the YAML file to deploy a POD.
19)What is POD in Kubernetes.
20)Types of Service in Kubernetes.
21)Namespaces in Kubernetes.

==============================================================================================================================================================================================================>
Introduction
Whats ur organisation current cicd process and tools
What do u know about Cyberark and what and how u r consuming that in ur pipeline
Which scriting language u r aware? and how much confident on that?
What are the services u were used in AWS
If U want to design a infra for high scalablity, how did u do that?
What are NACLs,SecurityGroups,NAT Gateway
About Kubernetes architecture
diff b/w Replicaset and Deployment
About Ansible,Terraform
About ConfigMaps,PV,PVCs
write a Deployment file
Write a Docker File
About multistage Docker file
Which type of Jenkins File u r using? Can u pls Write a Jenkins File?
What is the toughest situation u r faced while implementing anything and what did u learnt from that?
U handled any debug/troubleshoot for kubernetes?
About Prometheus/Grafana
About Fargate
What is lambda functions? did u used any Lambda functions? what did u acheived from that?

===================================================================================================================================================================================>
Company:NatWest Group -: 

about Maven release
about maven lifecycle
about dependency management tag
how did u manage Kubernetes pods it is on Linux right?
what will happen with maven install
what are the types of branching stratergy u r using
in which directory or in which place ur pom.xml there 

=================================================================================================================================================================================>
Company:Wikreate Media -:

Use of Route53
Git stash
Difference between S3 and EBS
difference between git fetch and git pull
What is the difference between vertical and horizontal scaling?
What is the role of continuous integration?
=======================================================================================================================================================================================>
AWS Cloud Engineer role -:

Explain OSI models
How do you did cost optimization in AWS?
Explain on Lambda, CFT, Data Storage, S3 ?
How do you implement best security policies on AWS?
What factor motivates you for this role?
Explain how you did your cloud migration

=================================================================================================================================================================================================>
SRE role -:

Commands used for Kubernetes,Docker and Ansible..
Explain Kubernetes Structure, Config Map and Schedular..
3.How do you troubleshoot Imagepull backoff error ?
Explain installation of prometheus and grafana..
Explain how you build CICD pipeline..
How we monitor cpu matrics in grafana?
How do you grant access to user?

=======================================================================================================================================================================================================>
From one of the linkedin post > 
interview experience happened yesterday (08-02-2025). 

Write a Terraform code to create multiple S3 buckets
How you managed statefile
So how are you managing the conflict? State file conflicts
what you did with Jenkins?
How are you integrating the SonarQube with the Jenkins server?
How were you authenticating Jenkins to push docker image to registery?
Have you worked on the Kubernetes?So what deployment strategy are you following?
So how are you implementing the blue green deployment?
Do you know what is HPA?
suppose you deploy one application okay and you found some issue, you wanted to roll back using the kubernetes how you roll back to the particular version, what is the command?
What is the stateful set in the Kubernetes?
Have you worked on the AWS, right?
So how many types of policy, IAM policy are there? IAM policies?
So what is the difference between the S3 bucket policies and acls?
what is the dynamic auto scaling?
What is the difference between Security groups and NACL?
Two AWS accounts are there in same organisation. Account A has Ec2 instance and Account B has some tokens.  Need to access the tokens from Ec2 instance.
How can achieve the requirement?

=============================================================================================================================================================================>
Company:Oracle  
Experience - 8 years's

What's the purpose of using init containers in K8s
Stateful vs deployment in k8s
Configmap VS secrets 
Pod Distribution budget in k8s
Explain me all the steps for multi stage docker image
Cron expression to schedule a job in Linux
What are the layer's you will get in Docker while building 
When you're trying to deploy a POD, it's  throwing an error, how will you investigate.
How to deploy a POD into a certain NODE?
Explain me all the components present under deployment.yaml file
If TF statefile is corrupted, how to fix it ?
TF Lifecycle (create / after destroy) 

==========================================================================================================================================================================================================>
Company: TCS-: 
Exp -9 yrs devops rel-4yrs

What is jfrog artifactory 
what is ansible tower 
what is nagios , how to integerate jenknins in nagios
what is ansible tower 
what is ansible roles 
what is jinja2 template 
what is jfrog xray 
what is jfrog uses cases 
how to find the mount point space of linux
what are the ansible modules you have used 
diff between github repo and jfrog 
what is branching stargery 
what is differnet type repo in jfrog 

===============================================================================================================================================================================================================>
Company: HCL-: 

what is diff between pv and pvc
Expalin kuberenets arthciecture 
what is diff between deployment and statefullset 
what is calico 
what is etcd
how to take bakcup of kubenretes cluster 
how to upgrade eks clusetr
what is rolling update 
what is deployment statgergy you are using 
can we run 1 conatiner with 2 pods
what is statefullset 
terraform have you build the vm 
what is ingress controller 
Supoose you have taked etcd backup and old vm corrupted ,can we create new vm with backup etcd ?
what are steps to upgrade eks cluster 
Suppose you have  in satetfull 3 pods which having name mongo-0 , mongo-1, mongo-2 what happen if mongo-0 dies when new pod will create what will be pod new name ?
have you worked on argo cd , helm 
suppose we have pods 2 running in rolling updates some are in deployments set and some pods are in statefull set , how rolling updates strategy will work here  ? 
what is docker multisatge , why it is used and all 
grafana have you worked 

=================================================================================================================================================================================================>
Company:CTS-:

Also for question 4 is the answer the below?
Check the status of nginx/apache service that was used to host the web app
Check if any firewall/security group rules are blocking the flow of traffic
Ping/telnet won't make sense since networking side is fine
Clearing cache/cookies from browser for stale enteries
Do nslookup for dns resolution if that works fine
Check SSL certificate if it has gone corrupt and is working fine
Lack of memory on the web app server due to which it is failing to load the resources for the app

======================================================================================================================================================================================>
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


====================================================================================================================================================================================>
Company:Emphasis-:

I attended interview for devops role, they were asking very basic questions about devops & aws.
Explain components in 3-tier architecture
Explain Kubernetes architecture
How private subnet connect with outside world
What is difference between NACL & security groups
What is the purpose of NAT gateway
Explain how to write a dockerfile
From where is the image pulled when you use docker pull image?
How is the image pulled from private repository
What is ingress in Kubernetes
Explain CI/CD pipeline and its stages

==============================================================================================================>
Devops interview questions
What is docker file what is inside it 
Why K8 instead of docker swam
Architecture of K8
What is blue green deployment explain a project based on it 
Why canary and blue green differ 
What happens if etcd stops working 
What are the types of services 
Explain a project in which u used Docker K8 and CICD
What ci/cd do u use tell us about it in detail 
What wil happen if the docker image has port 8080 and container/application has some port
Why is load balancer used
What iare ur git branching strategies 
terraform state file locking 
service vs deployment
linux commands 
ansible file in writen 
CI-CD pipeline in details
jenkinsfile stages
VPC 

=================================================================================================================>
Company:Nisum Technologies:

explain CI/CD pipeline
what is scrapper?
can we deploy services on master node?
Did you upgraded any services?
what is deployment stratergy that you are following?
what is differnece between COPY and ADD commands ?
How do you fix security issues in Docker images?
what is difference between content and tuple in terraform?
Any experience on phython/shell scripting? can you explain one file?
Any expercience on ansible?
Expalin about fargate?
If you pod is not running, how do you troubleshoot it?
what is diffrence between list and string in terraform?
Did you worked on helm charts?
 

============================================================================================================================================================================================================>
Company:Capegemini 
Exp- 6+ yrs overall experience (considering 3 yrs in devops)

How to assign memory to pod and how to make sure if pod should not get memory constraint. What to do if it happens.
How to pass variable in azure pipeline? how to parameterize pipeline
What is availability zone and explain the layout on ground level.(in depth)
How mysql will interact with azure key vault and it should happen thru privately and should not go anything on public
Diff between git fetch and git pull (what happens in background in depth)
What happens behind the hood when git add command is provided. File is added but what happens in the background how git knows on this command it has to add file. ( in dept related to git database)

==================================================================================================================================================================================================================>
Company: Commonwealth Bank
Exp-4-5 Yrs
Role: Principal SRE

1) What is observability architecture? Can you explain it?
2) What is DNS? When you type google.com. What exactly happening in the background?
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

What things have you worked and your experience in brief?
Terraform has errors while provisioning infrastructure. How to do investigate those? Basically how do you validate the terraform file
In ansible if you need to execute something as root user how do you that?
Your CI/CD pipeline has failed in jenkins. How do you investigate?
How do you store sensitive information like passwords in jenkins?
You have a multi-cloud environment. How do you manage pipelines for all those cloud environments?
How would you structure disaster recovery for your applciation?
How would you perform database migration for your database application?
You have a crashbackloop error. How would you fix this error?
Difference between deployment and stateful sets?
Explain terms in deployment.yml file in kubernetes
How you worked on docker swarm before? 

==============================================================================================================================================================================================================>
Company:Epam 
Exp--> 9 years devops -4yrs 


1.How to Create and Use Custom Resources in Kubernetes
what are name spaces in k8s
what is difference between deployment and statefulset
4.what is role based control access
5.what is cluster auto scaler and horizontal auto scaling

Terraform:
1.what is provider?
2.how to manage state in terraform
3.Do you store the data in statefile locally or remotely. What is the block you use while storing the statefile.
4.what is terraform module.
5.how to manage multiple env management in terrfaform
6.what is cloud watch uses cases.
7.what is the ECS and eks 
8.what is fargate
9.what is limitations of lamda
10.how lamda works containers 
11.what is ec2 instances
12.direct connect in aws
12.storage gateway in AWS
13.VPC, NAT gateway,s3, route53, vpc peering,transit gateway, autoscaling group
Difference between sG and NACL
14.wht is the difference beteen copy and add
difference between cmd and entrypoint
16.what is run and exec command
17.what contains inside vat and opt in linux.
writ a script to search a pattern as 'error' and warning in test.log file. store the pattern with error in one file and  'warning' in another file. pass test.log in the argument

=======================================================================================================================================================================================================================>
Company:NUOS INFO Systems 
Exp--4yrs 
Tech stack— mainly around Terraform, Azure, DevOps, Docker, and Git


How do you scale a Terraform pipeline that takes 25+ mins?
What happens to the Terraform state file if someone deletes resources from Azure?
If the pipeline fails due to existing resources, how do you handle RIP (Remove, Import, Plan)?
How do you export Azure resources into Terraform code?
How do you enforce Azure Policies (like tag or location restrictions) using Terraform at scale?
Best practices to structure repos and pipelines in a large DevOps project?
Pipeline fails only on Tuesdays, no code changes — how do you debug?
Logs are incomplete — how would you troubleshoot across AKS, Ingress, App, and Infra?
How to monitor Azure VM memory and alert if it crosses 80%?
How to write a multistage Dockerfile for a Node.js app — removing secrets and unnecessary layers?
Recommended tools for CI/CD, artifact storage, vulnerability scanning, and container registry in a hybrid (on-prem + Azure) setup?
How do you assess Azure DevOps migration readiness and plan the transition?
How do you manage AWS + Azure using a single DevOps process with focus on security & cost?
How would you use Azure DevOps REST API to apply a security policy to all repos programmatically?
What’s the difference between Git Merge and Rebase?
If someone force-pushed and lost the main branch, how do you recover it?
How to push the recovered branch back to remote?

====================================================================================================================================================================>>
Company:Netcracker-: 
Exp- 7 Years - DevOps role

Diff between mount and directories in Linux
How will you restart http service from VM
Disk I/O
What is meant by CPU throttling
Custom resource in k8s
What is ingress
Application is configured with Ingress but the webpage is not loading ? What are the steps will be checked
How will you monitor the cluster through Prometheus
Upgrading the worker nodes in K8s
For junior team member, what are the roles will be provided in k8s
Diff between Role and Role binding
If I want to deploy my app in worker node2, what should I do ?
Diff between Nodeselector, node affinity VS Taint, toleration
While updating your worker node, you're trying to perform drain out the PODs  but some PODs are not removed from the node, what you will do
What command will you give for view access for the cluster → READ in rolebinding.yaml file
Storage classes in k8s
NFS
What's the purpose of using storage class in k8s
I've two PODS in the same worker node, will they communicate with each other?
I've two PODS in diff worker nodes, can they communicate ?
How to restrict the communication between them ? → network policy
What component should be added in network policy YAML file

================================================================================================================================================================>
Company:Zensar 
Exp-:6+ years

What is service connection/connection string
How to set alerts in azure monitor (explain steps and configuration that u do)
Types of vnet peering
What is DR
What types of pipeline you use. How many types of pipeline are there
What is variable in pipeline
If we have to run multiple jobs parallely using a single pipeline, can it be done? How?
What is agent?
Diff between replica set and deployment
Diff between stateless and stateful application
How configmap and secrets can be used in k8s
If control plane goes down what will happen to worker plane?
What is etcd and its use
How to create app registration
What is docker networking
Why k8s is need if docker volume is there? (Dont remember exact Q framing but sounded like this)
If 2 pod are in diff namespace then how can we make them communicate to each other securely?
What is ingress
In terms of Cost optimisation which one should we use, Az App gateway or network gateway?


================================================================================================================================================>
Company:CTS -: 

What is connection drain
Backend.tf file is showing in repo but it is not showing in storage account, what may be the issue
How to login VM if VM has private IP
You have deployed a web app and it was working fine but application went down, and all networking are fine and related ports are open, how to troubleshoot
Types of load balancers
What is Az App Gateway and how it encrypt http/https traffic
App is down and throwing 503 err code, what steps should be taken


===================================================================================================================================================================>
Company:Hexaware 
Exp--4-5yrs 

 1.write yaml pipeline for ci/cd (overall structure) 
 2.what is the command for auto approval in terraform 
 3.what is deployment.yml
 4.what is service.yml
 5.what is replica set
 5.what is the output of helm chart
 6.what is storage explorer
 7.how do you set approval in cd pipeline
 8.what are the branching strategies you use 
 9.have you written any automation scripts in your daily tasks 
 10.how do you moved the code from one environment to other environment 
 11.what is variable group in azure devops
 12.what is sonarqube and which purpose it is used for 
 13. Write sample terraform code (overall skeleton)
 14.what are the files available inside helm chart


======================================================================================================================================================================================>
Company:Arrise Solutions 
Exp--7 yrs

Explain me about three tier architecture 
Diff b/w ALB and NLB in depth 
How to connect a VPC in AWS to VPC in IBM Cloud
Diff b/w Public and Private subnet 
How to connect your private subnet with Internet 
Does NAT gateway will run in public or private subnet
K8s architecture 
CoreDNS in k8s
What’s the purpose of CNI
How does kube-proxy communicates with nodes
Purpose of scheduler in k8s
Layers in Docker
Diff b/w using VM vs Docker
Types of storage drivers in Docker
Types of networking in Docker and explain in detail
Does Docker have kernel in place
C name and namespace in docker
Which network will be used to isolate a communication b/w two containers
How to check the linux process
Booting in Linux
How to check load of linux machine
While rebooting a Linux machine, what are the stages / layers will be restarted
Kernel logs are stored under which directory 
How to kill the running process
States of Linux machine
When you type google.com, what will happen at backend in browser 
When you type TOP command, what are the components will be displayed
CloudFront 
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
What happens if I type www.google.com in the background.,
What is SSL/TLS Handshake.,
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
What are the security protocols will be taken into consideration while designing three tier architecture.,
DB migration, how will you sync the data,
If DB POD is down, will it affect the data it gets stored,
How about the sticky session data if POD gets down → Yes the session data will be lost,
What service to use sticky session as an alternative option → redis,
During a sticky session problem what will be the cause for LB.,
Do you create clusters in multi regions, is it possible? If yes, then how will you manage them,
Onboarded trading app into AWS, how will you make sure availability, scalability, security,
How will you take a back of your entire cluster regulary ?,
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
