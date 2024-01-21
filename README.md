# How to setup monitoring on Kubernetes Cluster using Prometheus and Grafana \| Setup monitoring on EKS Cluster using Prometheus and Grafana ![](media/6d6fef669de63bee5fdb9dca1ea83efb.png)
![image](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/9fb8b10f-02a6-480e-b52b-b9a7c7d0ca09)

### What is Prometheus?

-   Prometheus is an open source monitoring tool
-   Provides out-of-the-box monitoring capabilities for the Kubernetes container orchestration platform. It can monitor servers and databases as well.
-   Collects and stores metrics as time-series data, recording information with a timestamp
-   It is based on pull and collects metrics from targets by scraping metrics HTTP endpoints.

### What is Grafana?

-   Grafana is an open source visualization and analytics software.
-   It allows you to query, visualize, alert on, and explore your metrics no matter where they are stored.

### Why System monitoring is critical?

-   **Alerting:** can give early warning of issues or problems before they become serious, costly or irreversible.
-   **Visibility**: Monitoring provides real-time insights into the health and performance of your applications and infrastructure.
-   **Capacity Planning**: By collecting and analyzing data over time, you can make informed decisions about capacity planning, such as when to add or remove nodes, allocate more resources, or scale your applications.

### Prometheus Architecture

![image](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/c7877350-6b2e-4445-b2bc-64ec006c1890)

### Key components:

1\. Prometheus server - Processes and stores metrics data

2\. Alert Manager - Sends alerts to any systems/channels

3\. Grafana - Visualize scraped data in UI

### Installation Method:

The are are many ways you can setup Prometheus and Grafana. You can install in following ways:

1\. Create all configuration files of both Prometheus and Grafana and execute them in right order.

2\. Prometheus Operator - to simplify and automate the configuration and management of the Prometheus monitoring stack running on a Kubernetes cluster

3\. Helm chart (Recommended) - Using helm to install Prometheus Operator including Grafana

#### Why to use Helm?

Helm is a package manager for Kubernetes. Helm simplifies the installation of all components in one command. Install using Helm is recommended as you will not be missing any configuration steps and very efficient.

# Pre-requisites:

## EKS Cluster needs to be up and running. [Click here to learn](https://www.coachdevops.com/2022/02/create-amazon-eks-cluster-by-eksctl-how.html) how to setup EKS cluster in AWS cloud.

## [Install Helm3](https://www.coachdevops.com/2021/03/install-helm-3-linux-setup-helm-3-on.html)

## EC2 instance to access EKS cluster

#### Implementation steps

We need to add the Helm Stable Charts for your local client. Execute the below command:

helm repo add stable https://charts.helm.sh/stable

![image](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/e0700ada-e9c7-449a-993b-062197ed9456)


\# Add prometheus Helm repo

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

![image](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/61622365-2d69-4280-b104-40d2e56abd94)

helm search repo prometheus-community

Prometheus and grafana helm chart moved to kube prometheus stack

![image](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/6da9ffc1-a2b5-4c64-a9e6-9f113063e8a4)

Create Prometheus namespace

kubectl create namespace prometheus

![image](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/cd817efe-aa1f-4ecd-be46-7f45579a2b00)

#### Install kube-prometheus-stack

Below is helm command to install kube-prometheus-stack. The helm repo kube-stack-prometheus (formerly prometheus-operator) comes with a grafana deployment embedded.

helm install stable prometheus-community/kube-prometheus-stack -n prometheus

![image](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/d6937d13-c6c1-4f93-8ffc-83148c9bca19)

Lets check if prometheus and grafana pods are running already

kubectl get pods -n prometheus

![image](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/f068f000-fe28-435d-9dcf-f2e32f6422e5)

kubectl get svc -n prometheus

![image](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/4c24143f-758d-41cc-a888-cda94d3c0621)

This confirms that prometheus and grafana have been installed successfully using Helm.

In order to make prometheus and grafana available outside the cluster, use LoadBalancer or NodePort instead of ClusterIP.

#### Edit Prometheus Service

kubectl edit svc stable-kube-prometheus-sta-prometheus -n prometheus

![image](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/f2e5da9c-38aa-4fff-a837-2168ad041fb4)

#### Edit Grafana Service

kubectl edit svc stable-grafana -n prometheus

![image](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/57f501da-225b-4e96-b875-c0f74efa4417)

Verify if service is changed to LoadBalancer and also to get the Load Balancer URL.

kubectl get svc -n prometheus

![image](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/e4fcd8b4-8d87-4dbd-8608-37ad201f74e1)

**Access Grafana UI in the browser**

**  
**

Get the URL from the above screenshot and put in the browser

![image](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/41f47922-dd53-45fd-a81f-ce04a686caa0)

UserName: admin

Password: prom-operator

**Create Dashboard in Grafana**

In Grafana, we can create various kinds of dashboards as per our needs.

## How to Create Kubernetes Monitoring Dashboard?

For creating a dashboard to monitor the cluster:

Click '+' button on left panel and select ‘Import’.

Enter 12740 dashboard id under Grafana.com Dashboard.

Click ‘Load’.

Select ‘Prometheus’ as the endpoint under prometheus data sources drop down.

Click ‘Import’.

This will show monitoring dashboard for all cluster nodes

![image](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/fff8656a-209d-4654-879e-1b8d6435c9dd)


**How to Create Kubernetes Cluster Monitoring Dashboard?**

For creating a dashboard to monitor the cluster:

Click '+' button on left panel and select ‘Import’.

Enter 3119 dashboard id under Grafana.com Dashboard.

Click ‘Load’.

Select ‘Prometheus’ as the endpoint under prometheus data sources drop down.

Click ‘Import’.

This will show monitoring dashboard for all cluster nodes

![image](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/534101bf-2de0-4932-bf09-fdb578a5b81a)

![image](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/c5e29be5-97b6-4261-a93d-da84bccd8ce0)

![image](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/c8362590-daca-4dc2-a035-7db1970b0194)


## Create POD Monitoring Dashboard

For creating a dashboard to monitor the cluster:

Click '+' button on left panel and select ‘Import’.

Enter 6417 dashboard id under Grafana.com Dashboard.

Click ‘Load’.

Select ‘Prometheus’ as the endpoint under prometheus data sources drop down.

Click ‘Import’.

![image](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/9b39c665-3c5d-42a6-9215-eec82ed80879)

![image](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/67e9ace2-74f0-498b-9639-6a1f9230b2ea)

This will show monitoring dashboard for all cluster nodes.

![image](https://github.com/vishal9969/setup-monitoring-on-Kubernetes-Cluster/assets/112000540/87442570-0238-4ebe-b70c-06197aee5921)


### Cleanup EKS Cluster

Use the below command to delete EKS cluster to avoid being charged by AWS.

eksctl delete cluster --name demo-eks --region us-east-1

**or Login to AWS console --\> AWS Cloud formation --\> delete the stack manually.**

**you can also delete the cluster under AWS console --\> Elastic Kubernetes Service --\> Clusters**

**Click on Delete cluster**
