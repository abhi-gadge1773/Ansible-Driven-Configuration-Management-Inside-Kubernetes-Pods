# 🔧 Ansible-Driven Configuration Management Inside Kubernetes Pods

This project demonstrates **how to perform configuration management using Ansible within Kubernetes Pods**. The approach combines Kubernetes orchestration with Ansible automation to manage software and configuration at the container level dynamically.

---

## 📌 Project Overview

| Component | Description |
|----------|-------------|
| **Automation Tool** | Ansible |
| **Orchestration Tool** | Kubernetes |
| **Container Runtime** | Docker |
| **Use Case** | Running Ansible playbooks from within a container running in a K8s Pod |
| **OS Base** | CentOS/Ubuntu (customized) |

---

## 🚀 Key Features

- 🔄 Use of Ansible for live configuration inside Kubernetes Pods
- 📦 Custom Docker image with Ansible installed
- ☸️ Kubernetes Pod deployment for Ansible container
- 📂 Ansible playbooks executed in real-time within Pods
- 🧪 Lightweight testing and validation scripts
- 🔧 Easily extendable for production environments

---

## 📂 Project Structure
├── Dockerfile
├── ansible/
│ ├── inventory
│ └── playbook.yml
├── k8s/
│ └── ansible-pod.yaml
└── README.md

```

---

## ⚙️ Prerequisites

- Docker installed locally
- Kubernetes cluster (e.g., Minikube or Kind)
- `kubectl` CLI
- Basic understanding of Ansible and Kubernetes

---

## 🔧 Setup Instructions

### 1️⃣ Clone the Repository

```bash
git clone https://github.com/abhi-gadge1773/Ansible-Driven-Configuration-Management-Inside-Kubernetes-Pods.git
cd Ansible-Driven-Configuration-Management-Inside-Kubernetes-Pods
```

2️⃣ Build the Ansible Docker Image

```
docker build -t ansible-in-pod .
```

3️⃣ Deploy Ansible Pod to Kubernetes

```
kubectl apply -f k8s/ansible-pod.yaml

```

4️⃣ Verify Pod Status

```
kubectl get pods
kubectl exec -it ansible-pod -- /bin/bash

```

🧪 Running Ansible Inside the Pod
Once inside the pod:

```
ansible-playbook -i inventory ansible/playbook.yml
```

This executes your configuration management tasks directly inside a live K8s container.


🛠 Use Cases

Dynamic configuration inside running containers

On-demand configuration validation

Ephemeral container setup using IaC

Test Ansible roles/playbooks inside K8s pods



📈 Future Improvements

✅ Integrate with ConfigMaps & Secrets

🔐 Use Ansible Vault for sensitive data

📦 Package image via CI pipeline (Jenkins/GitHub Actions)

📊 Monitoring with Prometheus/Grafana

☁️ Integrate with AWS/GCP Cloud Config





🙋‍♂️ Author
Abhijeet Gadge

🔗 GitHub
🔗 LinkedIn
🔗 Twitter















