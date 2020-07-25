# Odo Pipelines Bootstrap Command

Bootstrap command creates default environments for your initial application.  It outputs resources yaml files, kustomization files, and Manifest.  The following resources are written to filesystem.
   
* CI/CD environment with pipelines and resources
* ArgoCD environment
* Dev environment with an application/service
* Stage environment

```shell
$ odo pipelines bootstrap 
  --gitops-repo-url
  --service-repo-url 
  --image-repo
  --dockercfgjson 
  [--internal-registry-hostname]
  [--gitops-webhook-secret]
  [--service-webhook-secret]
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
| --dockercfgjson                       | This is used to authenticate image pushes to your image-repo. |
| --gitops-repo-url                     | This is where your configuration and pipelines live. |
| --gitops-webhook-secret               | Optional. This is used to validate incoming hooks.  (if not provided, it will be auto-generated)|
| --help                                | Help for bootstrap|
| --image-repo                          | Where should we configure your builds to push to? |
| --internal-registry-hostname          | Internal image registry hostname (default "image-registry.openshift-image-registry.svc:5000") |
| --output                              | Folder path to add Gitops resources (default ".") |
| --prefix                              | This is used to help separate user namespaces. |
| --service-repo-url                    | This is the source code to your first application. |
| --service-webhook-secret              | Optional. Creates a secret used to validate incoming hooks. (if not provided, it will be auto-generated)|
| --overwrite                           | Optional. Overwrite an existing GitOps configuration (default false) |
| --sealed-secrets-ns string            | Optional. Namespace in which the Sealed Secrets operator is installed, automatically generated secrets are encrypted with this operator (default "sealed-secrets") |
| --sealed-secrets-svc string           | Optional. Name of the Sealed Secrets Services that encrypts secrets (default "sealedsecretcontroller-sealed-secrets") |
| --status-tracker-access-token string  | Optional. Used to authenticate requests to push commit-statuses to your Git hosting service

The following [directory layout](output) is generated.

```shell
.
├── config
│   ├── argocd
│   │   ├── argo-app.yaml
│   │   ├── argocd.yaml
│   │   ├── cicd-app.yaml
│   │   ├── dev-app-taxi-app.yaml
│   │   └── kustomization.yaml
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
│       │   │   ├── gitops-webhook-secret.yaml
│       │   │   └── webhook-secret-dev-taxi.yaml
│       │   ├── 04-tasks
│       │   │   ├── deploy-from-source-task.yaml
│       │   │   └── deploy-using-kubectl-task.yaml
│       │   ├── 05-pipelines
│       │   │   ├── app-ci-pipeline.yaml
│       │   │   └── ci-dryrun-from-push-pipeline.yaml
│       │   ├── 06-bindings
│       │   │   ├── dev-app-taxi-taxi-binding.yaml
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
├── environments
│   ├── dev
│   │   ├── apps
│   │   │   └── app-taxi
│   │   │       ├── base
│   │   │       │   └── kustomization.yaml
│   │   │       ├── kustomization.yaml
│   │   │       ├── overlays
│   │   │       │   └── kustomization.yaml
│   │   │       └── services
│   │   │           └── taxi
│   │   │               ├── base
│   │   │               │   ├── config
│   │   │               │   │   ├── 100-deployment.yaml
│   │   │               │   │   ├── 200-service.yaml
│   │   │               │   │   └── kustomization.yaml
│   │   │               │   └── kustomization.yaml
│   │   │               ├── kustomization.yaml
│   │   │               └── overlays
│   │   │                   └── kustomization.yaml
│   │   └── env
│   │       ├── base
│   │       │   ├── dev-environment.yaml
│   │       │   ├── dev-rolebinding.yaml
│   │       │   └── kustomization.yaml
│   │       └── overlays
│   │           └── kustomization.yaml
│   └── stage
│       └── env
│           ├── base
│           │   ├── kustomization.yaml
│           │   └── stage-environment.yaml
│           └── overlays
│               └── kustomization.yaml
└── pipelines.yaml

```

