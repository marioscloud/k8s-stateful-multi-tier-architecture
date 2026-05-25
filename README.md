🏗️ Stateful Multi-Tier Infrastructure Orchestration (Kubernetes)

📖 Overview

This repository contains production-ready, modular Infrastructure as Code (IaC) configurations designed to automate the deployment, scaling, and lifecycle management of a decoupled, multi-tier application on Kubernetes.

Moving beyond basic container deployment, this project demonstrates advanced cluster management practices. It highlights how to successfully isolate stateful database workloads, enforce persistent data lifecycles, securely inject configuration data, and implement strict network micro-segmentation.


⚙️ Architectural Framework & DevOps Practices

The infrastructure is engineered using modular Kubernetes manifests, focusing on scalability, high availability, and security:

-Stateful vs. Stateless Workload Decoupling: * Stateless Tier: Managed via standard Deployments for the frontend (WordPress), enabling horizontal pod autoscaling and rapid failure recovery.

-Stateful Tier: Utilizes StatefulSets for the backend (MySQL) to guarantee stable network identifiers, ordered deployment, and predictable persistent storage mapping.

-Secrets & Configuration Management: Decouples configuration from application logic using ConfigMaps (Nginx/MySQL environments) and Kubernetes Secrets to securely inject base64-encoded administrative credentials at runtime.

-Persistent Data Lifecycle: Implements Persistent Volume Claims (PVC) to guarantee zero data loss across pod evictions, database upgrades, or unexpected node failures.

-Zero-Trust Network Micro-segmentation: Enforces strict internal security boundaries via Network Policies, isolating the database layer to accept ingress traffic exclusively from the designated frontend application namespace.

-Environment Isolation: Provisions a dedicated Namespace to enforce logical resource boundaries and strict access control within the cluster.


🛠️ Technology Stack
-->Orchestration: Kubernetes (Compatible with EKS, GKE, AKS, or local clusters)

-->Configuration Management: Native Kubernetes YAML manifests

-->Application Stack: WordPress (Frontend) / Nginx (Web Server) / MySQL (Database)

-->Security: Native Kubernetes Network Policies & Secret Management


🚀 Infrastructure Deployment Guide

Prerequisites

A running Kubernetes cluster with administrative (cluster-admin) privileges.

kubectl CLI tool installed and authenticated to your cluster.

1. Environment Initialization
Clone the repository and establish the dedicated logical workspace (Namespace):

Bash
git clone https://github.com/marioscloud/k8s-stateful-multi-tier-architecture.git
cd k8s-stateful-multi-tier-architecture

# Provision the logical boundary and set the active context
kubectl create ns wordpress
kubectl config set-context --current --namespace=wordpress

2. Configuration & Secret Provisioning
Securely inject the database credentials and application environment variables:

Bash
# Provision encrypted credentials
kubectl apply -f secret.yaml

# Provision stateless configurations
kubectl apply -f nginx-cm.yaml
kubectl apply -f mysql-cm.yaml

# Verify configuration resources
kubectl get secret,cm

3. Persistent Storage Allocation
Provision the required storage abstractions (PVCs) to guarantee database state persistence:

Bash
kubectl apply -f pvc.yaml
kubectl get pvc,pv

4. Stateful Database Tier Deployment
Deploy the database workload and its associated internal headless service:

Bash
kubectl apply -f mysql.yaml

# Monitor the creation of the stateful workload (This may take a few moments)
kubectl get sts,svc

5. Application Tier & Network Hardening
Deploy the stateless frontend and apply the network security policies:

Bash
# Deploy the stateless application tier
kubectl apply -f wordpress.yaml

# Enforce network isolation (Zero-Trust policy for MySQL)
kubectl apply -f networkpolicy.yaml

# Verify deployment and security policies
kubectl get deploy,networkpolicy


📈 Verifying External Access

Once the pods are healthy (Running), retrieve the external access parameters to reach the application UI:

Bash
# Retrieve the cluster Node IP architecture
kubectl get nodes -o wide

# Identify the assigned NodePort for external routing
kubectl get service wordpress-service -n wordpress

Locate the mapped port under the PORT(S) column (e.g., 80:30004/TCP). You can now securely access the application frontend via http://<Node-IP>:<NodePort>.

📬 Contact & Attributions
Author: Mario Araos | Cloud & DevOps Engineer

Contact: marioscloud@duck.com

Attributions: Base deployment methodology inspired by Aswin Vijayan via DevOpsCube.

