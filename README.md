# Kubernetes Assessment – Minikube and Kubernetes

## Project Overview

This project demonstrates Kubernetes installation, deployment management, networking, configuration management, persistent storage, replica scaling, rolling updates, and rollback using Minikube and kubectl.

## Tools and Technologies

- Ubuntu Linux
- Docker
- Minikube
- Kubernetes
- kubectl
- Nginx
- YAML
- Git and GitHub

## Task 1: Install Minikube, Kubernetes and kubectl

**Objective:** Install and configure a local Kubernetes environment.

**Commands used:**

```bash
sudo apt update
sudo apt install -y docker.io curl wget
sudo systemctl enable docker
sudo systemctl start docker

minikube start --driver=docker
minikube status
kubectl get nodes
kubectl get pods -A
```

**Expected Result:**
- Minikube cluster started successfully.
- Kubernetes node status is Ready.
- Kubernetes system Pods are running.

## Task 2: Create Namespace and Deploy Nginx

**Objective:** Create a Namespace and deploy an Nginx application using YAML.

**Files:**
- namespace.yaml
- nginx-deployment.yaml

**Commands:**

```bash
kubectl apply -f namespace.yaml
kubectl get namespaces

kubectl apply -f nginx-deployment.yaml
kubectl get deployments -n devops
kubectl get pods -n devops
```

**Expected Result:**
- Namespace: devops
- Deployment: nginx-deployment
- Replicas: 2
- Nginx Pods: Running

## Task 3: Expose Deployment Using Service

**Objective:** Expose Nginx using ClusterIP and NodePort Services.

**Files:**
- nginx-clusterip.yaml
- nginx-nodeport.yaml

**Commands:**

```bash
kubectl apply -f nginx-clusterip.yaml
kubectl apply -f nginx-nodeport.yaml

kubectl get svc -n devops
minikube service nginx-nodeport -n devops --url
```

**Expected Result:**
- ClusterIP Service created.
- NodePort Service created.
- Nginx application accessible through the NodePort URL.

NodePort: 30080

## Task 4: Create ConfigMap and Secret

**Objective:** Store application configuration and sensitive information using ConfigMap and Secret.

**Files:**
- configmap.yaml
- secret.yaml
- nginx-deployment.yaml

**Commands:**

```bash
kubectl apply -f configmap.yaml
kubectl apply -f secret.yaml
kubectl apply -f nginx-deployment.yaml

kubectl get configmap -n devops
kubectl get secrets -n devops
kubectl get pods -n devops
```

**Verify environment variables:**

```bash
kubectl exec -it POD_NAME -n devops -- printenv APP_NAME
kubectl exec -it POD_NAME -n devops -- printenv APP_ENV
kubectl exec -it POD_NAME -n devops -- printenv DB_USERNAME
```

**Expected Result:**
- ConfigMap created successfully.
- Secret created successfully.
- Environment variables available inside Nginx containers.

Note: Never upload real passwords or credentials to a public GitHub repository.

## Task 5: PersistentVolume and PersistentVolumeClaim

**Objective:** Create persistent storage and mount it inside the Nginx container.

**Files:**
- pv.yaml
- pvc.yaml
- nginx-deployment.yaml

**Commands:**

```bash
kubectl apply -f pv.yaml
kubectl apply -f pvc.yaml
kubectl apply -f nginx-deployment.yaml

kubectl get pv
kubectl get pvc -n devops
```

**Test persistent storage:**

```bash
kubectl exec -it POD_NAME -n devops -- sh
```

Inside the container:

```bash
echo "Kubernetes Persistent Storage Working" > /usr/share/nginx/html/data/test.txt

cat /usr/share/nginx/html/data/test.txt
exit
```

**Expected Result:**
- PersistentVolume created.
- PersistentVolumeClaim Bound.
- Storage mounted inside the Nginx container.
- Test data remains available after Pod replacement.

## Task 6: Deployment YAML and Scaling Replicas

**Objective:** Deploy an application and scale the number of replicas.

**File:**
- scaling-deployment.yaml

**Commands:**

```bash
kubectl apply -f scaling-deployment.yaml

kubectl get deployments -n devops
kubectl get pods -n devops
```

**Scale from 3 to 5 replicas:**

```bash
kubectl scale deployment nginx-scaling --replicas=5 -n devops
kubectl get deployment nginx-scaling -n devops
```

**Scale from 5 to 2 replicas:**

```bash
kubectl scale deployment nginx-scaling --replicas=2 -n devops
kubectl get deployment nginx-scaling -n devops
```

**Expected Result:**
- Initial replicas: 3
- Scaled replicas: 5
- Final replicas: 2
- Deployment available: 2/2

## Task 7: Rolling Update and Rollback

**Objective:** Update the Nginx container image and perform rollback.

**Rolling Update:**

```bash
kubectl set image deployment/nginx-scaling nginx=nginx:1.27 -n devops

kubectl rollout status deployment/nginx-scaling -n devops
```

**Check rollout history:**

```bash
kubectl rollout history deployment/nginx-scaling -n devops
```

**Perform Rollback:**

```bash
kubectl rollout undo deployment/nginx-scaling -n devops
```

**Verify rollback:**

```bash
kubectl rollout status deployment/nginx-scaling -n devops

kubectl get deployment nginx-scaling -n devops -o wide
kubectl get pods -n devops
```

**Expected Result:**
- Rolling update completed successfully.
- Nginx image updated to version 1.27.
- Rollback completed successfully.
- Previous image version restored.

## Task 8: Final Verification

**Objective:** Verify all Kubernetes resources.

**Check Pods:**

```bash
kubectl get pods -n devops
```

**Check Services:**

```bash
kubectl get svc -n devops
```

**Check Deployments:**

```bash
kubectl get deployments -n devops
```

**Check PersistentVolume:**

```bash
kubectl get pv
```

**Check PersistentVolumeClaim:**

```bash
kubectl get pvc -n devops
```

**Check ConfigMap:**

```bash
kubectl get configmap -n devops
```

**Check Secrets:**

```bash
kubectl get secrets -n devops
```

## Project Folder Structure

```text
kubernetes-assessment/
│
├── namespace.yaml
├── nginx-deployment.yaml
├── nginx-clusterip.yaml
├── nginx-nodeport.yaml
├── configmap.yaml
├── secret.yaml
├── pv.yaml
├── pvc.yaml
├── scaling-deployment.yaml
├── README.md
│
└── screenshots/
    ├── task-1.png
    ├── task-2.png
    ├── task-3.png
    ├── task-4.png
    ├── task-5.png
    ├── task-6.png
    ├── task-7.png
    └── task-8.png
```

## Final Outcome

Successfully practiced the following Kubernetes concepts:

1. Minikube and kubectl installation.
2. Namespace creation.
3. Nginx Deployment.
4. ClusterIP and NodePort Services.
5. ConfigMap and Secret management.
6. PersistentVolume and PersistentVolumeClaim.
7. Deployment scaling.
8. Rolling Update and Rollback.
9. Kubernetes resource verification.

## Conclusion

This project demonstrates the practical implementation of Kubernetes concepts using Minikube on Ubuntu Linux. All eight assessment tasks were practiced to understand container orchestration, application deployment, networking, configuration, storage, and deployment management.

**Project Status:** Kubernetes Assessment – Completed

**Platform:** Ubuntu Linux | Minikube | Kubernetes | Docker
