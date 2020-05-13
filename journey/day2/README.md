# Day 2 Operations

Day 2 Operations are actions that users take to  change a GitOps system. 
Currently, the following odo commands are available to allow users to add new
Evnrionments and Applications/Services.

* [odo pipelines environment](../../commands/environment)
* [oad pipelines service](../../commands/service)
* [oad pipelines webhook](../../commands/webhook)


## Prerequisites

* A GitOps system that was bootstrapped in [Day 1 Operations](../day1)
* A new soruce Git repository
* Download unofficial [odo](../../commands/bin) binary

## Create a new Environment

```shell
$ odo pipelines environment add \
  --env-name new-env
```

| Option                  | Description |
| ----------------------- | ----------- |
| --env-name  | New environment name |

The above command adds a new Environment `new-env` in the Pipelines Model.

```yaml
environments:
- name: new-env
```

It generates the following yamls.  The new resources are namespace and role bindings.

* [`environments/<env-name>/env/base/<env-name>-environment.yaml`](output/environments/new-env/env/base/new-env-environment.yaml)
* [`environments/<env-name>/env/base/<env-name>-rolebindings.yaml`](output/environments/new-env/env/base/new-env-rolebindgs.yaml)

## Create an Application/Service in the new Environment

```shell
$ odo pipelines service add \
  --env-name new-env \
  --app-name bus \
  --service-name bus-svc \
  --git-repo-url http://github.com/wtam2018/bus.git \
  --webhook-secret testing 
```

| Option                  | Description |
| ----------------------- | ----------- |
| --env-name  | Environment name. |
| --app-name  | Application name.  A new Application will be created if it does not exist already.|
| --service-name  | New Service Application name. |
|  --git-repo-url   | Source Git repository URL. |
|  --webhook-secret   | Webhook secret for the Git repository |

