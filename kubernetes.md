# Kubernetes

```yml
██   ██  █████  ███████ 
██  ██  ██   ██ ██      
█████    █████  ███████ 
██  ██  ██   ██      ██ 
██   ██  █████  ███████ 
```

## install

- `curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"` to download kubectl binary
- `curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"` to verify the kubectl binary against the checksum file
- `echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check` to validate binary
- `sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl`
- `kubectl version --client`

## install minikube

- `curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb`
- `sudo dpkg -i minikube_latest_amd64.deb`

## create a cluster

- `minikube start`
- `kubectl cluster-info`
- `kubectl create deployment nodeapplication2 --image=node:16-alpine`
- `kubectl get deployments`
