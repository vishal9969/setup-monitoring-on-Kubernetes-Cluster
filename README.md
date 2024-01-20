# setup-monitoring-on-Kubernetes-Cluster
Setup monitoring on EKS Cluster using Prometheus and Grafana
![Uploading image.png…]()


![Uploading Screen Shot 2022-05-26 at 11.44.04 AM.png…]()



What is Prometheus?
Prometheus is an open source monitoring tool
Provides out-of-the-box monitoring capabilities for the Kubernetes container orchestration platform. It can monitor servers and databases as well.
Collects and stores metrics as time-series data, recording information with a timestamp 
It is based on pull and collects metrics from targets by scraping metrics HTTP endpoints.
What is Grafana?
Grafana is an open source visualization and analytics software. 
It allows you to query, visualize, alert on, and explore your metrics no matter where they are stored.
Why System monitoring is critical?
Alerting: can give early warning of issues or problems before they become serious, costly or irreversible.
Visibility: Monitoring provides real-time insights into the health and performance of your applications and infrastructure.
Capacity Planning: By collecting and analyzing data over time, you can make informed decisions about capacity planning, such as when to add or remove nodes, allocate more resources, or scale your applications.

Prometheus Architecture
https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgLBmyVI-pgnAx1AYCdGcif41q80EWn-gZ2OUQKovX1Z4WWFWmkpVJhDLk2NEuEbeHTOIYecswxwo_Y0hQDPnMYf62D3Vd9_Uuz82xVSHPPBCNAR6ctQmhaz_SOgPGfGwIOuUHjYvd5YGhxcB9NMxcfOpPsN7AQdocArKZ4hC5q7XLFfXDn4gMyBQsR/w400-h289/Prometheus%20and%20Key components:
    1. Prometheus server - Processes and stores metrics data
    2. Alert Manager - Sends alerts to any systems/channels
    3. Grafana - Visualize scraped data in UI
Installation Method:
The are are many ways you can setup Prometheus and Grafana. You can install in following ways:

1. Create all configuration files of both Prometheus and Grafana and execute them in right order.

2. Prometheus Operator - to simplify and automate the configuration and management of the Prometheus monitoring stack running on a Kubernetes cluster

3. Helm chart (Recommended) - Using helm to install Prometheus Operator including Grafana

Why to use Helm?
Helm is a package manager for Kubernetes. Helm simplifies the installation of all components in one command. Install using Helm is recommended as you will not be missing any configuration steps and very efficient. 
Pre-requisites:
EKS Cluster needs to be up and running. Click here to learn how to setup EKS cluster in AWS cloud.
Install Helm3
EC2 instance to access EKS cluster
Implementation steps
We need to add the Helm Stable Charts for your local client. Execute the below command:

helm repo add stable https://charts.helm.sh/stableGrafana.png
https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj_nrqxSYXh9fqvW3KIdcV3u10xsnSRZaxJ--v3CigRyXeHDF48_UdwYrh3VDogfjT4G0Hibh1sRTyIDTmgLcywdyeZXL-DDewnEw35rvFdVcAt3d_qCBgZs45w5uNcIrds_hifX83Vzfn17nNFNJmB1WUx0XdW1Ik9wBoNpswbtKcBZiglReLQBLsp/s320/Screen%20Shot%202022-05-24%20at%209.54.48%20PM.png

# Add prometheus Helm repo
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts



helm search repo prometheus-community

Prometheus and grafana helm chart moved to kube prometheus stack


Create Prometheus namespace
kubectl create namespace prometheus


Install kube-prometheus-stack

Below is helm command to install kube-prometheus-stack. The helm repo kube-stack-prometheus (formerly prometheus-operator) comes with a grafana deployment embedded.

helm install stable prometheus-community/kube-prometheus-stack -n prometheus

Lets check if prometheus and grafana pods are running already
kubectl get pods -n prometheus



kubectl get svc -n prometheus



This confirms that prometheus and grafana have been installed successfully using Helm.

In order to make prometheus and grafana available outside the cluster, use LoadBalancer or NodePort instead of ClusterIP.
Edit Prometheus Service
kubectl edit svc stable-kube-prometheus-sta-prometheus -n prometheus


Edit Grafana Service
kubectl edit svc stable-grafana -n prometheus


Verify if service is changed to LoadBalancer and also to get the Load Balancer URL.

kubectl get svc -n prometheus



Access Grafana UI in the browser

Get the URL from the above screenshot and put in the browser



UserName: admin 
Password: prom-operator

Create Dashboard in Grafana

In Grafana, we can create various kinds of dashboards as per our needs.
How to Create Kubernetes Monitoring Dashboard?
For creating a dashboard to monitor the cluster:



Click '+' button on left panel and select ‘Import’.

Enter 12740 dashboard id under Grafana.com Dashboard.

Click ‘Load’.

Select ‘Prometheus’ as the endpoint under prometheus data sources drop down.

Click ‘Import’.



This will show monitoring dashboard for all cluster nodes





How to Create Kubernetes Cluster Monitoring Dashboard?


For creating a dashboard to monitor the cluster:



Click '+' button on left panel and select ‘Import’.

Enter 3119 dashboard id under Grafana.com Dashboard.

Click ‘Load’.

Select ‘Prometheus’ as the endpoint under prometheus data sources drop down.

Click ‘Import’.

This will show monitoring dashboard for all cluster nodes















Create POD Monitoring Dashboard
For creating a dashboard to monitor the cluster:



Click '+' button on left panel and select ‘Import’.

Enter 6417 dashboard id under Grafana.com Dashboard.

Click ‘Load’.

Select ‘Prometheus’ as the endpoint under prometheus data sources drop down.

Click ‘Import’.








This will show monitoring dashboard for all cluster nodes.





Cleanup EKS Cluster
Use the below command to delete EKS cluster to avoid being charged by AWS.
eksctl delete cluster --name demo-eks --region us-east-1

or Login to AWS console --> AWS Cloud formation --> delete the stack manually.

you can also delete the cluster under AWS console --> Elastic Kubernetes Service --> Clusters
Click on Delete cluster


