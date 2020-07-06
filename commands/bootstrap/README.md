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

```

| Flag                         | Description |
| ---------------------------- | ----------- |
| --dockercfgjson              | This is used to authenticate image pushes to your image-repo. |
| --gitops-repo-url            | This is where your configuration and pipelines live. |
| --gitops-webhook-secret      | This is used to validate incoming hooks.  (if not provided, it will be auto-generated)|
| --help                       | Help for bootstrap|
| --image-repo                 | Where should we configure your builds to push to? |
| --internal-registry-hostname | Internal image registry hostname (default "image-registry.openshift-image-registry.svc:5000") |
| --output                     | Folder path to add Gitops resources (default ".") |
| --prefix                     | This is used to help separate user namespaces. |
| --sealed-secrets-ns          | Namespace in which the Sealed Secrets operator is installed, automatically generated secrets are encrypted with this operator (default "kube-system")|
| --service-repo-url           | This is the source code to your first application. |
| --service-webhook-secret     | Creates a secret used to validate incoming hooks. (if not provided, it will be auto-generated)|

The following [directory layout](output) is generated.

```shell
.
├── config
│   ├── argocd
│   │   └── config
│   │       ├── kustomization.yaml
│   │       └── tst-dev-app-taxi-app.yaml
│   └── tst-cicd
│       ├── base
│       │   ├── kustomization.yaml
│       │   └── pipelines
│       │       ├── 01-namespaces
│       │       │   ├── cicd-environment.yaml
│       │       │   └── proj.yaml
│       │       ├── 02-rolebindings
│       │       │   ├── internal-registry-proj-binding.yaml
│       │       │   ├── pipeline-service-account.yaml
│       │       │   ├── pipeline-service-rolebinding.yaml
│       │       │   └── pipeline-service-role.yaml
│       │       ├── 03-secrets
│       │       │   ├── docker-config.yaml
│       │       │   ├── gitops-webhook-secret.yaml
│       │       │   └── webhook-secret-tst-dev-taxi.yaml
│       │       ├── 04-tasks
│       │       │   ├── deploy-from-source-task.yaml
│       │       │   └── deploy-using-kubectl-task.yaml
│       │       ├── 05-pipelines
│       │       │   ├── app-ci-pipeline.yaml
│       │       │   ├── cd-deploy-from-push-pipeline.yaml
│       │       │   └── ci-dryrun-from-pr-pipeline.yaml
│       │       ├── 06-bindings
│       │       │   ├── github-pr-binding.yaml
│       │       │   ├── github-push-binding.yaml
│       │       │   └── tst-dev-taxi-binding.yaml
│       │       ├── 07-templates
│       │       │   ├── app-ci-build-pr-template.yaml
│       │       │   ├── cd-deploy-from-push-template.yaml
│       │       │   └── ci-dryrun-from-pr-template.yaml
│       │       ├── 08-eventlisteners
│       │       │   └── cicd-event-listener.yaml
│       │       ├── 09-routes
│       │       │   └── gitops-webhook-event-listener.yaml
│       │       └── kustomization.yaml
│       └── overlays
│           └── kustomization.yaml
├── environments
│   ├── tst-dev
│   │   ├── apps
│   │   │   └── app-taxi
│   │   │       ├── base
│   │   │       │   └── kustomization.yaml
│   │   │       ├── kustomization.yaml
│   │   │       └── overlays
│   │   │           └── kustomization.yaml
│   │   ├── env
│   │   │   ├── base
│   │   │   │   ├── kustomization.yaml
│   │   │   │   ├── tst-dev-environment.yaml
│   │   │   │   └── tst-dev-rolebinding.yaml
│   │   │   └── overlays
│   │   │       └── kustomization.yaml
│   │   └── services
│   │       └── taxi
│   │           ├── base
│   │           │   ├── config
│   │           │   │   ├── 100-deployment.yaml
│   │           │   │   ├── 200-service.yaml
│   │           │   │   └── kustomization.yaml
│   │           │   └── kustomization.yaml
│   │           ├── kustomization.yaml
│   │           └── overlays
│   │               └── kustomization.yaml
│   └── tst-stage
│       └── env
│           ├── base
│           │   ├── kustomization.yaml
│           │   └── tst-stage-environment.yaml
│           └── overlays
│               └── kustomization.yaml
└── pipelines.yaml

```

