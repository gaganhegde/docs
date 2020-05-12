## Install ArgoCD

```shell
oc create namespace argocd

oc apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/v1.4.2/manifests/install.yaml

ARGOCD_SERVER_PASSWORD=$(oc -n argocd get pod -l "app.kubernetes.io/name=argocd-server" -o jsonpath='{.items[*].metadata.name}')

PATCH='{"spec":{"template":{"spec":{"$setElementOrder/containers":[{"name":"argocd-server"}],"containers":[{"command":["argocd-server","--insecure","--staticassets","/shared/app"],"name":"argocd-server"}]}}}}'

oc -n argocd patch deployment argocd-server -p $PATCH

oc -n argocd create route edge argocd-server --service=argocd-server --port=http --insecure-policy=Redirect

ARGOCD_ROUTE=$(oc -n argocd get route argocd-server -o jsonpath='{.spec.host}')

argocd --insecure --grpc-web login ${ARGOCD_ROUTE}:443 --username admin --password ${ARGOCD_SERVER_PASSWORD}

# Update admin's password (changeMe)
argocd --insecure --grpc-web --server ${ARGOCD_ROUTE}:443 account update-password --current-password ${ARGOCD_SERVER_PASSWORD} --new-password changeMe
```
Note: New password has been set by the last command which is used to login to ArgoCD web console

### Obtain web address (host/port) of ArgoCD web console by looking up OpenShift Route

```shell
$ oc get route -n argocd
NAME            HOST/PORT                                                    PATH   SERVICES        PORT   TERMINATION     WILDCARD
argocd-server   argocd-server-argocd.apps.gitops1.devcluster.openshift.com          argocd-server   http   edge/Redirect   None
```