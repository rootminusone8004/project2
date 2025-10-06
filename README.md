# Project 2

<p align="center">
  <img src="./assets/results/result.png" alt="Argocd Images"/>
</p>

## Project Overview

This project contains the **UI and backend** for a student result management system. It demonstrates a fully containerized and automated deployment pipeline using **Kubernetes**, **ArgoCD**, **Prometheus**, **Grafana**, and **SonarQube**. The infrastructure is built for scalable and observable microservices, enabling CI/CD with robust monitoring and quality assurance tools.

## 📦 Deployment

In order to deploy the project, see [instructions](Instructions.md) for details.

## 🛠️ Tools & Technologies

- **Kubernetes** – Container orchestration
- **ArgoCD** – GitOps continuous delivery
- **Prometheus** – Metrics collection and alerting
- **Grafana** – Monitoring dashboards
- **Node Exporter** – Host-level metrics exporter
- **SonarQube** – Static code analysis
- **Kubernetes Dashboard** – Visual UI for managing workloads
- **Docker** – Containerization of applications
- **Helm** – Package management for Kubernetes

## 🚀 CI/CD Pipeline

<div align="center">

```mermaid
flowchart

A(code push to github) --> H(Github Actions execute CI/CD Pipeline)
H --> B(SonarQube analysis)
B --> C(Build Docker Image)
C --> D(Image Deployed to registry)
D --> E(ArgoCD syncs kubernetes menifests to cluster)
E --> F(Application is deployed to cluster)
F --> G(Monitoring tools observe performance)
```
</div>

## 🔧 infrastructure

This project uses Terraform + Ansible to spin up a Kubernetes cluster with kubeadm.
- 🔧 Infrastructure provisioning → **Terraform**
- ⚙️ Cluster setup & configuration → **Ansible**
- 🎯 Target: Bare-metal cloud VMs (via SSH) → **AWS EC2**
- 🧠 Bootstrap: `kubeadm`-based Kubernetes install

### 🧱 Terraform – Infra Provisioning

Terraform handles EC2 instance creation, networking, and any cloud init scripts.

``` bash
$ cd terraform/kubeadm
$ terraform init
$ terraform apply -auto-approve
```

[![View Repository](https://img.shields.io/badge/View%20Repository-Terraform-6A1B9A?style=flat-square&logo=github)](https://github.com/rootminusone8004/terraform)

### ⚙️ Ansible – Kubernetes Setup

Once the infra is live, Ansible kicks in to install and configure Kubernetes using `kubeadm`

``` bash
$ cd ansible/
$ ansible all -i inventory/kubeadm.yaml -m ping
$ ansible-playbook --ask-become-pass --skip-tags "docker_only" -i inventory/kubeadm.yaml playbooks/kubeadm.yaml
```

[![View Repository](https://img.shields.io/badge/View%20Repository-Ansible-FF2722?style=flat-square&logo=github)](https://github.com/rootminusone8004/ansible)

This playbook handles:
- Installing containerd
- Running kubeadm init
- Setting up kubectl access

### 🗺️ Flow Summary

<div align="center">

```mermaid
flowchart

    A(Terraform: Provision Infra) --> B(Ansible: Configure Kubernetes)
    B --> C(kubeadm init / join)
    C --> D(Cluster Ready ✅)
```
</div>

## 📸 Dashboards & Monitoring screenshots

### 🔹 ArgoCD (GitOps Management)

<p align="center">
  <img src="assets/argocd/1_nodes.png" alt="Argocd Images"/>
  <br><i>Cluster nodes</i>
</p>

<p align="center">
  <img src="assets/argocd/2_backend.png" alt="Argocd Images"/>
  <br><i>Backend pods</i>
</p>

<p align="center">
  <img src="assets/argocd/3_capstone.png" alt="Argocd Images"/>
  <br><i>Frontend pods</i>
</p>

<p align="center">
  <img src="assets/argocd/4_redis.png" alt="Argocd Images"/>
  <br><i>Other pods</i>
</p>

<p align="center">
  <img src="assets/argocd/5_metrics.png" alt="Argocd Images"/>
  <br><i>Cluster Metrics</i>
</p>

### 🔹 Kubernetes Dashboard

<p align="center">
  <img src="assets/k8s-dashboard/deployments.png" alt="k8s Dashboard Images"/>
  <br><i>kubernetes deployments</i>
</p>

<p align="center">
  <img src="assets/k8s-dashboard/pod1.png" alt="k8s Dashboard Images"/><br>
  <img src="assets/k8s-dashboard/pod2.png" alt="k8s Dashboard Images"/>
  <br><i>kubernetes pods</i>
</p>

<p align="center">
  <img src="assets/k8s-dashboard/replica.png" alt="k8s Dashboard Images"/>
  <br><i>replica sets</i>
</p>

<p align="center">
  <img src="assets/k8s-dashboard/stateful.png" alt="k8s Dashboard Images"/>
  <br><i>stateful sets</i>
</p>

<p align="center">
  <img src="assets/k8s-dashboard/workload.png" alt="k8s Dashboard Images"/>
  <br><i>workload</i>
</p>

<p align="center">
  <img src="assets/k8s.png" alt="k8s Dashboard Images"/>
  <br><i>Terminal showing pods and services</i>
</p>

### 🔹 Monitoring Stack

#### 📊 Grafana Dashboards

<p align="center">
  <img src="assets/monitor/grafana/usage1.png" alt="monitoring stack Images"/><br>
  <img src="assets/monitor/grafana2/usage1.png" alt="monitoring stack Images"/>
  <br><i>CPU metrics</i>
</p>

<p align="center">
  <img src="assets/monitor/grafana2/usage2.png" alt="monitoring stack Images"/><br>
  <img src="assets/monitor/grafana2/usage3.png" alt="k8s Dashboard Images"/>
  <br><i>pods' resource metrics</i>
</p>

#### 📊 Prometheus Dashboards

<p align="center">
  <img src="assets/monitor/prometheus/cpu.png" alt="monitoring stack Images"/>
  <br><i>CPU metrics</i>
</p>

<p align="center">
  <img src="assets/monitor/prometheus/network.png" alt="monitoring stack Images"/>
  <br><i>Network metrics</i>
</p>

<p align="center">
  <img src="assets/monitor/prometheus/prom1.png" alt="monitoring stack Images"/>
  <br><i>Healthy pods</i>
</p>

<p align="center">
  <img src="assets/monitor/prometheus/prom2.png" alt="monitoring stack Images"/>
  <br><i>Memory usage query</i>
</p>

### 🎯 Sonarqube code analysis

<p align="center">
  <img src="assets/sonar/dev/sonar_abridged.png" alt="saonarqube"/>
  <br><i>Quality gate passing</i>
</p>

<p align="center">
  <img src="assets/sonar/dev/sonar.png" alt="saonarqube"/>
  <br><i>Detailed view</i>
</p>

## 📄 License

This project is licensed under MIT license. See the [LICENSE](LICENSE.txt) file for details.
