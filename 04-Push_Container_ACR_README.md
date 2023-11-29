
# Push the Container Image to Azure Container Registry

## Overview
pushing a container image to Azure Container Registry (ACR).


## Steps

### 1. Reinitialize Environment (If Necessary)
If your session has idled out, reinitialize your environment variables and reauthenticate with Azure:
```bash
AZ_RESOURCE_GROUP=javacontainerizationdemorg
AZ_CONTAINER_REGISTRY=<YOUR_CONTAINER_REGISTRY>
AZ_KUBERNETES_CLUSTER=javacontainerizationdemoaks
AZ_LOCATION=<YOUR_AZURE_REGION>
AZ_KUBERNETES_CLUSTER_DNS_PREFIX=<YOUR_UNIQUE_DNS_PREFIX_TO_ACCESS_YOUR_AKS_CLUSTER>

az login
az acr login -n $AZ_CONTAINER_REGISTRY
```

### 2. Push the Container Image
#### Sign in to Azure Container Registry
```bash
az acr login
```

#### Tag the Built Container Image
```bash
docker tag flightbookingsystemsample $AZ_CONTAINER_REGISTRY.azurecr.io/flightbookingsystemsample
```

#### Push the Image to Azure Container Registry
```bash
docker push $AZ_CONTAINER_REGISTRY.azurecr.io/flightbookingsystemsample
```

### 3. Verify the Pushed Image
Run the following command to view the image metadata in Azure Container Registry:
```bash
az acr repository show -n $AZ_CONTAINER_REGISTRY --image flightbookingsystemsample:latest
```

You will see output showing the image attributes including its digest and creation time.

---

Your container image is now in the Azure Container Registry, ready for deployment by Azure services such as Azure Kubernetes Service.
