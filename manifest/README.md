# Manifest Model

The manifest models environments, applications and services.

An environment can have many applications, which in-turn can reference many
services.

Services are "per-environment", and multiple applications _within_ an
environment can reference the same service, and override the configuration with
Kustomize.

```yaml
environments:
- name: tst-dev
  pipelines:
    integration:
      binding: github-pr-binding
      template: app-ci-template
  apps:
  - name: taxi
    services:
    - name: taxi-svc
      source_url: https://github.com/<username>/taxi.git
      webhook:
        secret:
          name: github-webhook-secret-taxi-svc
- name: tst-stage
- name: tst-cicd
  cicd: true
- name: tst-argocd
  argo: true
```

The bootstrap creates four environments, `dev`, `stage`, `cicd` and `argocd`
with a user-supplied prefix.

Namespaces are generated for the `dev`, `stage` and `cicd` environments.

The name of the app and service are derived from the last component of your
`app-repo-url` e.g. if your bootstrap with `--app-repo-url
https://github.com/myorg/myproject.git` this would bootstrap an app called
`myproject` and a service called `myproject-svc`.

## Environment configuration

The `tst-dev` environment created is the most visible configuration.

### configuring pipelines

```yaml
environments:
- name: tst-dev
  pipelines:
    integration:
      binding: github-pr-binding
      template: app-ci-template
  apps:
  - name: taxi
    services:
    - name: taxi-svc
      source_url: https://github.com/<username>/taxi.git
      webhook:
        secret:
          name: github-webhook-secret-taxi-svc
```

The `pipelines` key describes how to trigger a Tekton PipelineRun, the
`integration` binding and template are processed when a _Pull Request_
is opened.

This is the default pipeline specification for the `tst-dev` environment, you
can find the definitions for these in these two files:

 * `environments/<prefix>cicd/base/pipelines/07-templates/app-ci-build-pr-template.yaml`
 * `environments/<prefix>cicd/base/pipelines/06-bindings/github-pr-binding.yaml`

By default this triggers a PipelineRun of this pipeline

 * `environments/<prefix>cicd/base/pipelines/05-pipelines/app-ci-pipeline.yaml`

These files are not managed directly by the manifest, you're free to change them
for your own needs, by default they use [Buildah](https://github.com/containers/buildah)
to trigger build, assuming that the Dockerfile for your application is in the root
of your repository.

### configuring services

```yaml
apps:
- name: taxi
  services:
  - name: taxi-svc
    source_url: https://github.com/<username>/taxi.git
    webhook:
      secret:
        name: github-webhook-secret-taxi-svc
```