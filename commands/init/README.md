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
| --dockercfgjson                       | Filepath to config.json which authenticates the image push to the desired image registry. |
| --gitops-repo-url                     | Provide the URL for your GitOps repository e.g. https://github.com/organisation/repository.git |
| --gitops-webhook-secret               | Optional. Provide a secret that we can use to authenticate incoming hooks from your Git hosting service for the GitOps repository. (if not provided, it will be auto-generated)|
| --help                                | Help for init flags |
| --image-repo                          | Image repository of the form <registry>/<username>/<repository> or <project>/<app> which is used to push newly built images |
| --internal-registry-hostname          | Host-name for internal image registry e.g. docker-registry.default.svc.cluster.local:5000, used if you are pushing your images to the internal image registry |
| --output                              | Path to write GitOps resources (default is the current working directory|
| --prefix                              | Add a prefix to the environment names(Dev, stage,prod,cicd etc.) to distinguish and identify individual environments. |
| --overwrite                           | Optional. Overwrites previously existing GitOps configuration (if any) (default false) |
| --sealed-secrets-ns string            | Optional. Namespace in which the Sealed Secrets operator is installed, automatically generated secrets are encrypted with this operator (default "kube-system")|
|  --sealed-secrets-ns string           | Optional. Namespace in which the Sealed Secrets operator is installed, automatically generated secrets are encrypted with this operator (default "kube-system") |
| --sealed-secrets-svc string           | Optional. Name of the Sealed Secrets Services that encrypts secrets (default "sealed-secrets-controller") |
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

