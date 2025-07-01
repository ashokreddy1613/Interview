# Monitoring and Observability Guide

## 1. Monitoring Tools and Setup

### 1.1 Prometheus and Grafana Configuration
#### 1.1.1 What is Prometheus?
- Prometheus is a metrics-based monitoring system
- Collects and stores time-series data (numeric metrics over time)
- Pulls metrics from targets at defined intervals using HTTP GET to /metrics endpoint
- Metrics are stored in time-series database
- Can be queried using PromQL or visualized with Grafana

#### 1.1.2 What is Grafana?
- Visualization and dashboard tool
- Displays data from multiple sources:
  - Prometheus
  - InfluxDB
  - MySQL
  - CloudWatch
  - Elasticsearch

#### 1.1.3 Grafana Dashboard Configuration
1. Connect a Data Source
   - Go to Configuration → Data Sources
   - Add supported sources (e.g., Prometheus for DevOps/K8s)

2. Create a Dashboard
   - Go to + → Dashboard → Add new panel
   - Choose data source (e.g., Prometheus)

3. Customize Panels
   - Choose panel type: Graph, Gauge, Table, Stat, Heatmap

4. Add Alerts (if needed)

### 1.2 Kibana
- Open-source data visualization and exploration tool
- Primarily used with Elasticsearch
- Provides web-based interface for:
  - Searching
  - Viewing
  - Analyzing
  - Visualizing log and time-series data

## 2. Data Sources and Integration

### 2.1 Grafana Data Sources
- Prometheus
- Loki
- CloudWatch
- MySQL/PostgreSQL

### 2.2 Kibana Data Sources
- Tightly integrated with Elasticsearch
- Used for log analytics and full-text search

## 3. Alerting and Monitoring

### 3.1 Alert Setup and Management
- Centralized monitoring using:
  - Prometheus + Alertmanager + Grafana
  - CloudWatch Alarms (for AWS services)
- Integration with collaboration tools:
  - Slack
  - Microsoft Teams
  - Email

### 3.2 Resource Monitoring
#### 3.2.1 CPU and Disk Alert Handling
- Real-time metrics collection
- Smart alerting rules
- Automated or manual remediation steps
- Proactive action before user impact
- Prevention through automation and scaling

### 3.3 Grafana Extensions
- Created using plugin development framework
- Built with React/TypeScript
- Optional backend logic in Go
- Enhances capabilities for:
  - Custom visualizations
  - Integrations

## 4. Implementation Example
In a recent Kubernetes setup:
- Created Grafana dashboard using Prometheus data
- Monitored:
  - Node CPU
  - Pod memory usage
  - Container restarts
- Added namespace variable for service filtering
- Set up Slack alerts for high CPU usage via Webhooks

## 5. Questions to Address
1. What is observability architecture?
2. What is the difference between observability and monitoring?
3. When we have logs, why do we need trace?
4. What is Prometheus setup and monitoring?

### did u configure Prometheus and Grafana?
Yes, we have setup monitoring across our organization using monitoring with prometheus and grafana including for microservices applications.

### explain about Prometheus and grafana
Prometheus and Grafana are two of the most widely used open-source tools for monitoring and observability in cloud-native, containerized, and DevOps environments.

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
o create an extension in Grafana, use the plugin development framework to build custom panels, data sources, or apps. Extensions are built using React/TypeScript and can optionally include backend logic in Go. They enhance Grafana's capabilities for custom visualizations and integrations.
