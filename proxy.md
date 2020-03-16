# service proxy
## Automatic Sidecar Injection
Automatic sidecar injection in Kubernetes relies on mutating admission webhooks. The `istio-sidecar-injector` is added as a mutating webhook configuration resource when Istio is installed on Kubernetes.
The webhook will create 2 extra containers -- `istio-proxy` and `init-container` on pod if the namespace has the `istio-injection=enabled` label
```
kubectl get mutatingwebhookconfigurations istio-sidecar-injector -o yaml
apiVersion: admissionregistration.k8s.io/v1beta1
kind: MutatingWebhookConfiguration
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"admissionregistration.k8s.io/v1beta1","kind":"MutatingWebhookConfiguration","metadata":{"annotations":{},"labels":{"app":"sidecarInjectorWebhook","chart":"sidecarInjectorWebhook","heritage":"Tiller","release":"istio"},"name":"istio-sidecar-injector"},"webhooks":[{"clientConfig":{"caBundle":"","service":{"name":"istio-sidecar-injector","namespace":"istio-system","path":"/inject"}},"failurePolicy":"Fail","name":"sidecar-injector.istio.io","namespaceSelector":{"matchLabels":{"istio-injection":"enabled"}},"rules":[{"apiGroups":[""],"apiVersions":["v1"],"operations":["CREATE"],"resources":["pods"]}]}]}
  creationTimestamp: "2020-03-12T13:28:31Z"
  generation: 2
  labels:
    app: sidecarInjectorWebhook
    chart: sidecarInjectorWebhook
    heritage: Tiller
    release: istio
  name: istio-sidecar-injector
  resourceVersion: "10691"
  selfLink: /apis/admissionregistration.k8s.io/v1beta1/mutatingwebhookconfigurations/istio-sidecar-injector
  uid: 5034839a-0113-419d-a0bd-3eab71e131ff
webhooks:
- admissionReviewVersions:
  - v1beta1
  clientConfig:
    caBundle: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUMzakNDQWNhZ0F3SUJBZ0lSQVBaaWtZMWZVa01HbDBOYTFEKy9pTm93RFFZSktvWklodmNOQVFFTEJRQXcKR0RFV01CUUdBMVVFQ2hNTlkyeDFjM1JsY2k1c2IyTmhiREFlRncweU1EQXpNVEl4TXpNeU1UUmFGdzB6TURBegpNVEF4TXpNeU1UUmFNQmd4RmpBVUJnTlZCQW9URFdOc2RYTjBaWEl1Ykc5allXd3dnZ0VpTUEwR0NTcUdTSWIzCkRRRUJBUVVBQTRJQkR3QXdnZ0VLQW9JQkFRRFIxZTBhRHFXZGFtQ1czQlRJblA2SkdKVGNtVEEwcFNZNHdhdEwKZFBGTVBBUzk2OFIzTTJPdDRaZjRxS092dk9NQTFtcHBkeEhleFlMcnVIYit0ZlZnYUFEdTRvaUNRM2RMcmNYcgp6REhsTEFUdnlCZWJZRVR4L0lsTk0vVnJ0eDBjU1hWQWJpWmNRVStsSll0MDQ2R1p3dHVWYTU3UmJCSWljZjY0ClM4eWlTT0xIUzR0VFJwMFlpalVpNmVIMHJINjJkaURuZzlBRjlQa1RMdzloNlorbk11aWlUeHFQZEhaN0d0Z2sKRjc2U0FkK2F5K3dtUGFzMGNNYkptODhQVTZxM2xpaWRtVWtRbEZUN1VqSk1qRHoyMlFKNGhQZzZpRk9Vak1KTQp6ZWZwb1Nodi9TMGRaSGRSOTUzVGZLaUwyZ0JHREN6d05CUEZ2bkRxM3RzcWhFWG5BZ01CQUFHakl6QWhNQTRHCkExVWREd0VCL3dRRUF3SUNCREFQQmdOVkhSTUJBZjhFQlRBREFRSC9NQTBHQ1NxR1NJYjNEUUVCQ3dVQUE0SUIKQVFBREdzRW1uL2R1MUN0NXZJM0d1cjJRcVFXTUNZNFVTZ1dNZWtiU1hwSHprc1Rjcm5qQjFtYzBXMHROK1MvWgpxWmJHVGZEZS9ZWFM3NFQyNnMrS2JVWXU4cmNpdXpRYWdTckxJNjllTTRMNURFUE5abElCeXNNdUg3M0ZGbmp2Cm9jRVltOWFIem1ieUdvL0lBdCsxUnFlWGsxZTJtZmRnSDRuNEp5U2RYQUd4R0ZRSFpNelN1RnQ0cktodEJtV2QKdHVvZ2p0ekxodWwvRE5PU1IrODRxTFZrWU5MdHh6VEx1Sllpd1c2RTAyUUJtTUNjb09VTHUrdWM1bXJlNFVxWQpCMmxHMkRHQVRMWEU2OUhMbWhpR0JGeTk5NnlVUzZuU1Y1SDJ4RktqdUVmdm5HOXBMNVc4eXI1d2pBMmYzOCtqCjFCU1VWWFJ2VTMyamNaZ001RkhySGxIRAotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==
    service:
      name: istio-sidecar-injector
      namespace: istio-system
      path: /inject
      port: 443
  failurePolicy: Fail
  matchPolicy: Exact
  name: sidecar-injector.istio.io
  namespaceSelector:
    matchLabels:
      istio-injection: enabled
  objectSelector: {}
  reinvocationPolicy: Never
  rules:
  - apiGroups:
    - ""
    apiVersions:
    - v1
    operations:
    - CREATE
    resources:
    - pods
    scope: '*'
  sideEffects: Unknown
  timeoutSeconds: 30

kubectl get ns -L istio-injection
NAME                   STATUS   AGE     ISTIO-INJECTION
default                Active   3d14h   enabled
docker                 Active   3d14h   enabled
istio-system           Active   3d12h   disabled
kube-node-lease        Active   3d14h
kube-public            Active   3d14h
kube-system            Active   3d14h
```
  
## xDS
A collective set of discovery services for Envoy’s APIs is referred to as xDS.

listener discovery service (LDS), route discovery service (RDS), cluster discovery service (CDS), and endpoint discovery service (EDS).
  
## pilot-agent running in istio-proxy  
, Istio will terminate connections on reload of a service’s certificate. Envoy’s Secret Discovery Service (SDS) provides the mechanism by which you can push secrets (certificates) to each service proxy.
The `pilot-agent` handles restarting Envoy when certificates are rotated (once per hour). 
```
kubectl exec ratings-v1-7855f5bcb9-cnzpr -c istio-proxy ps
  PID TTY          TIME CMD
    1 ?        00:00:40 pilot-agent
   14 ?        00:03:02 envoy
   30 ?        00:00:00 ps
   
kubectl exec -it $(kubectl get pod | grep productpage | awk '{ print $1 }') -c istio-proxy cat /etc/certs/cert-chain.pem |openssl x509 -text -noout
```
