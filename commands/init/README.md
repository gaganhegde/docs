# Odo Pipelines Init Command

Init command creates CI/CD environments for you.  Unlike the [bootstrap](bootstrap.md) command, it does not create your first application and other environments.   It outputs resources yaml files, kustomization files, and Manifest to filesystem.

```shell
$ odo pipelines init 
  --gitops-repo-url
  --image-repo
  --dockercfgjson 
  [--internal-registry-hostname]
  [--gitops-webhook-secret]
  [--sealed-secrets-ns]  
  [--prefix]
  [--output]
  [--overwrite]
  [--sealed-secrets-ns]
  [--sealed-secrets-svc]
  [--status-tracker-access-token]
```

| Flag                                  | Description |
| ------------------------------------- | ----------- |
| --dockercfgjson                       | Path to dockercfgjson file..  This is used to authenticate image pushes to your image-repo. |
| --gitops-repo-url                     | The Git repository where your configuration and manifest live. E.g. https://github.com/user/gitops.git|
| --gitops-webhook-secret               | Optional. This is used to validate incoming hooks.  (if not provided, it will be auto-generated)|
| --help                                | Help for init |
| --image-repo                          | Where should we configure your builds to push to? E.g. quay.io/user/service or user/service for internal registry|
| --internal-registry-hostname          | Optional.  Internal image registry hostname (default _image-registry.openshift-image-registry.svc:5000_)
| --output                              | Optional.  Output path.  (default is the current working directory|
| --prefix                              | Optional.  This is used to help separate user namespaces. |
| --overwrite                           | Optional. Overwrite an existing GitOps configuration (default false) |
| --sealed-secrets-ns                   | Optional. Namespace in which the Sealed Secrets operator is installed, automatically generated secrets are encrypted with this operator (default "kube-system")|
|  --sealed-secrets-ns string           | Optional. Namespace in which the Sealed Secrets operator is installed, automatically generated secrets are encrypted with this operator (default "sealed-secrets") |
| --sealed-secrets-svc string           | Optional. Name of the Sealed Secrets Services that encrypts secrets (default "sealedsecretcontroller-sealed-secrets") |
| --status-tracker-access-token string  | Optional. Used to authenticate requests to push commit-statuses to your Git hosting service |

The following [directory layout](output) is generated.

```shell
.
├── config
│   └── cicd
│       ├── base
│       │   ├── 01-namespaces
│       │   │   └── cicd-environment.yaml
│       │   ├── 02-rolebindings
│       │   │   ├── pipeline-service-account.yaml
│       │   │   ├── pipeline-service-role.yaml
│       │   │   └── pipeline-service-rolebinding.yaml
│       │   ├── 03-secrets
│       │   │   ├── docker-config.yaml
│       │   │   └── gitops-webhook-secret.yaml
│       │   ├── 04-tasks
│       │   │   ├── deploy-from-source-task.yaml
│       │   │   └── deploy-using-kubectl-task.yaml
│       │   ├── 05-pipelines
│       │   │   ├── app-ci-pipeline.yaml
│       │   │   └── ci-dryrun-from-push-pipeline.yaml
│       │   ├── 06-bindings
│       │   │   └── gitlab-push-binding.yaml
│       │   ├── 07-templates
│       │   │   ├── app-ci-build-from-push-template.yaml
│       │   │   └── ci-dryrun-from-push-template.yaml
│       │   ├── 08-eventlisteners
│       │   │   └── cicd-event-listener.yaml
│       │   ├── 09-routes
│       │   │   └── gitops-webhook-event-listener.yaml
│       │   └── kustomization.yaml
│       └── overlays
│           └── kustomization.yaml
└── pipelines.yaml
```

