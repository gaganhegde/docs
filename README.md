# GitOps Documentation

## 1. What is Gitops?
* GitOps is a natural evolution of Agile and DevOps methodologies.
* GitOps is when the infrastructure and/or application state is fully represented by the contents of a git repository. Any changes to the git repository are reflected in the corresponding state of the associated infrastructure and applications through automation.

## 2. Principles behind Gitops?
* Git as the source of truth.
* Separate application source code (Java/Go) from deployment manifests i.e the application source code and the gitops configuration reside on seperate git repositories.
* Deployment manifests are standard Kubernetes (k8s) manifests i.e kuberenetes manifests in the Gitops repository can be simply applied with nothing more than a kubectl apply.
* Kustomize for defining the differences between environments i.e resuable parameters with extra resources described using kustomization.yaml

## 3. Why Gitops?
* Audit all changes made to pipelines, infrastructure, application make-up
* Roll forward/back to desired state in case of issues
* Consistent way to configure all environments
* Reduce manual effort by automating application and environment setup, remediation
* Easier to manage application and infrastructure state across clusters/environments

## 4. How to get started?
* [Pipelines Model (aka manifest model)](model)
* [GitOps Day 1 Operations](journey/day1)
* [GitOps Day 2 Operations](journey/day2)
* [Odo Pipelines Command Reference](commands)
* [Mock GitOps Repo](https://github.com/ishitasequeira/gitops)

## Disclaimer
GitHub and GitLab are supported.  However, only one Git type is supported in a setup.  Git type is determined by the GitOps Reposiotry URL used during bootstrapping/initialization.  For example, if GitHub repostory URL is specificed during bootstraping, all service/application Git repsostiories must be GitHub repositories.
