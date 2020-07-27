## Install Sealed Secrets

It is a two steps process to install Sealed Screts.
* Install Sealed Secrets Operator in the `kube-system` namespace
* Create a Sealed Secrets Controller in the `kube-system` namespace

![](img/sealed-secrets-controller-install.gif)

## Alternative approach

Install sealed secrets manually, run ``` kubectl apply -f https://github.com/bitnami-labs/sealed-secrets/releases/download/v0.12.3/controller.yaml ```.
