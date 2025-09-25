# 2.4.2 Configure TNS
You need to deploy the instrumented three-layer (data layer, backend layer, and frontend layer) application to a Kubernetes cluster and monitor it. Then, a dashboard will be deployed to a Grafana instance for the visualization of performance metrics.

Next, you install and configure the demo application. The specific steps are as follows.

Launch Minikube:
```bash
minikube config set driver docker
minikube config set driver virtualbox
minikube config set driver hyperv
minikube start --mount --mount-string="$(pwd):/home/minikube" --kubernetes-version v1.32.9 --memory=12000 --cpus=2
minikube config view
```
Add a chart:
```bash
helm repo add grafana https://grafana.github.io/helm-charts
```
Install Tempo:
```bash
kubectl create namespace monitoring
helm -n monitoring upgrade --install tempo grafana/tempo

# 創建 ConfigMap（包含 prometheus.yml）
kubectl apply -f prometheus-configmap.yaml

# 創建 PVC
kubectl apply -f prometheus-claim0-persistentvolumeclaim.yaml

# 部署 Prometheus
kubectl apply -f prometheus-service.yaml
kubectl apply -f prometheus-deployment.yaml
 
```
   * test
     ```bash
     kubectl port-forward -n monitoring service/prometheus 9090:9090
     ```
     In browser: `http://localhost:9090`
Install Loki:
```bash
kubectl apply -f loki-service.yaml
kubectl apply -f loki-deployment.yaml
kubectl port-forward   service/loki 3100:3100
```
Install Promtail. Save your values.yaml, as shown here:
```yaml
config:
  clients:
    - url: http://loki:3100/loki/api/v1/push
```

Execute this helm command:
```bash
helm  upgrade --install promtail grafana/promtail

kubectl get ds
NAME       DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
promtail   1         1         1       1            1           <none>          12m
```


Install Grafana. The version is 8.5.4. This is not a good way to install it. For example, it needs to be converted from YAML to `configmap`, or from YAML to Helm charts. Conversion is easy with open source tools, but it takes time.

```bash
kubectl -n monitoring  create configmap grafana-datasources --from-file=datasources.yaml
kubectl apply -f grafana-claim2-persistentvolumeclaim.yaml
kubectl apply -f grafana-claim1-persistentvolumeclaim.yaml  
kubectl apply -f grafana-service.yaml
kubectl apply -f grafana-deployment.yaml

```
Install the application:
```bash
kubectl port-forward -n monitoring service/grafana 8080:3000

```
In your web browser, go to `https://localhost:8080` to see the demo, the load generator. You can navigate to the Grafana dashboard to query the application logs, visualize metrics, and inspect trace.

Kubernetes writes the log file to the `/var/log/` folder.