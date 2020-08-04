---
category: documentation
title: FAQ
---

# FAQ


## What is Gitops?
GitOps is a way to do Kubernetes cluster management and application delivery.  It works by using Git as a single source of truth for declarative infrastructure and applications

## How does GitOps differ from Infrastructure as Code?
GitOps builds on top of Infrastructure as Code, providing application level concerns, as well as an operations model.

## Can I use a CI server to orchestrate convergence in the cluster?
You could apply updates to the cluster from the CI server, but it won’t continuously deploy the changes to the cluster, which means that drift won’t be detected and corrected.

## Should I abandon my CI tool?
No, you need CI to validate the changes that GitOps is applying.

## Why is a PULL vs a PUSH pipeline important?
This is the difference between orchestration and choreography, in a PUSH pipeline, code is pushed by scripts between each stage of the pipeline, in a PULL pipeline, changes are detected and automatically pulled into the next stage.

## Why choose Git and not a Configuration Database instead? / Why is git the source of truth?
Git has strong auditability, and it fits naturally into a developer’s flow.

## How do you manage secrets?
We are going with Sealed Secrets because of it's low-maintenance, and because it requires little investment to get going, you need to consider that anything you put into Git might get leaked at some point, so if you’re keeping secrets in there, they might be made publicly available.

## How do I get started?
Add some resources to a directory, and git commit and push, then ask ArgoCD to deploy the repository, change your resource, git commit and push, and the change should be deployed automatically.

## Why are Tekton pipelines used/ are important?
They’re not particularly important, they can fulfil the Integration Server part of the components of a successful GitOps implementation.


## Can I use SVN for GitOps?
Yes, why not? Ultimately if you can version your resources, and apply them to the cluster.
Gitops-engine could easily be adapted to run on-top of a svn checkout, rather than a Git checkout, GitOps-engine relies on a Git sidecar to update the repository that is parsed for the resources to be applied to the cluster https://github.com/argoproj/gitops-engine/tree/master/agent#customize-git-repository .

## How is gitOps different from devOps?
GitOps is a subset of DevOps, specifically focussed on deploying the application (and infrastructure) through a Git flow-like process.

Gitops = devops - system admins
Devops is currently more widely followed than gitops 

## How could small teams benefit from GitOps?
 GitOps is about automating the application life-cycle, with more automation, it frees up developers to work on the product features that customers love.

