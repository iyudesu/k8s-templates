# k8s-templates
For using as kubernetes manifest templates

### FYI: test on docker-desktop and Kind didn't work, try colima instead but didn't work on Chrome

Create metallb namespace

```sh
kubectl create ns metallb-system
```
Install MetalLB CRDs
```sh
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.14.5/config/manifests/metallb-native.yaml
```

Wait until controller and speaker pods are running or use command 
```sh
kubectl wait --namespace metallb-system \
  --for=condition=ready pod \
  --all \
  --timeout=120s
```

Install MetalLB configuration
```sh
kubectl apply -f metallb-config.yaml
```

Install ingress-nginx CRDs (no need to create namespace)
```sh
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
```

Apply manifest

Port forwarding to access (if directly access not working)
```sh
kubectl port-forward -n ingress-nginx svc/ingress-nginx-controller 8080:8080
```

check local IP
```sh
ifconfig | grep inet
```

Check Colima IP
```sh
colima list
```

Test curl commands
```sh
curl -v http://k8s.dojo:32714
# or 
curl -H "Host: k8s.dojo" http://192.168.64.2:32714
```
