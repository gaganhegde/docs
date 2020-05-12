# Pipelines Model (aka Manifest)

The Pipelines Model represents an inventory of _Environments_, _Applications_ and _Services_ within a GitOps setup.  This model allows tools to perform operations on Gitops setup without the needs to query cluster resources which is often hard if not impossible to do.  The actual execution of operations is still left to Git PullRequests to drive.

Within a GitOps setup, there can be many [Environments](#Environment), [Applications](#Application), and [Services]/(#Service).  The folloing diagram shows the structure of the Pipelines Model.


![Manifest Model](img/pipelines_model.png)
 

Pipelines Model is defined in a yaml file.   Here is an example a pipelines.yaml

```yaml
environments:
- apps:
  - name: service
    services:
    - service-svc
  name: dev
  pipelines:
    integration:
      bindings:
      - github-pr-binding
      template: app-ci-template
  services:
  - name: service-svc
    source_url: https://github.com/user/service.git
    webhook:
      secret:
        name: github-webhook-secret-service-svc
        namespace: cicd
- name: stage
- cicd: true
  name: cicd
- argo: true
  name: argocd
gitops_url: https://github.com/user/gitops.git
```

## Environment

There are three types of Environments
* CI/CD Environment
* ArgoCd Environment 
* (Plain old) Environment

### CI/CD Environment

The CI/CD Environment is a special Evironment that contains CI/CD pipelines.   These pipelines response to changes in GitOps configuraiton repository and Application/Service soruce repositories.   They are responisble for keeping the resources in-sync with the configurations in Git and re-build/re-deploy application/servcie images.

### ArgoCD Environment

ArgoCD is used to perform Continuous Delivery of Applications.   When an Application is created in the target Environment.  An ArgoCD application is also created and kept in the ArgoCCD Environment.  The user is reponsible to create deployment.yaml in the "config" folder for the application.  ArgoCD will deploy the application based on the user-provided deployment specification and re-deploy it automatically when the specification is changed.

### (Plain Old) Enviroment

Within a Pipelines Model, there are many Enviroments which holds Applications and Servcies.  Each Environment has its own namespacce.

## Application

An Application is a logical grouping of Services.  It contains references to Services.   When an Application is deployed, all referenced Services are deployed.   Two Applications can reference to a same Service.  They can have their own customerization to the Service they deploy.  A Service cannot be deployed by itself (without an Application).

## Service

Services are unique within an Anvironment.  No two Services can share a same source Git reposiotry even though they belong to different Environments.