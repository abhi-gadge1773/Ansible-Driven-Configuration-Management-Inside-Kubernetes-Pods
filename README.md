
# 🎯 Ansible-Driven Configuration Management Inside Kubernetes Pods

**Tags**: Kubernetes · Ansible · PHP · Docker · AWS

Automated **PHP Application Deployment** using **Ansible Configuration Management** inside a Kubernetes Pod running on an AWS EC2 instance with Minikube. This project showcases **Infrastructure as Code (IaC)** principles and real-world containerized automation.

---

## 👨‍💻 Author

**Abhijeet Gadge**  
🔗 [GitHub](https://github.com/abhi-gadge1773) | 🔗 [LinkedIn](https://www.linkedin.com/in/abhijeetgadge/) | 🔗 [Twitter](https://x.com/AbhiGadge5)

---

## 📋 Table of Contents

- 🎯 [Project Overview](#project-overview)
- 🏗️ [Architecture](#architecture)
- 🔧 [Prerequisites](#prerequisites)
- ⚡ [Quick Start](#quick-start)
- 📝 [Detailed Setup](#detailed-setup)
- 🔍 [Monitoring & Validation](#monitoring--validation)
- 🚨 [Troubleshooting](#troubleshooting)
- 💡 [Best Practices](#best-practices)
- 🔗 [Additional Resources](#additional-resources)

---

## 🎯 Project Overview

This project demonstrates advanced DevOps practices using:

✅ **Configuration Management**: Ansible playbooks for PHP app deployment  
✅ **Container Orchestration**: Kubernetes pod lifecycle and networking  
✅ **Infrastructure Automation**: Minikube cluster on AWS EC2  
✅ **Security**: SSH-based secure communication between Pods  
✅ **Service Exposure**: NodePort configuration for public access

---

## 🎪 Key Benefits

- 🔄 **Reproducible**: Consistent deployments across any environment  
- ⚙️ **Maintainable**: Ansible-based modular configuration  
- 🔐 **Secure**: Pod-to-pod communication via SSH  
- 🚀 **Scalable**: Horizontally scalable app layer

---

## 🏗️ Architecture

```

┌─────────────────────────────────────────────────────────────┐
│                    AWS EC2 Instance                         │
│  ┌─────────────────────────────────────────────────────────┐
│  │                Minikube Cluster                         │
│  │  ┌─────────────────┐    ┌─────────────────────────┐     │
│  │  │   Ansible Pod   │    │      PHP App Pod       │     │
│  │  │                 │    │                         │     │
│  │  │ • Ansible       │────│ • Apache2 + PHP        │     │
│  │  │ • SSH Client    │SSH │ • SSH Server            │     │
│  │  │ • Playbooks     │    │ • Web Application       │     │
│  │  └─────────────────┘    └─────────────────────────┘     │
│  │                              │                           │
│  │                              │ NodePort Service          │
│  └──────────────────────────────┼───────────────────────────┘
│                                 │
└─────────────────────────────────▼────────────────────────────┘
Browser Access

````

---

## 🔧 Prerequisites

### ✅ System Requirements

- **OS**: Ubuntu 22.04 LTS (EC2)
- **RAM**: 4–8 GB
- **CPU**: 2+ cores
- **Storage**: 20 GB+
- **Network**: Internet access for downloads

### 🛠 Required Tools

- Docker
- kubectl
- Minikube
- SSH client

---

## ⚡ Quick Start

### 1️⃣ Environment Setup (EC2 Ubuntu)

```bash
sudo apt update -y && sudo apt upgrade -y
sudo apt install -y curl wget apt-transport-https ca-certificates gnupg lsb-release
sudo apt install -y docker.io
sudo usermod -aG docker $USER
newgrp docker
sudo systemctl enable docker
sudo systemctl start docker
docker run hello-world
````

### 2️⃣ Install Kubernetes Tools

```bash
# kubectl
curl -LO "https://dl.k8s.io/release/$(curl -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl && sudo mv kubectl /usr/local/bin/

# Minikube
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

### 3️⃣ Launch Cluster

```bash
minikube start --driver=docker --memory=4096 --cpus=2
kubectl get nodes
```

---

## 📝 Detailed Setup

### 🐳 Build PHP App Image

```bash
docker build -t php-app-with-ssh .
```

### 📦 Deploy Kubernetes Pods

```bash
kubectl apply -f k8s/app-pod.yaml
kubectl apply -f k8s/ansible-pod.yaml
kubectl wait --for=condition=Ready pod/app --timeout=300s
```

### ⚙️ Configure Ansible Pod

```bash
kubectl exec -it ansible-pod -- bash

# Inside the pod:
apt update && apt install -y ansible sshpass openssh-client python3 vim
mkdir -p /ansible/files
```

### 🖥️ Configure PHP App Pod

```bash
kubectl exec -it app -- bash

# Inside the pod:
apt update && apt install -y openssh-server python3 vim
echo 'root:root' | chpasswd
echo 'PermitRootLogin yes' >> /etc/ssh/sshd_config
service ssh start
```

### 📋 Ansible Inventory & Playbook

```bash
cat > /ansible/hosts <<EOF
[web]
<php-pod-ip> ansible_connection=ssh ansible_user=root ansible_password=root ansible_python_interpreter=/usr/bin/python3
EOF

export ANSIBLE_HOST_KEY_CHECKING=False
```

```bash
# /ansible/playbook.yml
---
- name: Deploy PHP App
  hosts: web
  become: yes
  tasks:
    - name: Install Apache
      apt:
        name: apache2
        state: present
        update_cache: yes
    - name: Deploy PHP file
      copy:
        src: files/mypage.php
        dest: /var/www/html/mypage.php
        owner: www-data
        group: www-data
        mode: '0644'
    - name: Restart Apache
      service:
        name: apache2
        state: restarted
```

### 🚀 Run Deployment

```bash
ansible-playbook -i hosts playbook.yml -v
```

---

## 🔍 Monitoring & Validation

```bash
kubectl get pods -o wide
kubectl logs app -f
kubectl describe pod ansible-pod
```

### 🌐 Service Exposure

```bash
kubectl apply -f k8s/app-service.yaml
minikube service app-service --url
```

---

## 🚨 Troubleshooting

| Problem            | Solution                                |
| ------------------ | --------------------------------------- |
| ❌ Pod not ready    | `kubectl describe pod <pod>`            |
| ❌ SSH failure      | Check SSH service in PHP pod            |
| ❌ 403 Apache error | Fix file permissions and restart Apache |
| ❌ Network failure  | Ping between pods & validate IPs        |

---

## 💡 Best Practices

* 🔐 Use SSH keys instead of passwords
* 🔒 Use Kubernetes Secrets for sensitive data
* 🔧 Apply resource limits for pods
* 🔄 Add CI/CD integration for auto deployment
* 📊 Integrate monitoring tools like Prometheus & Grafana

---

## 🔗 Additional Resources

* [Kubernetes Documentation](https://kubernetes.io/docs/)
* [Ansible Docs](https://docs.ansible.com/)
* [Minikube Setup](https://minikube.sigs.k8s.io/)
* [Docker CLI](https://docs.docker.com/engine/reference/commandline/cli/)

---

## ⭐ Show Your Support

If this project helped you, give it a ⭐ on GitHub and share it!

```
