# 10.3.2.2 Prometheus Adapter Demo
```bash
minikube config set driver docker
minikube config set driver virtualbox
minikube config set driver hyperv
minikube start --kubernetes-version v1.32.9 --memory=12000 --cpus=4
minikube config view
```
Use the `monitoring` namespace:
```bash
kubectl create namespace monitoring
```
Install the operator:
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm upgrade --install prometheus-stack prometheus-community/kube-prometheus-stack \
      --namespace monitoring --create-namespace       
```
Check the installed pods:
```bash
kubectl -n monitoring get pods
NAME                                                     READY   STATUS              RESTARTS   AGE
alertmanager-prometheus-stack-kube-prom-alertmanager-0   0/2     Init:0/1            0          8s
prometheus-prometheus-stack-kube-prom-prometheus-0       0/2     Init:0/1            0          8s
prometheus-stack-grafana-7c655db89f-6s8z5                0/3     ContainerCreating   0          45s
prometheus-stack-kube-prom-operator-6598c96bc7-zcbqh     1/1     Running             0          45s
prometheus-stack-kube-state-metrics-6c6c689fb-qltc8      1/1     Running             0          45s
prometheus-stack-prometheus-node-exporter-ljk5s          1/1     Running             0          45s
```
Configure port forwarding:
```bash
kubectl port-forward svc/prometheus-operated  9090:9090 --namespace monitoring
```
kube-prometheus-stack has been installed. Check its status by running:
```bash
  kubectl --namespace monitoring get pods -l "release=prometheus-stack"
```
Get Grafana 'admin' user password by running:
```bash
  kubectl --namespace monitoring get secrets prometheus-stack-grafana -o jsonpath="{.data.admin-password}" | base64 -d ; echo
```
Access Grafana local instance:
```bash
  export POD_NAME=$(kubectl --namespace monitoring get pod -l "app.kubernetes.io/name=grafana,app.kubernetes.io/instance=prometheus-stack" -oname)
  kubectl --namespace monitoring port-forward $POD_NAME 3000
```  



Use the custom-metrics namespace:
```bash
kubectl create namespace custom-metrics
```
You can retrieve registered custom metrics using the following commands:
```bash
kubectl get --raw "/apis/custom.metrics.k8s.io/v1beta1/" | jq | grep pods/
```