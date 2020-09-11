# Prometheus and Grafana configure in Azure Kubernetes (AKS)

Prometheus and Grafana is open source monitoring tools for Kubernetes clusters. It allows User and developers to capture metrics and telemetry data for applications running inside the cluster, allowing deeper insights into application performance and reliability at container level.

### Prometheus

Prometheus stores all The data as a time series. This data can be queried via the PromQL query language and visualized with a built-in expression browser. Prometheus does not Provide dashboard, it relies on Grafana for visualizing data. it is one of the most used Kubernetes monitoring tools. Other tools from the Kubernetes ecosystem, including Istio, include a built-in Prometheus adapter that exposes generated metrics.

### Grafana

Grafana is used to visualize any data. It can be used to create multiple dashboards that support a variety of beautiful graphs and visualizations for different users and stakeholders. It has support for integration different type of data sources:
* Prometheus
* Azure Monito
* Google Cloud Monitoring
* AWS CloudWatch
* Elasticsearch
* Graphite
* InfluxDB
* Loki
* Microsoft SQL Server (MSSQL)
* MySQL
* OpenTSDB
* PostgreSQL

#### Prerequisites:-
1) Azure subscription
2) Helm cli

#### Step:-
1. Clone this repository 
2. 
