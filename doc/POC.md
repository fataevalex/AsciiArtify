
# Proof of Concept: GitOps with ArgoCD

## 🎯 Objective
Demonstrate the technical feasibility of deploying a GitOps system based on Kubernetes and ArgoCD as a foundation for building the MVP of the **AsciiArtify** project.

---

## ⚙️ Technologies Used

- **Kubernetes (k3d)** — Lightweight distribution for quick cluster setup.
- **ArgoCD** — Tool for implementing GitOps-based application delivery.
- **kubectl** — CLI for managing Kubernetes clusters.
- **Linux/GitHub Codespace** — Environment for local or cloud-based deployment.

---

## 🔧 Implementation Steps

### 1. Prepare the Environment
Install essential tools:
```bash
sudo apt install -y curl git
```

Install `kubectl` and `k9s`:
```bash
curl -LO "https://dl.k8s.io/release/$(curl -s https://cdn.dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl && sudo mv kubectl /usr/local/bin/
wget https://github.com/derailed/k9s/releases/download/v0.32.5/k9s_linux_amd64.deb
sudo apt install ./k9s_linux_amd64.deb
```

Install Kubernetes (k3d):
```bash
curl -s https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash
k3d cluster create asciicluster"
```

Verify installation:
```bash
export KUBECONFIG="$(k3d kubeconfig write asciicluster)"
kubectl get nodes
```

---

### 2. Install ArgoCD
```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl get pods -n argocd -w
```

---

### 3. Access the ArgoCD UI

#### Retrieve the initial admin password:
```bash
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d
```

#### Port-forward:
```bash
kubectl port-forward svc/argocd-server -n argocd 8443:443
```

#### Login:
Open [https://localhost:8443](https://localhost:8443)  or in your browser  
Username: `admin`  
Password: `<decoded password>`

---


### 5. Validate Functionality

- ArgoCD UI is accessible.
- Git repository can be connected.
- ArgoCD Application can be created.
- Sync status and updates are functional.

---

## ✅ Result

- Kubernetes cluster is up and running.
- ArgoCD is installed and configured.
- UI access is available for the team.
- Ready for MVP integration using GitOps.

---

## 📝 Next Steps

- Add MVP Helm chart to a Git repository.
- Configure automated deployment via ArgoCD.
- Prepare production infrastructure based on the PoC setup.