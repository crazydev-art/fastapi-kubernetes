
```markdown
# FastAPI Kubernetes Deployment

This repository contains all the necessary files to deploy a FastAPI application with MySQL using Kubernetes. The solution includes:
- A FastAPI application.
- A MySQL database configured using StatefulSet.
- Kubernetes manifests to manage deployments and services.

---

## Prerequisites

1. **Kubernetes Cluster**: Ensure you have a running Kubernetes cluster (e.g., Minikube, K3s, or cloud-managed).
2. **kubectl**: Kubernetes command-line tool installed and configured to access your cluster.
3. **Docker**: Installed and configured to build and push images.
4. **Docker Hub Account**: For storing the Docker image.

---

## Deployment Steps

### 1. Clone the Repository
```bash
git clone https://github.com/<your-username>/fastapi-kubernetes.git
cd fastapi-kubernetes
```

### 2. Build and Push Docker Image
1. Navigate to the FastAPI application directory:
   ```bash
   cd test/k8s-eval-fastapi
   ```

2. Build the Docker image:
   ```bash
   docker build -t <your-dockerhub-username>/fastapi:latest .
   ```

3. Push the Docker image to Docker Hub:
   ```bash
   docker push <your-dockerhub-username>/fastapi:latest
   ```

---

### 3. Deploy MySQL Database

1. Create the required Kubernetes secrets for MySQL:
   ```bash
   kubectl create secret generic mysql-user -n eval \
       --from-literal=user=<your-mysql-username> \
       --from-literal=password=<your-mysql-password> \
       --from-literal=database=<your-database-name>
   ```

2. Apply the MySQL manifests:
   ```bash
   kubectl apply -f mysql/mysql-local-data-folder-pv.yaml
   kubectl apply -f mysql/statefulset.yaml
   kubectl apply -f mysql/mysql-service.yaml
   ```

3. Verify the MySQL pod is running:
   ```bash
   kubectl get pods -n eval
   ```

---

### 4. Deploy the FastAPI Application

1. Apply the FastAPI Deployment and Service:
   ```bash
   kubectl apply -f test/k8s-eval-fastapi/fastapi-deployment.yaml
   kubectl apply -f test/k8s-eval-fastapi/fastapi-service.yaml
   ```

2. Verify the FastAPI pods are running:
   ```bash
   kubectl get pods -n eval
   ```

3. Check the exposed service:
   ```bash
   kubectl get svc -n eval
   ```

---

### 5. Access the FastAPI Application

1. Get the IP address of your Kubernetes node:
   ```bash
   kubectl get nodes -o wide
   ```

2. Use the NodePort to access the application:
   ```bash
   curl http://<Node-IP>:30000
   ```

---

### 6. Testing and Logs

1. View logs of the FastAPI pod:
   ```bash
   kubectl logs <fastapi-pod-name> -n eval
   ```

2. Check logs for debugging:
   ```bash
   kubectl logs <mysql-pod-name> -n eval
   ```

---

### 7. Clean Up

To delete the resources:
```bash
kubectl delete -f mysql/
kubectl delete -f test/k8s-eval-fastapi/
```

---

## Repository Structure

```plaintext
fastapi-kubernetes/
├── mysql/
│   ├── mysql-local-data-folder-pv.yaml    # Persistent Volume for MySQL
│   ├── mysql-service.yaml                 # MySQL Service
│   ├── statefulset.yaml                   # MySQL StatefulSet
├── test/k8s-eval-fastapi/
│   ├── app/                               # FastAPI source code
│   │   └── main.py                        # FastAPI application entry point
│   ├── Dockerfile                         # Dockerfile to build the FastAPI image
│   ├── fastapi-deployment.yaml            # Deployment for FastAPI
│   ├── fastapi-service.yaml               # Service for FastAPI
│   ├── requirements.txt                   # Python dependencies
└── README.md                              # Project documentation
```

---

## Authors
- **Your Name**: [GitHub Profile](https://github.com/<your-username>)
```

You can edit `<your-dockerhub-username>`, `<your-mysql-username>`, `<your-mysql-password>`, and `<your-database-name>` with your actual values. Additionally, replace `<your-username>` with your GitHub username.
