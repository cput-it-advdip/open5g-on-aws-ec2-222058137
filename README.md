# Assignment 1: Deploying K3s on AWS

**Name:** Luchwayito Bokweni
**Student Number:** 222058137  
Advanced Diploma in IT 

---

## 🏗 Architecture Overview

### What is K3s?
K3s is a streamlined and certified Kubernetes distribution specifically engineered for environments with limited computing resources. It is particularly suitable for use cases such as edge computing, Internet of Things (IoT), and modern 5G infrastructures. Unlike traditional Kubernetes, K3s is packaged as a compact single binary (less than 100MB), achieved by removing non-essential, legacy, and experimental components. This makes it significantly easier to deploy, manage, and maintain in constrained environments.

### Core Components
* **Control Plane (Server Node):** This acts as the central management unit of the cluster. It includes the Kubernetes API Server, the Scheduler, and the Controller Manager.
* **Worker Nodes (Agents):** These nodes execute the actual workloads by running containerized applications deployed within the cluster.
* **Container Runtime:** K3s utilizes **containerd**, a lightweight runtime that efficiently handles container lifecycle operations.
* **Container Networking Interface (CNI):** The default solution, **Flannel**, enables communication between pods across different nodes via a Layer 3 VXLAN overlay.
* **Kine (Datastore Layer):** Instead of the resource-intensive etcd, K3s uses Kine to integrate with lightweight databases like **SQLite**.
* **Built-in Services:** Includes an embedded load balancer (ServiceLB) and **Traefik** as the ingress controller.

---

## 2. Infrastructure Specifications

The following AWS EC2 instances were provisioned to support the HA cluster:

| Component | Role           | Instance Type | Resources         | OS            |
|----------|----------------|--------------|-------------------|---------------|
| Master-1 | Initial Leader | t3.large     | 2 vCPU / 8GB RAM  | Ubuntu 22.04  |
| Master-2 | HA Peer        | t3.large     | 2 vCPU / 8GB RAM  | Ubuntu 22.04  |
| Master-3 | HA Peer        | t3.large     | 2 vCPU / 8GB RAM  | Ubuntu 22.04  |

---

## 🛠 Deployment Process

### 1. Infrastructure Provisioning and Security Configuration
An AWS Virtual Private Cloud (VPC) was configured to host the cluster. Security Groups were defined to allow the following essential traffic:
* **6443/tcp** – Kubernetes API Server
* **8472/udp** – Flannel VXLAN networking
* **10250/tcp** – Kubelet metrics and node management

---

## 📸 Deployment Evidence

### 3.1 Cluster Node Status
The cluster verification output demonstrates that both the control plane and worker node are in a **Ready** state.
![Nodes Status](nodes_status.png)

### 3.2 System Pods and Networking
This highlights critical system pods (CoreDNS, Metrics Server, Traefik) operating correctly within the `kube-system` namespace.
![Pods Status](pods_status.png)

### 3.3 AWS EC2 Console View
The AWS Management Console shows the active EC2 instances running in the `us-east-1` region.
![AWS Console](aws_console.png)

### 3.4 Installation Output from Terminal
This confirms the successful execution of installation and node join commands from the local CLI.
![Terminal Output](terminal_output.png)

---

## 🔍 Technical Reflection

### 4.1 Knowledge Gained and Skills Development
This assignment offered valuable insight into the practical implementation of cloud-native technologies. I developed hands-on experience working with AWS VPCs, routing tables, and Security Groups. Understanding the distinction between the control plane and worker nodes helped clarify how Kubernetes achieves high availability.

### 4.2 Challenges Encountered and Solutions Applied
* **GitHub 403 Error:** Encountered a "Permission Denied" error when pushing updates. Resolved by correctly re-linking the local Git origin to my specific assigned repository.
* **Connection Timeouts:** The worker node initially failed to join the master. After auditing AWS Security Groups, I identified that ports **6443** and **8472** were closed. Opening these ports allowed the worker to join immediately.

### 4.3 Role of K3s in 5G and Cloud-Native Environments
K3s is vital for 5G "Edge" processing. By replacing etcd with **SQLite**, K3s reduces the resource footprint, making it suitable for running **Containerized Network Functions (CNFs)** like the User Plane Function (UPF) on low-power hardware near cell towers.

### 4.4 Scalability Through Virtualization and Containerization
Virtualization (AWS EC2) provides hardware abstraction, while Containerization (K3s) ensures application portability. Together, they allow for **Horizontal Scaling**—dynamically launching containers and instances to handle 5G traffic spikes while maintaining cost efficiency.
