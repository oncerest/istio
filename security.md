Secure Production Identity Framework for Everyone (SPIFFE). Default ServiceAccount is encoded as `spiffe://cluster.local/ns/default/sa/default`

Both SPIRE and Istio issue X.509 SVIDs that are short lived—they expire on the order of an hour after issuance. 

## mTLS
mTLS is TLS in which both parties, client and server, present certificates to each other.
