# 1.3.3 Profiles
   ```bash 
   go mod init  
   go mod tidy
    
   go build ./cmd 
   ```
# 1.3.3.3 HotROD Profiles
So far, I have only described profiles and have not explained how to use them in terms of correlation with traces and overall root cause analysis. With thousands of microservices and lots of legacy, understanding the full context and pinpointing exactly what to profile is a difficult process.

* HotROD has some problems:
* Sequential processes

Managing thread pools

I identified these problems with traces from HotROD. I looked at how to reduce latency and improve performance by changing from sequential to parallel processing, changing the thread pool set in the configuration parameters, and fixing concurrency problems.

You need to understand the sequential processes, the limited number of thread pools, the connection timeouts, and the retries. With the visualization of spans in tracing, it is limited for root cause analysis because it is difficult to analyze resources such as threads and memory. If you suspect inefficient code, or if the problem is resource-related, you should use profiles.

Let’s configure the demo.
```bash
minikube config set driver docker
minikube config set driver virtualbox
minikube config set driver hyperv
minikube start --kubernetes-version v1.32.9 --memory=12000 --cpus=2
minikube config view
```
The Pyroscope Helm chart deploys the Pyroscope server and creates the appropriate RBAC roles.
* https://grafana.com/docs/pyroscope/latest/deploy-kubernetes/helm/
* https://github.com/grafana/pyroscope/tree/main/operations/pyroscope/helm/pyroscope
```bash
helm repo add pyroscope-io https://pyroscope-io.github.io/helm-chart
helm install demo pyroscope-io/pyroscope -f values.yaml
```

# References:
* [Profiling in Python with Pyroscope's Pip Package](https://grafana.com/blog/2021/10/14/profiling-in-python-with-pyroscopes-pip-package/)
  * https://github.com/grafana/pyroscope/tree/main/examples/language-sdk-instrumentation/python
* https://pkg.go.dev/github.com/pyroscope-io/hotrod-golang
  * https://github.com/jaegertracing/jaeger/tree/main/examples/hotrod
* [What you’re missing without profiling: An Introduction to Pyroscope](https://itnext.io/what-youre-missing-without-profiling-an-introduction-to-pyroscope-eb45a4ec2608)