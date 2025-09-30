# 10.3.2.3 KEDA
```bash
minikube config set driver docker
minikube config set driver virtualbox
minikube config set driver hyperv
minikube start --kubernetes-version v1.32.9 --memory=12000 --cpus=4
minikube config view
```
Install KEDA (https://artifacthub.io/packages/helm/kedacore/keda)
```bash
helm repo add kedacore https://kedacore.github.io/charts
helm repo update

kubectl create namespace keda
helm install keda kedacore/keda --namespace keda --version 2.17.2
```
When you try to access the sample-app, you’ll see the http_requests_total metric count increase.
Check the `ScaleObject` and `PodToscaler`:
```bash
$ kubectl apply -f scale.yaml
scaledobject.keda.sh/sample-app created

$ kubectl get scaledobject
name scaletargetkind scaletargetname min max triggers authentication ready active fallback age
sample-app apps/v1.Deployment sample-app 1 5 prometheus True True False 211d
```

Identify autoscaled applications. After a certain number of pods, the number of pods starts to increase.
```bash
$ kubectl get hpa
name reference targets minpods maxpods replicas age
keda-hpa-sample-app Deployment/sample-app 44667m/5k (avg) 1 5 3 21m

$ kubectl get pod
name ready status restarts age
prometheus-alertmanager-58d64b84db-jv4dk 2/2 Running 0 128m
prometheus-kube-state-metrics-5547d95bd-htz9r 1/1 Running 0 128m
prometheus-node-exporter-7s2lb 1/1 Running 0 128m
prometheus-operator-6bf9dd7f76-sq565 1/1 Running 2 211d
prometheus-pushgateway-85679964b8-s6q46 1/1 Running 0 128m
prometheus-server-6bfb6b68-kzfv5 2/2 Running 0 128m
sample-app-7cfb596f98-h4ww4 1/1 Running 1 211d
sample-app-7cfb596f98-lfpqr 1/1 Running 0 97m
sample-app-7cfb596f98-nwj44 1/1 Running 0 111m
```