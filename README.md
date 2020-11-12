# argocd

````

kubectl create namespace argocd

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

cloud.google.com/load-balancer-type: External

kubectl get pods -n argocd -l app.kubernetes.io/name=argocd-server -o name | cut -d'/' -f 2


# To reset password you might remove 'admin.password' and 'admin.passwordMtime' keys from argocd-secret and restart api server pod. Password will be reset to pod name

kubectl patch secret argocd-secret  -p '{"data": {"admin.password": null, "admin.passwordMtime": null}}'

kubectl -n argocd patch secret argocd-secret -p '{"stringData": { "admin.password": "$2a$10$YfSavKYddETNHY2giEmTjOdieYEc8poRnmqm51.j666oTnnyQlFjK","admin.passwordMtime": "'$(date +%FT%T%Z)'" }}'



````

By default the password is set to the name of the server pod, as per the getting started guide.

To change the password, edit the argocd-secret secret and update the admin.password field with a new bcrypt hash. You can use a site like https://www.browserling.com/tools/bcrypt to generate a new hash. For example:

# bcrypt(password)=$2a$10$rRyBsGSHK6.uc8fntPwVIuLVHgsAhAX7TcdrqW/RADU0uh7CaChLa
kubectl -n argocd patch secret argocd-secret \
  -p '{"stringData": {
    "admin.password": "$2a$10$rRyBsGSHK6.uc8fntPwVIuLVHgsAhAX7TcdrqW/RADU0uh7CaChLa",
    "admin.passwordMtime": "'$(date +%FT%T%Z)'"
  }}'
Another option is to delete both the admin.password and admin.passwordMtime keys and restart argocd-server. This will set the password back to the pod name as per the getting started guide.


apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: bookinfo
spec:
  hosts:
  - "*"
  gateways:
  - bookinfo-gateway
  http:
  - match:
    - uri:
        exact: /productpage
    route:
    - destination:
        host: productpage
        port:
          number: 9080