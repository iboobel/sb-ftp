### install k3d (Little helper to run Rancher Lab's k3s in Docker) (https://github.com/rancher/k3d )

### create k3s cluster
```
k3d c --publish 21:21 --publish 21100:21100 --publish 21101:21101 --publish 21102:21102 --server-arg "--no-deploy=traefik" 
```

### set up kubectl 
```
export KUBECONFIG="$(k3d get-kubeconfig --name='k3s-default')"
```

### Link helm with tiller
``` 
kubectl -n kube-system create serviceaccount tiller
kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller
helm init --service-account tiller
```

### install nginx-ingress 
```
helm install --name nginx-ingress -f nginx-ingress-values.yaml stable/nginx-ingress
```

### apply manifests
```
kubectl apply -f pvc.yaml
kubectl apply -f service.yaml
kubectl apply -f deployment.yaml
```

### check ftp 
127.0.0.1 , port 21