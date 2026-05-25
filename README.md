# 🏗️ Stateful Multi-Tier Infrastructure Orchestration (Kubernetes)

[![Orchestration](https://img.shields.io/badge/Kubernetes-v1.26+-326ce5?style=flat-square&logo=kubernetes)](#)
[![Infrastructure as Code](https://img.shields.io/badge/IaC-Modular_Manifests-orange?style=flat-square)](#)
[![Security](https://img.shields.io/badge/Security-Network_Policy-success?style=flat-square)](#)

> **🎯 Executive Summary**
> This repository contains production-ready, modular **Infrastructure as Code (IaC)** configurations designed to automate the deployment, scaling, and lifecycle management of a decoupled, multi-tier application on Kubernetes. 

Moving beyond basic container deployment, this project demonstrates advanced cluster management practices. It highlights how to successfully isolate stateful database workloads, enforce persistent data lifecycles, securely inject configuration data, and implement strict network micro-segmentation.

---

## ⚙️ Architectural Framework & DevOps Practices

The infrastructure is engineered using modular Kubernetes manifests, focusing on **scalability**, **high availability**, and **security**:

* **Stateful vs. Stateless Workload Decoupling:** * **Stateless Tier:** Managed via standard `Deployments` for the frontend (WordPress), enabling horizontal pod autoscaling and rapid failure recovery.
  * **Stateful Tier:** Utilizes `StatefulSets` for the backend (MySQL) to guarantee stable network identifiers, ordered deployment, and predictable persistent storage mapping.
* **Secrets & Configuration Management:** Decouples configuration from application logic using `ConfigMaps` (Nginx/MySQL environments) and `Kubernetes Secrets` to securely inject base64-encoded administrative credentials at runtime.
* **Persistent Data Lifecycle:** Implements `Persistent Volume Claims (PVC)` to guarantee **zero data loss** across pod evictions, database upgrades, or unexpected node failures.
* **Zero-Trust Network Micro-segmentation:** Enforces strict internal security boundaries via `Network Policies`, isolating the database layer to accept ingress traffic *exclusively* from the designated frontend application namespace.
* **Environment Isolation:** Provisions a dedicated `Namespace` to enforce logical resource boundaries and strict access control within the cluster.

---

## 🛠️ Technology Stack
* **Orchestration:** Kubernetes (Compatible with EKS, GKE, AKS, or local clusters)
* **Configuration Management:** Native Kubernetes YAML manifests
* **Application Stack:** WordPress (Frontend) / Nginx (Web Server) / MySQL (Database)
* **Security:** Native Kubernetes Network Policies & Secret Management

---

## 🚀 Infrastructure Deployment Guide

### Prerequisites
* A running Kubernetes cluster with administrative (`cluster-admin`) privileges.
* `kubectl` CLI tool installed and authenticated to your cluster.

### 1. Environment Initialization
Clone the repository and establish the dedicated logical workspace (Namespace):

```bash
git clone https://github.com/marioscloud/k8s-stateful-multi-tier-architecture.git
cd k8s-stateful-multi-tier-architecture

# Provision the logical boundary and set the active context
kubectl create ns wordpress
kubectl config set-context --current --namespace=wordpress
