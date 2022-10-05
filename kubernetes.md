# Kubernetes

```yml
██   ██  █████  ███████ 
██  ██  ██   ██ ██      
█████    █████  ███████ 
██  ██  ██   ██      ██ 
██   ██  █████  ███████ 
```

## install

- `curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"` to download kubectl
- `curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"` to verify the kubectl binary against the checksum file
- `sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl`
