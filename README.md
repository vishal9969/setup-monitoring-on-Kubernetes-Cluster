# setup-monitoring-on-Kubernetes-Cluster
Setup monitoring on EKS Cluster using Prometheus and Grafana

![Screen Shot 2022-05-26 at 11 44 04 AM](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/3bb5a4fc-30a1-43f7-972b-6c74c3b316c2)

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

![Prometheus and Grafana](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/ed63e644-ab4f-471e-9998-682eee90fd2c)

Key components:
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


# Add prometheus Helm repo
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts


Implementation steps
We need to add the Helm Stable Charts for your local client. Execute the below command:

helm repo add stable https://charts.helm.sh/stable

![Uploading Screen Shot 2022-05-24 at 9.54.48 PM.png…]()


# Add prometheus Helm repo
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

![Screen Shot 2022-05-24 at 9 55 51 PM](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/dc1eeea8-410d-4839-a7eb-d78f48d78d0c)

Prometheus and grafana helm chart moved to kube prometheus stack

![Screen Shot 2022-05-12 at 11 03 29 PM](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/ed4f26e0-9114-43d0-994b-2dd584fcd09b)

Create Prometheus namespace
kubectl create namespace prometheus

![Screen Shot 2022-05-20 at 6 14 33 PM](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/0d91e976-b035-4fd0-9584-39a6c1cc2127)

Install kube-prometheus-stack

Below is helm command to install kube-prometheus-stack. The helm repo kube-stack-prometheus (formerly prometheus-operator) comes with a grafana deployment embedded.

helm install stable prometheus-community/kube-prometheus-stack -n prometheus

![Screen Shot 2022-05-20 at 6 16 04 PM](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/45114951-8a31-44d0-9d2f-37b80ee07666)

Lets check if prometheus and grafana pods are running already
kubectl get pods -n prometheus

![Screen Shot 2022-05-13 at 8 18 00 PM](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/1aea25ee-ce70-40a0-821d-9f9f386d75a1)


kubectl get svc -n prometheus

![Screen Shot 2022-05-20 at 6 18 09 PM](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/b2285df5-db42-4824-bc90-627c505cabec)


This confirms that prometheus and grafana have been installed successfully using Helm.

In order to make prometheus and grafana available outside the cluster, use LoadBalancer or NodePort instead of ClusterIP.
Edit Prometheus Service
kubectl edit svc stable-kube-prometheus-sta-prometheus -n prometheus

![Screen Shot 2022-05-20 at 6 20 20 PM](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/0394d57a-5451-41c7-8907-b26e6f02befa)

Edit Grafana Service
kubectl edit svc stable-grafana -n prometheus

![Screen Shot 2022-05-20 at 6 21 20 PM](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/330284f5-b338-48f3-92b9-cc7e547a1a26)


Verify if service is changed to LoadBalancer and also to get the Load Balancer URL.

kubectl get svc -n prometheus

![Screen Shot 2022-05-13 at 8 19 56 PM](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/3db7108b-407c-443e-a011-eee565583459)


Access Grafana UI in the browser

Get the URL from the above screenshot and put in the browser

![Screen Shot 2022-05-20 at 6 37 54 PM](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/0c9826e5-80cd-46f2-8bc9-2a291928ed7b)


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


![Screen Shot 2022-05-24 at 10 28 00 PM](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/c489568e-819f-4e81-a02d-7397d333f9f3)



How to Create Kubernetes Cluster Monitoring Dashboard?


For creating a dashboard to monitor the cluster:



Click '+' button on left panel and select ‘Import’.

Enter 3119 dashboard id under Grafana.com Dashboard.

Click ‘Load’.

Select ‘Prometheus’ as the endpoint under prometheus data sources drop down.

Click ‘Import’.

This will show monitoring dashboard for all cluster nodes


![Screen Shot 2022-05-20 at 6 45 21 PM](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/aedf4595-3a92-4d24-87db-37c1c68d131c)

![Screen Shot 2022-05-20 at 6 47 53 PM](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/a2d122d6-7f3d-4b39-bcb7-063b88786a9f)

![Screen Shot 2022-05-20 at 6 48 33 PM](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/44afbeea-acc2-4356-a4a6-00bc69c16a7d)



Create POD Monitoring Dashboard
For creating a dashboard to monitor the cluster:



Click '+' button on left panel and select ‘Import’.

Enter 6417 dashboard id under Grafana.com Dashboard.

Click ‘Load’.

Select ‘Prometheus’ as the endpoint under prometheus data sources drop down.

Click ‘Import’.


![Screen Shot 2022-05-20 at 6 51 46 PM](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/5528b5b9-0a7f-463b-a006-3cb32e7b44c2)


![Screen Shot 2022-05-20 at 6 51 58 PM](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/25a89b98-6aa1-48fa-b75f-da8301ecca19)




This will show monitoring dashboard for all cluster nodes.

![Screen Shot 2022-05-20 at 6 53 28 PM](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/15864ce8-48b0-4e4c-9518-c8716ea776cb)



Cleanup EKS Cluster
Use the below command to delete EKS cluster to avoid being charged by AWS.
eksctl delete cluster --name demo-eks --region us-east-1

or Login to AWS console --> AWS Cloud formation --> delete the stack manually.

you can also delete the cluster under AWS console --> Elastic Kubernetes Service --> Clusters
Click on Delete cluster
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


