<img src="./images/kubernetes-architecture.png" alt="Kubernetes Architecture" width="500">


[概述](https://thenewstack.io/kubernetes-an-overview/)


* [minikube](https://github.com/kubernetes/minikube)
* [kubeadm](https://github.com/kubernetes/kubeadm)
* [kops](https://github.com/kubernetes/kops)
* [play-with-kubernetes](https://labs.play-with-k8s.com/)


#### minikube

安装 minikube 

```bash
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-amd64
chmod +x minikube
mv minikube /usr/local/bin
```

```bash
minikube start
# Error starting cluster:  kubeadm init error
# 遇到上述报错
```