# Custom NGINX Deployment on Kubernetes with Kind

This repository contains the Kubernetes manifests required to deploy an NGINX application with a custom configuration (`nginx.conf`), persistent storage, and secret management.

The project utilizes:
* **Deployment**: Manages the NGINX application pods.
* **Service**: Exposes the NGINX application within the cluster (NodePort type).
* **ConfigMap**: Injects a custom `nginx.conf` file.
* **Secret**: Stores sensitive data as environment variables.
* **StorageClass**: Defines a storage type for dynamic volume provisioning.
* **PersistentVolumeClaim (PVC)**: Requests persistent storage for NGINX logs.

## 🎯 Objective

The goal is to demonstrate a configurable and stateful NGINX deployment on Kubernetes, decoupling the configuration and data from the Pod's lifecycle.

## 🛠️ Components

The manifest files are located in the `k8s/` directory:

*   `k8s/configmap.yaml`: Contains the custom `nginx.conf`.
*   `k8s/secret.yaml`: Stores sensitive data (e.g., `APP_USER`, `APP_PASSWORD`).
*   `k8s/deployment.yaml`: Defines the NGINX deployment, mounting the ConfigMap, PVC, and Secret.
*   `k8s/service.yaml`: Exposes NGINX so it can be accessed from outside the cluster.
*   `k8s/storage/default-storage.yaml`: Defines a `StorageClass` for local path provisioning, required by Kind.
*   `k8s/storage/pvc.yaml`: Requests persistent storage for NGINX logs.

### Prerequisites

You must have the following tools installed:
* [kubectl](https://kubernetes.io/docs/tasks/tools/)
* [Kind (Kubernetes in Docker)](https://kind.sigs.k8s.io/)
* [Docker](https://www.docker.com/products/docker-desktop/)

## 🚀 Deployment Steps

1.  **Create a Kind Cluster:**
    If you don't have a cluster running, create one:
    ```sh
    kind create cluster
    ```


2.  **Apply the Manifests:**
    Apply all the application components. You can apply all files in a directory at once.
    ```sh
    # Apply storage components first
    kubectl apply -f k8s/storage/

    # Apply the rest of the application
    kubectl apply -f k8s/
    ```


3.  **Verify the Deployment:**
    Check that the pod is running:
    ```sh
    kubectl get pods
    # NAME                                READY   STATUS    RESTARTS   AGE
    # nginx-deployment-xxxxxxxxxx-xxxxx   1/1     Running   0          ...
    ```


## 🌐 Accessing the Application

1.  **Forward a Port to the Service:**
    To access the NGINX service from your local machine, forward a local port to the service port.
    ```sh
    kubectl port-forward service/nginx-service 8080:80
    ```
    You can now access the application by navigating to `http://localhost:8080` in your browser.

2.  **Access NGINX:**
    Open your browser and navigate to `http://localhost:8080`. You should see the default NGINX welcome page.

## 🧹 Cleanup

To delete the application resources from your cluster, run:
```sh
kubectl delete -f k8s/
kubectl delete -f k8s/storage/
```

To delete the entire Kind cluster, run:
```sh
kind delete cluster
```
