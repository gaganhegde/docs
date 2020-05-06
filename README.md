# Odo Pipelines Commands


## Bootstrap 

Bootstrap command creates defualt environments for your initial application.  It outputs resources yaml files, kustomization files, w and Manifest.  The following resources are written to filesystem.
   
* CI/CD envrionment with pipelines and resources
* ArgoCD environment
* Dev envrionment with an applicatin/service
* Stage environment

```shell
$ odo manifest bootstrap \
  --app-repo-url <source Git repository URL> \
  --app-webhook-secret <application webhook secret> \
  --gitops-repo-url <CI/CD pipeline configuration repostory URL> \
  --gitops-webhook-secret <GitOps webhook secret> \
  --image-repo <application image repository> \
  --dockercfgjson ~/Downloads/<username>-auth.json 
```

| Option                  | Description |
| ----------------------- | ----------- |
| --app-repo              | This is the source code to your initial application.   E.g. https://github.com/user/app.git |
| --app-webhook-secret    | Creates a secret used to validate incoming hooks. |
| --gitops-repo-url       | This is where your configuration and manifest live. E.g. https://github.com/user/gitops.git|
| --gitops-webhook-secret | This is used to validate incoming hooks. |
| --image-repo            | Where should we configure your builds to push to? E.g. quay.io/wtam/app|
| --dockercfgjson         | This is used to authenticate image pushes to your image-repo. |
| --prefix                | Optional.  This is used to help separate user namespaces. |
| --output                | Optional.  Output path.  |

The following directory layout is generated.

```shell
.
├── environments
│   ├── demo-argocd
│   │   └── config
│   │       ├── demo-dev-taxi-app.yaml
│   │       └── kustomization.yaml
│   ├── demo-cicd
│   │   ├── base
│   │   │   ├── kustomization.yaml
│   │   │   └── pipelines
│   │   │       ├── 01-namespaces
│   │   │       │   └── cicd-environment.yaml
│   │   │       ├── 02-rolebindings
│   │   │       │   ├── pipeline-service-account.yaml
│   │   │       │   ├── pipeline-service-rolebinding.yaml
│   │   │       │   └── pipeline-service-role.yaml
│   │   │       ├── 03-secrets
│   │   │       │   ├── docker-config.yaml
│   │   │       │   ├── github-webhook-secret-taxi-svc.yaml
│   │   │       │   └── gitops-webhook-secret.yaml
│   │   │       ├── 04-tasks
│   │   │       │   ├── deploy-from-source-task.yaml
│   │   │       │   └── deploy-using-kubectl-task.yaml
│   │   │       ├── 05-pipelines
│   │   │       │   ├── app-ci-pipeline.yaml
│   │   │       │   ├── cd-deploy-from-push-pipeline.yaml
│   │   │       │   └── ci-dryrun-from-pr-pipeline.yaml
│   │   │       ├── 06-bindings
│   │   │       │   ├── github-pr-binding.yaml
│   │   │       │   └── github-push-binding.yaml
│   │   │       ├── 07-templates
│   │   │       │   ├── app-ci-build-pr-template.yaml
│   │   │       │   ├── cd-deploy-from-push-template.yaml
│   │   │       │   └── ci-dryrun-from-pr-template.yaml
│   │   │       ├── 08-eventlisteners
│   │   │       │   └── cicd-event-listener.yaml
│   │   │       ├── 09-routes
│   │   │       │   └── gitops-webhook-event-listener.yaml
│   │   │       └── kustomization.yaml
│   │   └── overlays
│   │       └── kustomization.yaml
│   ├── demo-dev
│   │   ├── apps
│   │   │   └── taxi
│   │   │       ├── base
│   │   │       │   └── kustomization.yaml
│   │   │       ├── kustomization.yaml
│   │   │       └── overlays
│   │   │           └── kustomization.yaml
│   │   ├── env
│   │   │   ├── base
│   │   │   │   ├── demo-dev-environment.yaml
│   │   │   │   ├── demo-dev-rolebinding.yaml
│   │   │   │   └── kustomization.yaml
│   │   │   └── overlays
│   │   │       └── kustomization.yaml
│   │   └── services
│   │       └── taxi-svc
│   │           ├── base
│   │           │   ├── config
│   │           │   │   ├── 100-deployment.yaml
│   │           │   │   ├── 200-service.yaml
│   │           │   │   └── kustomization.yaml
│   │           │   └── kustomization.yaml
│   │           ├── kustomization.yaml
│   │           └── overlays
│   │               └── kustomization.yaml
│   └── demo-stage
│       └── env
│           ├── base
│           │   ├── demo-stage-environment.yaml
│           │   └── kustomization.yaml
│           └── overlays
│               └── kustomization.yaml
└── pipelines.yaml
```

