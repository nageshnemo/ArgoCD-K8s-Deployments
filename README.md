# ArgoCD-K8s-Deployments

Sure! Here’s a structured `README.md` file for your GitHub project:

```markdown
# GCP Project Setup and Argo CD Installation

## Prerequisites

1. Google Cloud Platform (GCP) Account: Make sure you have a GCP account.
2. kubectl: Ensure you have kubectl installed and configured to interact with your GKE cluster.
3. jq: Install `jq` for JSON processing, if not already installed.

## Steps to Set Up

### 1. Create a GCP Project

- Create a new GCP project and enable billing for it.

### 2. Create a GKE Cluster

- Follow GCP's documentation to create a Google Kubernetes Engine (GKE) cluster within your project.

### 3. Connect to Your Cluster

```bash
gcloud container clusters get-credentials [YOUR_CLUSTER_NAME] --zone [YOUR_ZONE] --project [YOUR_PROJECT_ID]
```
![image](https://github.com/user-attachments/assets/3f31d8c7-13f9-4f36-8f6e-2b71ed06ed5c)

### 4. Install Argo CD

1. **Create the Argo CD namespace**:

    ```bash
    kubectl create namespace argocd
    ```

2. **Apply the Argo CD installation manifest**:

    ```bash
    kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
    ```

### 5. Download Argo CD CLI

- Download the latest Argo CD CLI version from the [Argo CD Releases](https://github.com/argoproj/argo-cd/releases/latest).

- For Mac, Linux, and WSL, you can use Homebrew:

    ```bash
    brew install argocd
    ```

### 6. Access the Argo CD API Server

By default, the Argo CD API server is not exposed with an external IP. You can choose one of the following methods to access it:

#### Service Type LoadBalancer

- Change the `argocd-server` service type to `LoadBalancer`:

    ```bash
    kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
    ```

#### Port Forwarding

- Alternatively, use kubectl port-forwarding to connect to the API server without exposing the service:

    ```bash
    kubectl port-forward svc/argocd-server -n argocd 8080:443
    ```

### 7. Get the Argo CD Admin Credentials

To access the Argo CD UI, retrieve the initial admin username and password:

```bash
kubectl get secret -n argocd
```

- The output will show `argocd-initial-admin-secret`. To get the password, run:

```bash
kubectl get secret argocd-initial-admin-secret -n argocd -o json | jq -r .data.password | base64 --decode
```

### 8. Connect to Your GitHub Repository

- Add your GitHub repository URL:

    ```
    https://github.com/nageshnemo/ArgoCD-K8s-Deployments.git
    ```

### 9. Create a New Application in Argo CD

1. Go to the Applications section in Argo CD.
2. Create a new application.
3. Set the sync policy to **Automatic** to automatically deploy changes once the Git branch is updated.
4. Provide the Kubernetes URL and path, and your GitHub repository URL.
5. Ensure the sync is enabled for all changes to be deployed.

### 10. Change the Image to Test Sync

- Modify the image in your Kubernetes deployment to see the sync in action between Argo CD and your deployment.

## Conclusion

You’ve successfully set up a GCP project, created a GKE cluster, installed Argo CD, and connected it to your GitHub repository. Enjoy managing your Kubernetes deployments with Argo CD!
```

