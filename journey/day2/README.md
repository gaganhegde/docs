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

The above command adds a new Environment `new-env` in the Pipelines Model.

```yaml
environments:
- name: new-env
```

It generates the following yamls.

* [`environments/<env-name>/env/base/<env-name>-environment.yaml`](output/environments/new-env/base/new-env-environment.yaml)
* [`environments/<env-name>/env/base/<env-name>-rolebindings.yaml`](output/environments/env-env/base/new-env-rolebindgs.yaml)