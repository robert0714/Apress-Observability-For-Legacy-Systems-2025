# 2.6 Running the o11y Shop Demo
This [demo](https://github.com/Condla/web-shop-o11y-demo) explains how to develop, build, and troubleshoot the problem.
* Build microservices using OpenTelemetry manual and automated instrumentation.
* Use OpenSearch to collect metrics, logs, and traces and implement observability. Also, understand the correlation with OpenSearch.
* original site: https://github.com/Condla/web-shop-o11y-demo

## 2.6.4 Install the Demo
```bash
minikube config set driver docker
minikube config set driver virtualbox
minikube config set driver hyperv
minikube start --kubernetes-version v1.32.9 --memory=12000 --cpus=4
minikube config view
```