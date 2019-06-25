---
layout: post
title:  Kubernetes commands from interactive tutorial
description: "Kubernetes commands from interactive tutorial"
modified: 2019-06-24
tags: [kubernetes]
---

Kubernetes commands from tutorial https://kubernetes.io/docs/tutorials

{% raw %}

### setup (optional)

```bash
sudo mv /root/.kube /root/.minikube /home1/irteam
sudo chown -R irteam /home1/irteam/.kube /home1/irteam/.minikube

sudo minikube start --vm-driver=none
sudo minikube stop
sudo minikube delete -p minikube

sudo mv /root/.kube /root/.minikube $HOME
sudo chown -R $USER $HOME/.kube $HOME/.minikube
vi $HOME/.kube/config
```

### check cluster

```bash
minikube version
minikube ip
kubectl version
kubectl cluster-info
```

### run pod
```bash
kubectl run kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1 --port=8080
kubectl run hello-minikube --image=k8s.gcr.io/echoserver:1.10 --port=8080

kubectl get pods
kubectl get deployments

export POD_NAME=$(kubectl get pods -o go-template \
--template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')
echo POD_NAME=$POD_NAME
```

### access to running pod

```bash
kubectl exec -it $POD_NAME env
kubectl exec -it $POD_NAME curl http://localhost:8080
kubectl exec -it $POD_NAME bash
```

### create service (expose pod)

```bash
kubectl expose deployment/kubernetes-bootcamp --type="NodePort" --port 8080
kubectl get services 
kubectl describe services/kubernetes-bootcamp
```

### access outside cluster

```bash
export NODE_PORT=$(kubectl get services/kubernetes-bootcamp \
-o go-template='{{(index .spec.ports 0).nodePort}}')
echo NODE_PORT=$NODE_PORT
curl $(minikube ip):$NODE_PORT
```

### get by label

```bash
kubectl describe services 
kubectl get deployments -l run=kubernetes-bootcamp
kubectl get pods -l run=kubernetes-bootcamp
kubectl get services -l run=kubernetes-bootcamp
```

### label a pod

```bash
kubectl label pod $POD_NAME app=v1
kubectl get pods -l app=v1
kubectl get services -l app=v1 # not labeled
```

### scale-up pods

```bash
kubectl scale deployments/kubernetes-bootcamp --replicas=4
kubectl get deployments
kubectl get pods -o wide
```

### see load balancing

```bash
curl $(minikube ip):$NODE_PORT # requests are served at different pods
curl $(minikube ip):$NODE_PORT
curl $(minikube ip):$NODE_PORT
curl $(minikube ip):$NODE_PORT
```

### roll upgrade

```bash
kubectl set image deployments/kubernetes-bootcamp \
kubernetes-bootcamp=jocatalin/kubernetes-bootcamp:v2
kubectl get pods # see changes
kubectl get pods
kubectl get pods

kubectl rollout status deployments/kubernetes-bootcamp # confirm rollout with latest image version
```

### roll back

```bash
kubectl set image deployments/kubernetes-bootcamp \
kubernetes-bootcamp=gcr.io/google-samples/kubernetes-bootcamp:v10
kubectl get pods # see problem
kubectl rollout undo deployments/kubernetes-bootcamp # roll back to previous image version
kubectl get pods
```

### remove service

```bash
kubectl delete service -l run=kubernetes-bootcamp
kubectl get services
curl $(minikube ip):$NODE_PORT
kubectl exec -it $POD_NAME curl localhost:8080
```

{% endraw %}
