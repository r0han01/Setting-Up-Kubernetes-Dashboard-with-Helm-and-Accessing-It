# Setting Up Kubernetes Dashboard with Helm and Accessing It

This guide provides a step-by-step approach to install, configure, and access the Kubernetes Dashboard using Helm. It includes common troubleshooting tips and commands to ensure smooth installation and usage.

## Prerequisites
- A working Kubernetes cluster (e.g., Minikube or any Kubernetes setup).
- `kubectl` installed and configured to interact with your cluster.
- `helm` installed. If not, follow the installation steps below.

## Installing Helm
There are two options for installing Helm:

### Option 1: Install Helm via Snap
```bash
sudo snap install helm --classic
```
## Installing Helm and Kubernetes Dashboard

### Step 1: Install Helm via Script
Run the following command to install Helm:
```bash
curl -fsSL https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

### Step 2: Verify Installation
To verify that Helm was successfully installed, run:
```bash
helm version
```
## Step 3: Deploying Kubernetes Dashboard

### Step 1: Add Kubernetes Dashboard Repository
Add the Kubernetes Dashboard repository to Helm:
```bash
helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard/
```

## Step 2: Install the Kubernetes Dashboard
Install the Kubernetes Dashboard using Helm:

```bash
helm upgrade --install kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard --create-n
```

### Step 4: Accessing Kubernetes Dashboard
Option 1: Using Port-Forwarding
Run the following command to port-forward the Kubernetes Dashboard service:

```bash
kubectl -n kubernetes-dashboard port-forward svc/kubernetes-dashboard-kong-proxy 8443:443
```
### Step 2: Open the Dashboard in Browser
Open your browser and navigate to:
```bash
https://localhost:8443
```

### Authentication: Generating a Bearer Token
List Service Accounts:
```bash
kubectl -n kubernetes-dashboard get serviceaccounts
```
Generate a Bearer Token:

```bash
kubectl -n kubernetes-dashboard create token kubernetes-dashboard
```
Copy and Paste the Token: Copy the generated token from the output. Paste it into the Bearer Token field when accessing the Dashboard.

### Troubleshooting
Problem 1: Services "kubernetes-dashboard" Not Found
Verify if the dashboard resources are deployed:

```bash
kubectl get all -n kubernetes-dashboard
```
If no resources are found, redeploy the dashboard:

```bash
helm upgrade --install kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard --create-namespace --namespace kubernetes-dashboard
```
###Problem 2: Container Creating / ImagePullBackOff
Check the status of pods:

```bash
kubectl get pods -n kubernetes-dashboard
```
Inspect pod logs for errors:

```bash
kubectl logs -n kubernetes-dashboard <pod-name>
```
###Problem 3: Proxy Issues
Start the proxy:

```bash
kubectl proxy
```
Open the dashboard URL:

```bash
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
```
Reinstalling the Dashboard
Uninstall the existing deployment:

```bash
helm uninstall kubernetes-dashboard -n kubernetes-dashboard
```
Reinstall the dashboard:

```bash
helm upgrade --install kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard --create-namespace --namespace kubernetes-dashboard
```
### Summary
- Install Helm to manage Kubernetes packages.

- Deploy Kubernetes Dashboard with Helm.

- Access the Dashboard via port-forwarding.

- Authenticate using a Bearer Token.

- Troubleshoot common issues like missing services, pod errors, or proxy issues.

- ![ScreenShot Tool -20241214092753](https://github.com/user-attachments/assets/c7270c37-706e-4c98-87d7-a61aec7d35e5)
