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

It generates the following yamls.  The new resources are namespace and role bindings.

* [`environments/<env-name>/env/base/<env-name>-environment.yaml`](output/environments/new-env/env/base/new-env-environment.yaml)
* [`environments/<env-name>/env/base/<env-name>-rolebindings.yaml`](output/environments/new-env/env/base/new-env-rolebindgs.yaml)

## Create an Application/Service in the new Environment

```shell
$ odo pipelines service add \
  --env-name new-env \
  --app-name bus \
  --service-name bus-svc \
  --git-repo-url http://github.com/<user>/bus.git \
  --webhook-secret testing 
```

**NOTE**: DO NOT use `testing` as your secrets.

Per the (GitHub documentation)[https://developer.github.com/webhooks/securing/]
you should generate a secret for each of them:

```shell
$ ruby -rsecurerandom -e 'puts SecureRandom.hex(20)'
```

The above command adds a new Service and Application under new-env in the Pipelines Model.

```yaml
environments:
- apps:
  - name: bus
    services:
    - bus-svc
  name: new-env
  services:
  - name: bus-svc
    source_url: http://github.com/<user>/bus.git
    webhook:
      secret:
        name: github-webhook-secret-bus-svc
        namespace: tst-cicd
```

It generates the following yamls.  The new resources are namespace and role bindings.

* [`environments/new-env/apps/bus/base/kustomization.yaml`](output/environments/new-env/apps/bus/base/kustomization.yaml)

* [`environments/new-env/services/bus-svc/base`](output/environments/new-env/services/bus-svc/base)






