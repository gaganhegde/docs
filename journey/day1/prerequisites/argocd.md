
### Argocd Operator Installation - with operator( Recommended )

Click on the argocd operator as shown below in the operator hub on your openhift console and install the operator in the argocd namespace.


![ArgocdOperator](../img/Argocd_operator_gitops.png)


Then run ``` oc adm policy add-cluster-role-to-user cluster-admin system:serviceaccount:argocd:argocd-application-controller ``` to give the argocd controller the correct permissions.

In order to get the correct password to login to the argocd web ui , run ``` kubectl get secret argocd-cluster -n argocd -ojsonpath='{.data.admin\.password}' | base64 --decode ```, user-name being admin. 


## Install ArgoCD - manually

```shell
oc create namespace argocd

oc apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/v1.4.2/manifests/install.yaml

ARGOCD_SERVER_PASSWORD=$(oc -n argocd get pod -l "app.kubernetes.io/name=argocd-server" -o jsonpath='{.items[*].metadata.name}')

PATCH='{"spec":{"template":{"spec":{"$setElementOrder/containers":[{"name":"argocd-server"}],"containers":[{"command":["argocd-server","--insecure","--staticassets","/shared/app"],"name":"argocd-server"}]}}}}'

oc -n argocd patch deployment argocd-server -p $PATCH

oc -n argocd create route edge argocd-server --service=argocd-server --port=http --insecure-policy=Redirect

ARGOCD_ROUTE=$(oc -n argocd get route argocd-server -o jsonpath='{.spec.host}')

# Wait a few seconds until ArgoCD services are running then run this command.  
# Re-run this command until you see "'admin' logged in successfully" message
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
