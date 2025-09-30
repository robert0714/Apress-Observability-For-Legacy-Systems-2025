# 1.3.5 Events
## 1.3.5.2 Example 2
```bash
minikube config set driver docker
minikube config set driver virtualbox
minikube config set driver hyperv
minikube start --kubernetes-version v1.32.9 --memory=12000 --cpus=4
minikube config view
```