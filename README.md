# istio

## notes running istio on k8s/docker desktop

https://github.com/kubernetes/dashboard

kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | awk '/default-token/ {print $1}')

