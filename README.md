# 🏋️ Kubernetes NGINX Gym Website

![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)
![NGINX](https://img.shields.io/badge/NGINX-009639?style=for-the-badge&logo=nginx&logoColor=white)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)

A gym website deployed on Kubernetes using NGINX, with static HTML served via a **ConfigMap**, mounted as a volume in a **Deployment**, and exposed using a **NodePort Service**.

---

## 📋 Table of Contents

- [Project Overview](#project-overview)
- [Architecture](#architecture)
- [Prerequisites](#prerequisites)
- [Step-by-Step Setup](#step-by-step-setup)
  - [Step 1: Create index.html](#step-1-create-indexhtml)
  - [Step 2: Create ConfigMap](#step-2-create-configmap)
  - [Step 3: Create Deployment with Volume Mount](#step-3-create-deployment-with-volume-mount)
  - [Step 4: Expose Using NodePort Service](#step-4-expose-using-nodeport-service)
  - [Step 5: Access the Webpage](#step-5-access-the-webpage)
- [Project Structure](#project-structure)
- [Tech Stack](#tech-stack)

---

## 📖 Project Overview

This project demonstrates how to:
- Serve a **static HTML gym website** using NGINX on Kubernetes
- Store HTML content in a **ConfigMap** (instead of rebuilding the image every time)
- Mount the ConfigMap as a **Volume** inside the NGINX container
- Expose the application externally via a **NodePort Service**

---

## 🏗️ Architecture



---

## ✅ Prerequisites

- Kubernetes cluster (Minikube / kubeadm / any cloud cluster)
- `kubectl` configured and connected to your cluster
- Basic knowledge of Kubernetes objects

---

## 🚀 Step-by-Step Setup

### Step 1: Create `index.html`

Create your gym website HTML file:

---

### Step 2: Create ConfigMap

Create a ConfigMap from the `index.html` file:

```bash
kubectl create configmap gym-html --from-file=index.html
```

Verify the ConfigMap:

```bash
kubectl get configmap gym-html
kubectl describe configmap gym-html
```

---

### Step 3: Create Deployment with Volume Mount

Create the NGINX deployment and edit it to mount the ConfigMap as a volume:

```bash
kubectl create deployment gym-website --image=nginx
```

Edit the deployment to add the volume mount:

```bash
kubectl edit deployment gym-website
```

Add the following under `spec.template.spec`:

```yaml
volumes:
  - name: html-volume
    configMap:
      name: gym-html

containers:
  - name: nginx
    image: nginx
    volumeMounts:
      - name: html-volume
        mountPath: /usr/share/nginx/html
```

Verify the pod is running:

```bash
kubectl get pods
kubectl describe pod <pod-name>
```

---

### Step 4: Expose Using NodePort Service

Expose the deployment as a NodePort service:

```bash
kubectl expose deployment gym-website --type=NodePort --port=80
```

Check the service and assigned NodePort:

```bash
kubectl get svc gym-website
```

---

### Step 5: Access the Webpage

Get your Node IP:

```bash
kubectl get nodes -o wide
```

Open in your browser:
```bash
http://<NODE-IP>:<NODE-PORT>
```


## 📚 What I Learned

- How to store static files in a Kubernetes ConfigMap
- How to mount ConfigMaps as volumes inside a pod
- How to expose a deployment externally using NodePort
