# k8s-templates
For using as kubernetes manifest templates

### FYI: test on docker-desktop and Kind didn't work, try colima instead but didn't work on Chrome

### Prerequisite
0.1 Create metallb namespace

```sh
kubectl create ns metallb-system
```
0.2 Install MetalLB CRDs
```sh
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.14.5/config/manifests/metallb-native.yaml
```
0.3 Wait until controller and speaker pods are running or use command 
```sh
kubectl wait --namespace metallb-system \
  --for=condition=ready pod \
  --all \
  --timeout=120s
```
0.4 Install MetalLB configuration
```sh
kubectl apply -f metallb-config.yaml
```

### For ingress using
1. Install ingress-nginx CRDs (no need to create namespace)
```sh
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
```
2. Install manifest-test-ingress.yaml
```sh
kubectl apply -f manifest-test-ingress.yaml
```
4. Check IP from colima to config in /etc/hosts
```sh
colima list
```
```
# Ex.
192.168.64.2    k8s.dojo
```
5. Check NodePort by command to see \<port\>:\<node-port\>/TCP (use node-port)
```sh
kubectl get svc -A | grep gateway 
```
6. We will get url to test on web browser
```
http://k8s.dojo:<node-port>
```
or test curl
```sh
# Ex.
curl -v http://k8s.dojo:32714
# or 
curl -H "Host: k8s.dojo" http://192.168.64.2:32714
```

### For gateway API using
1. Install Gateway API CRDs
```sh
kubectl apply --server-side -f https://github.com/kubernetes-sigs/gateway-api/releases/download/v1.5.0/standard-install.yaml
```
2. Install NGINX Gateway Fabric (controller)
```sh
kubectl apply --server-side -f https://raw.githubusercontent.com/nginxinc/nginx-gateway-fabric/main/deploy/crds.yaml
kubectl apply -f https://raw.githubusercontent.com/nginxinc/nginx-gateway-fabric/main/deploy/default/deploy.yaml
```
3. Install manifest-test-gateway-api.yaml
```sh
kubectl apply -f manifest-test-gateway-api.yaml
```
4. Check IP from colima to config in /etc/hosts
```sh
colima list
```
```
# Ex.
192.168.64.2    k8s.dojo
```
5. Check NodePort by command to see \<port\>:\<node-port\>/TCP (use node-port)
```sh
kubectl get svc -A | grep gateway 
```
6. We will get url to test on web browser
```
http://k8s.dojo:<node-port>
```
or test curl
```sh
# Ex.
curl -v http://k8s.dojo:32714
# or 
curl -H "Host: k8s.dojo" http://192.168.64.2:32714
```

### Install configmap/secret
```sh
kubectl apply -f ./configmap-and-secret/configmap.yaml

kubectl apply -f ./configmap-and-secret/secret.yaml
```

### Other useful commands
Port forwarding to access (if directly access not working)
```sh
kubectl port-forward -n ingress-nginx svc/ingress-nginx-controller 8080:8080
```

check local IP
```sh
ifconfig | grep inet
```