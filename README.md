# wecloudTask6
Task6 Kube Fluentd Prometheus Grafana
# Monitoring Setup with Prometheus and Grafana

This guide provides step-by-step instructions for setting up monitoring using Prometheus and Grafana in Kubernetes. We will deploy FluentD as a DaemonSet to collect logs from all nodes in the cluster.

## Prerequisites

1. EKS  up and running.
2. Helm installed on your local machine.

## Step 1: Create app.yaml

Ensure you have the necessary deployment configurations for your application in the app.yaml file.

## Step 2: Create clusterRole.yaml, rolebinding.yaml, and serviceAccount.yaml

Create the required Kubernetes RBAC resources to grant necessary permissions to FluentD. Here are the files you need to create:

- `clusterRole.yaml`
- `rolebinding.yaml`
- `serviceAccount.yaml`

## Step 3: Create configMap for FluentD (config-map.yaml)

Create a ConfigMap to store the FluentD configuration. The `config-map.yaml` file should contain the FluentD configuration, including input and filter configurations.

## Step 4: Provision FluentD DaemonSet

Deploy FluentD as a DaemonSet so it can run on all nodes in the cluster and collect logs from various pods.

```bash
kubectl apply -f app.yaml
kubectl apply -f clusterRole.yaml
kubectl apply -f rolebinding.yaml
kubectl apply -f serviceAccount.yaml
kubectl apply -f config-map.yaml
kubectl create ns monitoring

## Step 5: Create values.yaml
Create a values.yaml file to customize the Helm chart installation.

## Step 6: Install Prometheus and Grafana
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

helm install monitoring prometheus-community/kube-prometheus-stack --namespace=monitoring

helm upgrade monitoring prometheus-community/kube-prometheus-stack --namespace=monitoring --values values.yaml

Accessing the UI
After the installation is complete, you can access the Prometheus and Grafana UIs.

For Prometheus:
kubectl port-forward -n monitoring <prometheus pod name> 9090:9090
For Grafana:
kubectl port-forward service/<monitoring-grafana> 3000:80 -n monitoring

## Alternatively, modify the service for Prometheus and Grafana to use LoadBalancer instead of ClusterIP. AWS will create a LoadBalancer with DNS, so you can access the UIs using the LoadBalancer's DNS.

For Grafana:

Username: admin
Password: prom-operator
