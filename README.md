# DevOps Tools Repository

## Overview

This repository serves as a collection of configurations, scripts, and examples for implementing various DevOps practices, primarily focused on Kubernetes-based environments. It includes setups for logging with the ELK stack (Elasticsearch, Logstash/Filebeat, Kibana), monitoring with Prometheus and Grafana, and a sample application for demonstrating metrics exposure. The repository is structured into thematic directories for easy navigation.

The primary languages used are JavaScript (for the sample app), Shell (for scripts), and Dockerfile (for containerization). 

Note: Some directories (e.g., `bluegreen` and `minikube`) are placeholders and currently contain no files. They are intended for future expansions on blue-green deployments and local Minikube setups.

## Directory Structure

- **`bluegreen/`**  
  Placeholder for blue-green deployment configurations and scripts. Currently empty.

- **`elk/`**  
  Contains Helm values files for deploying the ELK stack on Kubernetes. These files customize the installation of Elasticsearch, Filebeat, and Kibana, including S3 credentials for backups.  
  - `es-values-firstcopy.yaml`: Initial Helm values for Elasticsearch setup.  
  - `es-values-finalcopy.yaml`: Refined or final Helm values for Elasticsearch.  
  - `filebeat-values.yaml`: Helm values for Filebeat (log shipping).  
  - `kibana-values.yaml`: Helm values for Kibana (visualization dashboard).  
  - `elasticsearch-s3-credentials.yaml`: Secret credentials for Elasticsearch S3 repository integration.

- **`minikube/`**  
  Placeholder for Minikube-related configurations (local Kubernetes cluster). Currently empty.

- **`prometheus-grafana/`**  
  Configurations for setting up Prometheus and Grafana for monitoring Kubernetes clusters.  
  - `custom-kube-prometheus-stack.yaml`: Custom Helm values for the kube-prometheus-stack chart, which installs Prometheus, Grafana, and related components.  
  - `app/`  
    A sample JavaScript application designed to expose Prometheus-compatible metrics at a specific endpoint. This can be used to demonstrate monitoring in a Kubernetes environment. (Details on files within `app/` include source code, dependencies, and build instructions via Dockerfile.)

## Prerequisites

- Kubernetes cluster (e.g., Minikube for local development or a managed service like EKS/GKE).  
- Helm 3+ installed for deploying charts.  
- kubectl for interacting with the cluster.  
- Docker for building the sample app (if applicable).

## Installation and Usage

### ELK Stack Setup
1. Install the Elasticsearch Helm chart using the provided values:
   ```
     helm repo add elastic https://helm.elastic.co
     helm install elasticsearch elastic/elasticsearch -f elk/es-values-finalcopy.yaml
   ```
2. Apply S3 credentials:
   ```
      kubectl apply -f elk/elasticsearch-s3-credentials.yaml
   ```
4. Install Filebeat and Kibana similarly using their respective values files.

### Prometheus and Grafana Setup

1. Add the Prometheus community Helm repo:

   ```
    helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
    helm repo update
   ```
   
2. Install the kube-prometheus-stack:
   ```
    helm install prometheus prometheus-community/kube-prometheus-stack -f prometheus-grafana/custom-kube-prometheus-stack.yaml
   ```

3. Deploy the sample app in `prometheus-grafana/app/` to your cluster to test metrics scraping.

### Sample App Deployment

- Navigate to `prometheus-grafana/app/`.  
- Build the Docker image (using the Dockerfile if present).  
- Deploy to Kubernetes and configure Prometheus to scrape metrics from the app's endpoint.

For detailed steps, refer to official documentation for each tool:  
- [ELK Stack Helm Charts](https://github.com/elastic/helm-charts)  
- [Kube-Prometheus-Stack](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack)
