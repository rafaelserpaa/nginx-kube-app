# Custom NGINX Deployment on Kubernetes with Kind

This repository contains the Kubernetes manifests required to deploy an NGINX application with a custom configuration (`nginx.conf`), persistent storage, and secret management.

The project utilizes:
* **Deployment**: Manages the NGINX application pods.
* **Service**: Exposes the NGINX application within the cluster (NodePort type).
* **ConfigMap**: Injects a custom `nginx.conf` file.
* **PersistentVolumeClaim (PVC)**: Persists data (e.g., logs or website content).
* **Secret**: Stores sensitive data (e.g., auth credentials, TLS keys).

## 🎯 Objective

The goal is to demonstrate a configurable and stateful NGINX deployment on Kubernetes, decoupling the configuration and data from the Pod's lifecycle.

## 🛠️ Components

The manifest files are located in the `k8s/` directory:

* `01-configmap.yaml`: Contains the custom `nginx.conf`.
* `02-pvc.yaml`: Requests persistent storage for NGINX data.
* `03-secret.yaml`: Stores sensitive data (e.g., username and password for basic auth).
* `04-deployment.yaml`: Defines the NGINX deployment, mounting the ConfigMap, PVC, and Secret.
* `05-service.yaml`: Exposes NGINX so it can be accessed.

### Prerequisites

You must have the following tools to test:
* [kubectl](https://kubernetes.io/docs/tasks/tools/)
* [Kind (Kubernetes in Docker)](https://kind.sigs.k8s.io/)
* [Docker](https://www.docker.com/products/docker-desktop/)

