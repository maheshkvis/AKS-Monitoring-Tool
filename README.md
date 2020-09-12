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
![test](https://e9auzg.dm.files.1drv.com/y4m0xC5wKHC1TM6kwVfYzNszRptkMqvtznlIGT0xRpTVPmkGnBOsRdYHidgINrb0MNAvwwKnDMroMyAkSL1P6bM6Q_iTGZDxQW4T9ig3TxEgfSksGtqeCFJ-qi42Z-46xR33bIkIrxbElW12sAPkQAhjkYebrKAf3w35v9GzJ8l9fce5RmCXhRiQ41ZOI0SUQ8GyB_A9bzx9gDXYsqaq5BopQ?width=1024&height=238&cropmode=none)

#### Select Prometheus for data source. 
![test2](https://e9atzg.dm.files.1drv.com/y4mA8c0hZEJuDqTJUi97kt4LOZW_g6WQ3r9jLg-67lIdKWKCnhNkw9wEZ7E4iul0gl06XsCtKtR9qzGatvlkPm9IlyOZFGqtMvPwAWa5Jy18urowHTJ7ohj6NAqNRyghNrmaDiFe04bhLhAaZ6u9Z1I4PKmd9BCRkOMNwPrdj0TD9jbgmTIsw4869k7QUb4UZPjjd0F_HEgowCJQwQugU3gig?width=1024&height=188&cropmode=none)

#### Add your Prometheus server cluster ip in URL.
![test3](https://e9arzg.dm.files.1drv.com/y4mrutKebHbInN97ouy-A1V8pj36Jm7SdllnGwSikUlJplrzZZfAGLm4P7AYrKpT1j9TvFUa0sqiUTCdGb1fB94P7aaXg3HrZifFQ0x1C0CzeeIIGr9uybww3wDZ56iQVE2Nw4sIqHfWCeB1UvOy6tc7QpZ9PnaNDZcoJFm5aVJ12PYjf644L5MOKux8AxbeMGWiojX33k4QEEzSDZXLWje4g?width=1024&height=340&cropmode=none)

#### Import dashboard 
![test4](https://e9ayzg.dm.files.1drv.com/y4mK2cxf8EFWj5lTjsRLmnkoeD58p_786N5vF1SFAdvJn7k5KLCgW8ruGKxuyTfDFP8206cFU8oBpN1zRVVaVkbr6lPpOk1auyTT6xHdxrt48D5J2fJSpN81LccMWOJb5D8y5lrZiX2DU3uK_DxpN5dTLklOt9zPBzbMLBaDpUO-PpdJZpt2x3H-sU2Jp_TE0sGcrpR2h-HZVl6Itt07MZlxQ?width=660&height=344&cropmode=none)

#### Add dashboard ID 
![test5](https://e9axzg.dm.files.1drv.com/y4mZuXY6IoSCsK1OoYFTEddjba3ulYv3VxnPLNAq54Giz8SpZBCYX1AlMNLKLlosxbxExGPMA2O-mxuR4DT0GfCr2VygEUHF9PIoM-qPWk1K1Nq_laTjKgdVFclzbWiEHpg_dN5XDbppXNMp4_zJOPrsYTNqArVPA-WucaVeRuMq_SnKTssJA0z8Writ7VdfWXf32-0oe4O1fI9a8HnSD0OJw?width=1024&height=325&cropmode=none)

#### Select data source for the dashboard
![test6](https://e9aszg.dm.files.1drv.com/y4mKfAP6ZlX0mRmCC6ZRBlGs8XAjMO--qYo1q3eZJZ9JZAJQQxd5wH8IZ1DCmk3XNN3iZBvSj1YAukKAXzg1cSSFuQWC4jeNVgnv4nHF7QvW7B4fcKkeXFVRDl4D7yJX2UxGdTPd26DWvBZylegrXldU4UbdIZokhuiJcpNOutf0FLXUgCYZexgMXN3X7xf-PLElhqROOIEcmXRz44kA1sERg?width=660&height=233&cropmode=none)

#### This is the Final outcome 
![test7](https://e9avzg.dm.files.1drv.com/y4mKtTIEjN7Dx-dnc8crkJZvpqFzGcMLf2c0oxYDEJY_sjC01fJrl8uvi4cVT8DPj0PXKdWOGaY8m91RL25xRxIPEiRXVDrRscyBULliCS01-akTsSlCNMPw0wS1ith92owrKGlrH91O5HW_kbb7AgqjPkj38L4sBZ7zCGwbubuoh_KiwmOKQoKbP7gfqHEP1fnINM4_to0mDtHKjeX4dz8Ow?width=1024&height=493&cropmode=none)

## Feedback 
Any questions or suggestions?

You are welcome to discuss it on :)

[![Tweet](https://img.shields.io/twitter/url?style=social&url=https%3A%2F%2Ftwitter.com%2Fer_maheshkvish)](https://twitter.com/er_maheshkvish)<br>
[![Link](https://img.icons8.com/fluent/48/000000/linkedin.png)](https://www.linkedin.com/in/mahesh-kumar-393407153/)
