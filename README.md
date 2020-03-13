# istio

## notes running istio on k8s/docker desktop

install docker desktop, verify by `docker run hello-world`

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
NAME                                       CREATED AT
adapters.config.istio.io                   2020-03-12T13:03:09Z
attributemanifests.config.istio.io         2020-03-12T13:02:43Z
authorizationpolicies.security.istio.io    2020-03-12T13:02:43Z
certificates.certmanager.k8s.io            2020-03-12T13:02:52Z
challenges.certmanager.k8s.io              2020-03-12T13:03:02Z
clusterissuers.certmanager.k8s.io          2020-03-12T13:02:52Z
clusterrbacconfigs.rbac.istio.io           2020-03-12T13:02:43Z
destinationrules.networking.istio.io       2020-03-12T13:02:42Z
envoyfilters.networking.istio.io           2020-03-12T13:02:42Z
gateways.networking.istio.io               2020-03-12T13:02:42Z
handlers.config.istio.io                   2020-03-12T13:02:43Z
httpapispecbindings.config.istio.io        2020-03-12T13:02:41Z
httpapispecs.config.istio.io               2020-03-12T13:02:41Z
instances.config.istio.io                  2020-03-12T13:02:43Z
issuers.certmanager.k8s.io                 2020-03-12T13:02:52Z
meshpolicies.authentication.istio.io       2020-03-12T13:02:41Z
orders.certmanager.k8s.io                  2020-03-12T13:03:02Z
peerauthentications.security.istio.io      2020-03-12T13:02:43Z
policies.authentication.istio.io           2020-03-12T13:02:41Z
quotaspecbindings.config.istio.io          2020-03-12T13:02:42Z
quotaspecs.config.istio.io                 2020-03-12T13:02:41Z
rbacconfigs.rbac.istio.io                  2020-03-12T13:02:43Z
requestauthentications.security.istio.io   2020-03-12T13:02:43Z
rules.config.istio.io                      2020-03-12T13:02:43Z
serviceentries.networking.istio.io         2020-03-12T13:02:42Z
servicerolebindings.rbac.istio.io          2020-03-12T13:02:43Z
serviceroles.rbac.istio.io                 2020-03-12T13:02:43Z
sidecars.networking.istio.io               2020-03-12T13:02:42Z
templates.config.istio.io                  2020-03-12T13:03:09Z
virtualservices.networking.istio.io        2020-03-12T13:02:42Z
```

install istio demo `kubectl apply -f install/kubernetes/istio-demo.yaml`


