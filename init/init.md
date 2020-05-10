# Odo Pipelines Init Command


## Init 

Init command creates CI/CD environments for you.  Unlinke the [bootstrap](bootstrap.md) command, it does not create your first application and other environments.   It outputs resources yaml files, kustomization files, and Manifest.  The following resources are written to filesystem.
   
* CI/CD envrionment with pipelines and resources

```shell
$ odo pipelines bootstrap \
  --gitops-repo-url <CI/CD pipeline configuration repostory URL> \
  --gitops-webhook-secret <GitOps webhook secret> \
  --image-repo <application image repository> \
  --dockercfgjson ~/Downloads/<username>-auth.json 
```

| Option                  | Description |
| ----------------------- | ----------- |
| --gitops-repo-url       | This is where your configuration and manifest live. E.g. https://github.com/user/gitops.git|
| --gitops-webhook-secret | This is used to validate incoming hooks. |
| --image-repo            | Where should we configure your builds to push to? E.g. quay.io/wtam/app|
| --dockercfgjson         | This is used to authenticate image pushes to your image-repo. |
| --prefix                | Optional.  This is used to help separate user namespaces. |
| --output                | Optional.  Output path.  |

The following [directory layout](output) is generated.

```shell
.
├── environments
│   └── [cicd]
│       ├── base
│       │   ├── kustomization.yaml
│       │   └── pipelines
│       │       ├── 01-namespaces
│       │       │   └── cicd-environment.yaml
│       │       ├── 02-rolebindings
│       │       │   ├── pipeline-service-rolebinding.yaml
│       │       │   └── pipeline-service-role.yaml
│       │       ├── 03-secrets
│       │       │   └── gitops-webhook-secret.yaml
│       │       ├── 04-tasks
│       │       │   ├── deploy-from-source-task.yaml
│       │       │   └── deploy-using-kubectl-task.yaml
│       │       ├── 05-pipelines
│       │       │   ├── app-ci-pipeline.yaml
│       │       │   ├── cd-deploy-from-push-pipeline.yaml
│       │       │   └── ci-dryrun-from-pr-pipeline.yaml
│       │       ├── 06-bindings
│       │       │   ├── github-pr-binding.yaml
│       │       │   └── github-push-binding.yaml
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
└── pipelines.yaml

```

