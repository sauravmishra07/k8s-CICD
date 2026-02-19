# Task Management System - React + Vite + Kubernetes

A modern task management application built with **React**, **Vite**, **Tailwind CSS**, and deployed on **Kubernetes** with automated CI/CD pipeline support.

## 📋 Table of Contents

- [Project Overview](#project-overview)
- [Prerequisites](#prerequisites)
- [Local Setup](#local-setup)
- [Docker Setup](#docker-setup)
- [Kubernetes Deployment](#kubernetes-deployment)
- [CI/CD Pipeline](#cicd-pipeline)
- [Available Commands](#available-commands)

## 🎯 Project Overview

This is a task management application with role-based access:
- **Admin Dashboard**: Manage tasks and users
- **Employee Dashboard**: View and manage assigned tasks
- **Authentication**: Email/password-based login system
- **Local Storage**: Persistent user data storage

## 📦 Prerequisites

Before getting started, ensure you have the following installed:

- **Node.js** (v18+)
- **npm** or **yarn**
- **Docker** (for containerization)
- **Kubernetes** (kubectl configured)
- **Docker Hub account** (for pushing images)

## 🚀 Local Setup

### 1. Clone the Repository

```bash
git clone https://github.com/sauravmishra07/k8s-CICD.git
cd k8s-CICD
```

### 2. Install Dependencies

```bash
npm ci
# or
npm install
```

### 3. Run Development Server

```bash
npm run dev
```

The application will be available at `http://localhost:5173`

### 4. Build for Production

```bash
npm run build
```

## 🐳 Docker Setup

### Build Docker Image

```bash
docker build -t sauravmishra07/ems:tag .
```

### Run Docker Container Locally

```bash
docker run --rm -p 5173:80 sauravmishra07/ems:tag
```

The application will be available at `http://localhost:5173`

### Push to Docker Registry

```bash
docker login
docker push sauravmishra07/ems:tag
```

## ☸️ Kubernetes Deployment

### 1. Create Namespace

```bash
kubectl apply -f k8s/namesapce.yml
```

Or manually create the namespace:

```bash
kubectl create namespace tms-ns
```

Verify namespace creation:

```bash
kubectl get namespaces
```

### 2. Deploy Application

```bash
kubectl apply -f k8s/deployment.yml
```

Verify deployment:

```bash
kubectl get deployments -n tms-ns
kubectl get pods -n tms-ns
```

### 3. Deploy Service

```bash
kubectl apply -f k8s/service.yml
```

Verify service:

```bash
kubectl get services -n tms-ns
```

### 4. Access the Application

#### Port Forwarding

```bash
kubectl port-forward -n tms-ns service/task-management-service 5173:80
```

Access at: `http://localhost:5173`

#### Using LoadBalancer (if configured)

Get the external IP:

```bash
kubectl get svc -n tms-ns
```

#### Using NodePort (if configured)

```bash
kubectl get svc -n tms-ns -o wide
```

### 5. Scale Deployment

Scale up replicas:

```bash
kubectl scale deployment task-management-deployment -n tms-ns --replicas=3
```

Scale down replicas:

```bash
kubectl scale deployment task-management-deployment -n tms-ns --replicas=1
```

### 6. Update Deployment Image

Update the image version:

```bash
kubectl set image deployment/task-management-deployment \
  tms-container=sauravmishra07/ems:new-tag \
  -n tms-ns
```

### 7. Monitor Deployment

View logs:

```bash
kubectl logs -f deployment/task-management-deployment -n tms-ns
kubectl logs -f pod-name -n tms-ns
```

Describe deployment:

```bash
kubectl describe deployment task-management-deployment -n tms-ns
```

Watch pod status:

```bash
kubectl get pods -n tms-ns -w
```


## 📝 Available Commands

| Command | Description |
|---------|-------------|
| `npm install` | Install dependencies |
| `npm run dev` | Start development server |
| `npm run build` | Build for production |
| `npm run preview` | Preview production build locally |
| `docker build -t sauravmishra07/ems:tag .` | Build Docker image |
| `docker run --rm -p 5173:80 sauravmishra07/ems:tag` | Run Docker container |
| `kubectl apply -f k8s/namesapce.yml` | Create K8s namespace |
| `kubectl apply -f k8s/deployment.yml` | Deploy to K8s |
| `kubectl apply -f k8s/service.yml` | Create K8s service |
| `kubectl get pods -n tms-ns` | List pods in namespace |
| `kubectl logs -f deployment/task-management-deployment -n tms-ns` | View deployment logs |
| `kubectl port-forward -n tms-ns service/task-management-service 5173:80` | Port forward to access app |
| `kubectl delete -f k8s/ -n tms-ns` | Delete all K8s resources |

## 🔑 Login Credentials

**Admin:**
- Email: `admin@me.com`
- Password: `123`

**Employees:** Check localStorage data for employee credentials

## 📚 Project Structure

```
k8s-CICD/
├── src/
│   ├── components/
│   │   ├── Auth/
│   │   ├── Dashboard/
│   │   ├── TaskList/
│   │   └── other/
│   ├── context/
│   ├── utils/
│   └── App.jsx
├── k8s/
│   ├── deployment.yml
│   ├── service.yml
│   └── namesapce.yml
├── Dockerfile
├── package.json
└── vite.config.js
```

## 🔧 Environment Variables

Create a `.env` file if needed for environment-specific configurations

## 🐛 Troubleshooting

**Pod not starting:**
```bash
kubectl describe pod pod-name -n tms-ns
kubectl logs pod-name -n tms-ns
```

**Image pull errors:**
```bash
kubectl get events -n tms-ns
# Ensure Docker image is pushed to registry with correct tag
```

**Connection issues:**
```bash
kubectl port-forward -n tms-ns service/task-management-service 5173:80
```

## 📄 License

MIT

## 👨‍💻 Technology Stack

- **Frontend**: React, Vite, Tailwind CSS
- **Containerization**: Docker
- **Orchestration**: Kubernetes
- **Build Tool**: Vite
