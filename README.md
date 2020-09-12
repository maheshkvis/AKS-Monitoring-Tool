# Prometheus and Grafana configure in Azure Kubernetes (AKS)

Prometheus and Grafana are open source monitoring tools for Kubernetes clusters. It allows users and developers to capture metrics and telemetry data for applications running inside the cluster, allowing deeper insights into application performance and reliability at the container level.

### Prometheus

Prometheus stores all The data as a time series. This data can be queried via the PromQL query language and visualized with a built-in expression browser. Prometheus does not Provide a dashboard, it relies on Grafana for visualizing data. it is one of the most used Kubernetes monitoring tools. Other tools from the Kubernetes ecosystem, including Istio, include a built-in Prometheus adapter that exposes generated metrics.

### Grafana

Grafana is used to visualize any data. It can be used to create multiple dashboards that support a variety of beautiful graphs and visualizations for different users and stakeholders. It has support for the integration different type of data sources
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

##### Prerequisites: -
1) Azure subscription
2) Azure CLI
3) Azure Kubernetes cluster 
4) Helm CLI

##### Step: -
 Clone this repository Execute below given CMD it will setup the AKS in your local system 
 ```
$ cd AKS-Monitoring-Tool\Prometheus
$ az login
$ az account list -o table
$ az account set --subscription <subscription ID>
$ az aks get-credentials -n <aks_name> -g <resource_group_name>
```
Install the Prometheus Helm chart
Set "rbac.create=true" if RBAC is enable in your cluster 
```
$ kubectl create namespace monitoring
$ helm install . --namespace monitoring --set rbac.create=false --generate-name
$ SVC_IP=$(kubectl get svc --namespace monitoring -l "app=prometheus,component=server" -o jsonpath="{.items[0].spec.clusterIP}"")
```
Note: Save the SVC_IP it will help you to set up the Grafana dashboard with Prometheus

Install Grafana Helm chart
```
$ cd ..\grafana
$ helm install . --set persistence.enabled=true --set persistence.accessModes={ReadWriteOnce} --set persistence.size=8Gi --generate-name --namespace monitoring
$export POD_NAME=$(kubectl get pods --namespace monitoring -l "app.kubernetes.io/name=grafana,app.kubernetes.io/instance=chart-1599858402" -o jsonpath="{.items[0].metadata.name}")
$ kubectl --namespace monitoring port-forward $POD_NAME 3000
```
The Grafana dashboard username is admin and for password execute this CMD
```
$ kubectl get secret --namespace monitoring chart-1599858402-grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```
Now you have successfully installed the Grafana and Prometheus.
Follow the next step for config Grafana dashboard.

## Grafana dashboard Setup
#### Login inside the Grafana dashboard and add your data source.
[](https://drive.google.com/uc?export=view&id=1fsGzmGV-CTiHLvBrkQEOVcr0Qw-gu_qh)

#### Select Prometheus for data source. 
[](https://drive.google.com/uc?export=view&id=bO4zYiX6CDIvWOQp4tW_Ypfn08NJPwTV)

#### Add your Prometheus server cluster ip in URL.
[](https://drive.google.com/uc?export=view&id=1_RyVpiWEvLe3gEj2GuUPTmFVzkOjZHmk)

#### Import dashboard 
[](https://drive.google.com/uc?export=view&id=1GPYm5f1-m5UGw2_GZqYo1s6ilpD5V5NW)

#### Add dashboard ID 
[](https://drive.google.com/uc?export=view&id=1vCoRtJ02-V8RFzdfe1xtGtHpfymeYtja)

#### Select data source for the dashboard
[](https://drive.google.com/uc?export=view&id=1SBXXAXsKSKSwrIvYgDSMRhBHWlb2AtlQ)

#### This is the Final outcome 
[](https://drive.google.com/uc?export=view&id=1iDkj5nCYjsFzYB5lytBk0ka7aw5xcncx)
