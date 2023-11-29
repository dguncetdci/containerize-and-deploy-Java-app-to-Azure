
# Deploy the Container Image to Azure Kubernetes Service

## Overview
This guide will instruct you on deploying a container image to Azure Kubernetes Service (AKS).


## Steps

### 1. Reinitialize Environment (If Necessary)
If your session has idled out or you're starting from another CLI, reinitialize your environment variables and reauthenticate with Azure:
```bash
AZ_RESOURCE_GROUP=javacontainerizationdemorg
AZ_CONTAINER_REGISTRY=<YOUR_CONTAINER_REGISTRY>
AZ_KUBERNETES_CLUSTER=javacontainerizationdemoaks
AZ_LOCATION=<YOUR_AZURE_REGION>
AZ_KUBERNETES_CLUSTER_DNS_PREFIX=<YOUR_UNIQUE_DNS_PREFIX_TO_ACCESS_YOUR_AKS_CLUSTER>

az login
az acr login -n $AZ_CONTAINER_REGISTRY
```

### 2. Deploy the Container Image
Create a file called `deployment.yml` within your project root and add the necessary configurations.

#### Create the Deployment File
```bash
vi deployment.yml
```

#### Add Contents to Deployment File
Replace `<AZ_CONTAINER_REGISTRY>` with your actual Azure Container Registry.
```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: flightbookingsystemsample
spec:
  replicas: 1
  selector:
    matchLabels:
      app: flightbookingsystemsample
  template:
    metadata:
      labels:
        app: flightbookingsystemsample
    spec:
      containers:
      - name: flightbookingsystemsample
        image: <AZ_CONTAINER_REGISTRY>.azurecr.io/flightbookingsystemsample:latest
        resources:
          requests:
            cpu: "1"
            memory: "1Gi"
          limits:
            cpu: "2"
            memory: "2Gi"
        ports:
        - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
  name: flightbookingsystemsample
spec:
  type: LoadBalancer
  ports:
  - port: 8080
    targetPort: 8080
  selector:
    app: flightbookingsystemsample
```

### 3. Connect to Your Kubernetes Cluster
Install `kubectl` locally and configure it to connect to your AKS cluster.
```bash
az aks install-cli
az aks get-credentials --resource-group $AZ_RESOURCE_GROUP --name $AZ_KUBERNETES_CLUSTER
```

### 4. Apply Deployment Changes
Apply the changes in your `deployment.yml` file to the AKS cluster.
```bash
kubectl apply -f deployment.yml
```

### 5. Monitor the Deployment Status
Check the status of your deployment.
```bash
kubectl get all
```

If your POD status is Running, the app should be accessible.

### 6. Access the Deployed App
Use the EXTERNAL-IP from your `kubectl get services flightbookingsystemsample` command to access the running app in AKS.

---

Your application is now successfully deployed to Azure Kubernetes Service and can be accessed externally.
