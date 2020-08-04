# Day 1 Operations

Day 1 Operations are actions that users take to bootstrap a GitOps system.  

## Prerequisites

### Cluster setup

The following will be provided to attendees in a 4.6 / 4.5 cluster

* [Sealed Secrets Operator](prerequisites/sealed_secrets.md)
* [OpenShift Pipelines Operator](prerequisites/tekton_operator.md)
* [ArgoCD](prerequisites/argocd.md)


### Developer Setup

* Create [GitOps repository](prerequisites/gitops_repo.md) on Github.

* Fork/Clone the source Git repository ([bluetooth-module-inventory](prerequisites/service_repo.md) is used as an example in this document) in your Github account.

* The external image repository secret to authenticate image pushes on sucessfull pipeline execution. To use quay.io, please follow [prerequisites/quay.md](prerequisites/quay.md).Create a repository named 
"bluetooth-module-inventory".

* Download unofficial [odo](../../commands/bin) binary

* Steps to create the git access token for [commit-status-tracker/webhook access-token](prerequisites/git_access_token_steps.md)


**NOTE**: `odo pipelines` commands are hidden in `Expermential Mode`.  To enable `Expermential Mode` available, please set th `EXPERIEMENTAL` environment variable in the running terminal.
```shell
$ export ODO_EXPERIMENTAL=true
```

Validate the version of `odo`

```
$ odo version
odo v1.2.3 (8e08fc8f3)
```


# Workshop

## Bootstrapping the Manifest

```shell
$ odo pipelines bootstrap \
  --service-repo-url https://github.com/<username>/bluetooth-module-inventory.git \
  --gitops-repo-url https://github.com/<username>/gitops.git \
  --image-repo quay.io/<username>/bluetooth-module-inventory \
  --dockercfgjson ~/Downloads/<username>-auth.json \
  --prefix bluetooth
  --sealed-secrets-ns kube-system
  --sealed-secrets-svc sealed-secrets-controller
```


## Bringing the bootstrapped environment up

First of all, let's get started with our Git repository.

From the root of your gitops directory (with the pipelines.yaml), execute the
following commands:

```shell
$ git init .
$ git add .
$ git commit -m "Initial commit."
$ git remote add origin <insert gitops repo>
$ git push -u origin master
```

This should initialise the GitOps repository, this is the start of your journey
to deploying applications via Git.

Next, we'll bring up our deployment infrastructure, this is only necessary at the
start, the configuration should be self-hosted thereafter.

```shell
$ oc apply -k config/argocd/
```

You should now be able to create a route to your new service, it should be
running [nginx](https://nginx.org/) and serving a page.


## Setup up Git webhooks

### Your Application Repository


```shell
$ odo pipelines webhook create \
    --access-token $GITHUB_TOKEN \ 
    --env-name bluetooth-dev \
    --service-name bluetooth-module-inventory
```

### Your Gitops Repository

We need to ensure that there's a Webhook configured for
the "gitops" repo.

```shell
$ odo pipelines webhook create \
    --access-token $GITHUB_TOKEN \
    --cicd
```


## Changing the initial deployment

### Update Image

The bootstrap creates a `Deployment` in `environments/<prefix>-dev/services/<service name>/base/config/100-deployment.yaml` this should bring up nginx, this is purely for demo purposes, you'll need to change this to deploy your built image.

```yaml
spec:
  containers:
  - image: nginxinc/nginx-unprivileged:latest
    imagePullPolicy: Always
    name: bluetooth-module-inventory-svc
```

You'll want to replace this with the image for your application, if you've built
it.

### Add Ingress

Navigate to 

```
/environments/bluetooth-dev/apps/app-bluetooth-module-inventory/services/bluetooth-module-inventory/base/config
```

Create a file named `300-route.yaml`

```
kind: Route
apiVersion: route.openshift.io/v1
metadata:
  name: bluetooth-module-inventory
  namespace: bluetooth-dev
spec:
  to:
    kind: Service
    name: bluetooth-module-inventory
    weight: 100
  port:
    targetPort: http
  wildcardPolicy: None
```

Update `kustomization.yaml`

```
resources:
- 100-deployment.yaml
- 200-service.yaml
- 300-route.yaml
```

Make a PR to your gitops repository and watch the CI validate the changes.

After validation is complete, merge the PR and watch the route become available in the `bluetooth-dev` namespace on OpenShift.

----

## Managed workloads with applications


Navigate to 

```
/environments/bluetooth-dev/apps/app-bluetooth-module-inventory/services/bluetooth-module-inventory/base/config
```

### Add a managed database

Create `400-managed-database.yaml`

```
apiVersion: postgresql.baiju.dev/v1alpha1
kind: Database
metadata:
  name: bluetooth-db
  namespace: bluetooth-dev
spec:
  image: docker.io/postgres
  imageName: postgres
  dbName: postgres
```

### Add managed credentials

Create `500-managed-binding.yaml`

```
apiVersion: apps.openshift.io/v1alpha1
kind: ServiceBindingRequest
metadata:
  name: bluetooth-managed-credentials
  namespace: bluetooth-dev
spec:
  backingServiceSelector:
    group: postgresql.baiju.dev
    version: v1alpha1
    kind: Database
    resourceRef: bluetooth-db
```

### Update your Application

Edit `100-deployment.yaml`


```
spec:
    containers:
    - image: YOUR_EXISTING_IMAGE
      envFrom:
        - secretRef:
          name: fruit-app-managed-credentials
```
