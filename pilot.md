
# mesh configuration
## mesh config
- MeshConfig (mesh.istio.io/v1alpha1.MeshConfig)
- ProxyConfig (mesh.istio.io/v1alpha1.ProxyConfig)
- MeshNetworks (mesh.istio.io/v1alpha1.MeshNetworks)

## network config
- ServiceEntry 
- DestinationRules 
- VirtualServices 

## service discovery

# debugging
```
istioctl authn
istioctl proxy-config <bootstrap | listener | route | cluster>     <kubernetes pod>::
istioctl proxy-status

kubectl -n istio-system get cm istio -o jsonpath="{@.data.mesh}"
```
