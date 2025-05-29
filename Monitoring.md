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

### did u configure Prometheus and Grafana?
Yes, we have setup monitoring across our organization using monitoring with prometheus and grafana including for microservices applications.

### explain about Prometheus and grafana
Prometheus and Grafana are two of the most widely used open-source tools for monitoring and observability in cloud-native, containerized, and DevOps environments.

What is Prometheus?
Prometheus is a metrics-based monitoring system that collects and stores time-series data (numeric metrics over time).

What is Grafana?
Grafana is a visualization and dashboard tool used to display data from Prometheus and other sources like InfluxDB, MySQL, CloudWatch, Elasticsearch, etc.

### What is Kibana 
Kibana is an open-source data visualization and exploration tool that's used primarily with Elasticsearch. It provides a web-based interface to search, view, analyze, and visualize log and time-series data stored in an Elasticsearch index.

### How does Prometheus collect metrics
Prometheus pulls metrics from targets at defined intervals, using an HTTP GET to the /metrics endpoint. These metrics are stored in its time-series database and can be queried using PromQL or visualized with Grafana.

### What are data sources for Grafana, Kibana
Grafana supports multiple data sources, primarily used for metrics and time-series data. 
Prometheus,Loki, CloudWatch,MySQL / PostgreSQL.

Kibana is tightly integrated with Elasticsearch and is used primarily for log analytics and full-text search.

### How do you receive alerts in your project and how is it setup
In our project, we have a centralized monitoring and alerting setup using Prometheus + Alertmanager + Grafana, and in some cases, CloudWatch Alarms for AWS services. The alerts are integrated with collaboration tools like Slack, Microsoft Teams, or email to ensure quick response from the team.

### How do you handle disk, CPU alerts 
I handle CPU and disk alerts by combining real-time metrics collection, smart alerting rules, and automated or manual remediation steps. The key is to act before users are impacted and to prevent recurring issues through automation and scaling strategies.

###  How can you create the extensions in Grafana
o create an extension in Grafana, use the plugin development framework to build custom panels, data sources, or apps. Extensions are built using React/TypeScript and can optionally include backend logic in Go. They enhance Grafana’s capabilities for custom visualizations and integrations.

1) What is observability architecture? Can you explain it?- notAnswered
2) 
3) What is the difference between observability
and monitoring?
4) When we have logs, why we need trace?

what is prometheus, grafana, promrtheuis setup,  monitoring etc 