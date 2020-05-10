# Odo Pipelines Environment Command


## Environment 

## Environment add

_Environment add_ command creates a new environment in an existing GitOps setup.  It outputs resources yaml files, kustomization files, and updated Manifest.  The following resources are written to filesystem.
   
* A new envrionment with resources

```shell
$ odo pipelines environment add 
  --env-name 
  --manifest 
```

| Option                  | Description |
| ----------------------- | ----------- |
| --env-name              | The name of environment to be added|
| --manifest    | Optional.  Path to manifest file.  Default is pipelines.yaml. |


The following [directory layout](output) is generated.

```shell
.
└── environments
    └── new-env
        └── env
            ├── base
            │   ├── kustomization.yaml
            │   └── new-env-environment.yaml
            └── overlays
                └── kustomization.yaml
```
  