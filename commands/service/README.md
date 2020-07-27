# Odo Pipelines Service Command

## Service add

_Service_add_ command adds a new service to an existing environment.  Services are logically grouped by applications.   This command will create an application for the new service if the target application does not exist.  It outputs resources yaml files, kustomization files, and updated Manifest to filesystem.  

**NOTE**: Service deployment resources are not generated.  They must be manually added to `environments/<env-new>/ services/<service-name>/base/config` and update the kustomization file `environments/<env-new>/ services/<service-name>/base/kustomization.yaml`

```shell
$ odo pipelines service add 
    --env-name 
    --app-name 
    --service-name
    [--git-repo-url]
    [--sealed-secrets-ns]
    [--webhook-secret]
    [--image-repo]
    [--internal-registry-hostname]
    [--pipelines-file]
```

| Flag                    | Description |
| ----------------------- | ----------- |
| --app-name | Name of the application which the service will be added.|
| --env-name | Name of the environment which the service will be added.|
| --git-repo-url | Optional.  Source Git repository URL.  It must be unique winith GitOps.|
| --help | Show help|
| --image-repo | Optional. Used to push built images|
|--internal-registry-hostname| Optioanl.  Internal image registry hostname (default "image-registry.openshift-image-registry.svc:5000") |
| --pipelines-file | Optional.  Path to pipelines file.  Default is _pipelines.yaml_. |
| --sealed-secrets-ns | Optional. Namespace in which the Sealed Secrets operator is installed, automatically generated secrets are encrypted with this operator (default "kube-system") |
| --service-name | Name of the service to be added.  Service name must be unique within an environment. |
| --webhook-secret | Optional.  Wehook secret of the source Git repository URL. It is required if git-repo-url is provided|



The following [directory layout](output) is generated.

```shell

.
├── apps
│   └── app-bus
│       ├── base
│       │   └── kustomization.yaml
│       ├── kustomization.yaml
│       ├── overlays
│       │   └── kustomization.yaml
│       └── services
│           └── bus
│               ├── base
│               │   └── kustomization.yaml
│               ├── kustomization.yaml
│               └── overlays
│                   └── kustomization.yaml
└── env
    ├── base
    │   ├── kustomization.yaml
    │   ├── new-env-environment.yaml
    │   └── new-env-rolebinding.yaml
    └── overlays
        └── kustomization.yaml

```
  
