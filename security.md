Secure Production Identity Framework for Everyone (SPIFFE). Default ServiceAccount is encoded as `spiffe://cluster.local/ns/default/sa/default`

Both SPIRE and Istio issue X.509 SVIDs that are short lived—they expire on the order of an hour after issuance. 

## authn policy
mTLS is TLS in which both parties, client and server, present certificates to each other.
```
apiVersion: authentication.istio.io/v1alpha1
kind: Policy
metadata:  
  name: foo-require-mtls  
  namespace: default
spec:  
  targets:  
    - name: foo.default.svc.cluster.local  
  peers:  
    - mtls:     
      mode: STRICT
```

### end user auth via JWT
end-user credentials stored as a JWT in the "x-goog-iap-jwt-assertion" header
```
apiVersion: authentication.istio.io/v1alpha1
kind: Policy
metadata:  
  name: end-user-auth  
  namespace: default
spec: 
  target: 
    - name: bar  
  peers:  
    - mtls: {}  
  origins:  
    - jwt:      
       issuer: "https://securetoken.google.com"      
       audiences:      - "bar"      
       jwksUri: "https://www.googleapis.com/oauth2/v1/certs"      
       jwt_headers:      
          - "x-goog-iap-jwt-assertion"  
  principalBinding: USE_ORIGIN
  ```
  
  
## authz policy
Istio’s RBAC is service-to-service focused, specifying which services can connect and communicate with one another. It defines ServiceRole and ServiceRoleBinding
```
apiVersion: "rbac.istio.io/v1alpha1"
kind: ClusterRBACConfig
metadata:
  name: default
  namespace: istio-system
spec:
  mode: ON
```
