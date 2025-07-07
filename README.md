
# 🚀 Argo CD Simple NGINX Deployment

This repository demonstrates a basic GitOps setup using **Argo CD** to deploy a simple NGINX web application to a Kubernetes cluster.

---

## 🎯 What This Project Does

- Deploys a basic NGINX app using Kubernetes manifests
- Stores deployment configuration in Git
- Uses **Argo CD** to sync and deploy the app automatically or manually
- Enables GitOps-style application delivery

---

## 📁 Folder Structure

```

argo-cd-simple-app/
└── manifests/
├── deployment.yaml         # NGINX Deployment
├── service.yaml            # ClusterIP Service
└── kustomization.yaml      # Kustomize file (optional but Argo CD-friendly)

````

---

## ⚙️ Prerequisites

- A Kubernetes cluster (e.g. Minikube, KinD, EKS, GKE, etc.)
- Argo CD installed on the cluster  
  👉 [Install Guide](https://argo-cd.readthedocs.io/en/stable/getting_started/)
- Access to the Argo CD Web UI or CLI

---

## 🛠 How to Use This Project

### 1. Fork or clone this repo

```bash
git clone https://github.com/your-username/argo-cd-simple-app.git
````

### 2. Create an Application in Argo CD

Go to the Argo CD Web UI and click **NEW APP**, then fill in the following:

| Field                | Value                                                                          |
| -------------------- | ------------------------------------------------------------------------------ |
| **Application Name** | `simple-nginx`                                                                 |
| **Project**          | `default`                                                                      |
| **Sync Policy**      | Manual or Auto                                                                 |
| **Repository URL**   | Your Git repo URL (e.g. `https://github.com/your-username/argo-cd-simple-app`) |
| **Revision**         | `HEAD`                                                                         |
| **Path**             | `manifests`                                                                    |
| **Cluster**          | `https://kubernetes.default.svc` (in-cluster)                                  |
| **Namespace**        | `default`                                                                      |

### 3. Sync the App

* Click **Sync** in the Argo CD UI to deploy the app.
* Alternatively, use the Argo CD CLI:

```bash
argocd app sync simple-nginx
```

---

## 📦 Application Details

* **App Name**: `simple-nginx`
* **Image**: `nginx:alpine`
* **Port**: `80`
* **Service Type**: `ClusterIP` (internal access only)

To access it from outside, expose it via `NodePort`, `Ingress`, or `Port Forward`:

```bash
kubectl port-forward svc/simple-nginx 8080:80
```

Then open: [http://localhost:8080](http://localhost:8080)

---

## 📄 Files Overview

### `deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: simple-nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: simple-nginx
  template:
    metadata:
      labels:
        app: simple-nginx
    spec:
      containers:
      - name: nginx
        image: nginx:alpine
        ports:
        - containerPort: 80
```

### `service.yaml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: simple-nginx
spec:
  selector:
    app: simple-nginx
  ports:
  - port: 80
    targetPort: 80
  type: ClusterIP
```

### `kustomization.yaml`

```yaml
resources:
  - deployment.yaml
  - service.yaml
```

---

## 💡 What to Try Next

* Replace NGINX with your own container image
* Change `ClusterIP` to `NodePort` or set up an Ingress
* Use Helm instead of raw YAML
* Add Argo CD auto-sync, health checks, or app of apps pattern

---

## 🙌 Why This Matters

This is a great hands-on introduction to:

* 🚀 GitOps workflows with Argo CD
* 🔄 Continuous delivery to Kubernetes
* 🧩 Modular infrastructure with Kustomize or Helm

---

**Happy deploying with GitOps!** 🔁

```

---

Let me know if you want to make this project **Helm-based**, add **auto-sync**, or include a **CI pipeline** that updates the image tag automatically.
```
