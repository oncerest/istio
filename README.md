# istio

## notes running istio on k8s/docker desktop

install docker desktop

verify by `docker run hello-world`

enable k8s https://docs.docker.com/docker-for-windows/#kubernetes

having issue of k8s startup? see https://github.com/docker/for-win/issues/1649
```
kubectl version --short
kubectl get nodes
```
install k8s dashboard https://github.com/kubernetes/dashboard

for token to access dashboard, grab one from `kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | awk '/default-token/ {print $1}')`

install istio from https://github.com/istio/istio/releases

register istio crd **CustomResourceDefinitions**
```
for i in install/kubernetes/helm/istio-init/files/crd*yaml; do kubectl apply -f $i; done
kubectl  get crd
```

