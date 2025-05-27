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


 ### have you written any automation scripts in your daily tasks-notAnswered
 ### how do you moved the code from one environment to other environment-notAnswered
### How would you structure disaster recovery for your applciation?-notAnswered
What is DNS? When you type google.com. What exactly happening in the background?
Also for question 4 is the answer the below?
Check the status of nginx/apache service that was used to host the web app
Check if any firewall/security group rules are blocking the flow of traffic
Ping/telnet won't make sense since networking side is fine
Clearing cache/cookies from browser for stale enteries
Do nslookup for dns resolution if that works fine
Check SSL certificate if it has gone corrupt and is working fine
Lack of memory on the web app server due to which it is failing to load the resources for the app

/var partition is 90% full. What’s your immediate action?
You’re locked out via SSH with no root access. How do you recover?
Add 50GB to /opt using LVM without any downtime. What are the steps?
I've a client and remote machine, how do certs communicate b/w them ?
How does SSL certs works
Describe a situation where you had to improve the reliability of a critical system.
What is Web Application Firewall
What is SSL/TLS Handshake.
which are the production incidents you had attended , pls explain