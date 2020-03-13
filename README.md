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

verify with `kubectl get svc -n istio-system` and `kubectl get po -n istio-system`
```
istioctl proxy-status
NAME                                                   CDS        LDS        EDS        RDS          PILOT                           VERSION
istio-egressgateway-67f444d69-kddzl.istio-system       SYNCED     SYNCED     SYNCED     NOT SENT     istio-pilot-bb4bc587f-9j4ch     1.5.0
istio-ingressgateway-7cc859b7c8-wdjt8.istio-system     SYNCED     SYNCED     SYNCED     NOT SENT     istio-pilot-bb4bc587f-9j4ch     1.5.0
```
label default namespace with `kubectl label namespace default istio-injection=enable`

deploy sample `kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml`
```
kubectl describe po reviews-v1-64bc5454b9-685xh
  istio-proxy:
    Container ID:  docker://a4ad762ff638a0941370368c1989418c16944de72f5720a934316d29580c925e
    Image:         docker.io/istio/proxyv2:1.5.0
    Image ID:      docker-pullable://istio/proxyv2@sha256:89b5fe2df96920189a193dd5f7dbd776e00024e4c1fd1b59bb53867278e9645a
    Port:          15090/TCP
    Host Port:     0/TCP
    Args:
      proxy
      sidecar
```

