# argocd


```

## install 
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml 


## server yaml edit to expose to outside world 

cloud.google.com/load-balancer-type: external


## password 
kubectl get pods -n argocd -l app.kubernetes.io/name=argocd-server -o name | cut -d'/' -f 2 




```
