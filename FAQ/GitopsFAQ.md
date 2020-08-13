# Frequently Asked Questions

## What is Gitops?

_GitOps is a way to do Kubernetes cluster management and application delivery.  It works by using Git as a single source of truth for declarative infrastructure and applications_

## How does GitOps differ from Infrastructure as Code?
_GitOps builds on top of Infrastructure as Code, providing application level concerns, as well as an operations model_.

## Can I use a CI server to orchestrate convergence in the cluster?
_You could apply updates to the cluster from the CI server, but it won’t continuously deploy the changes to the cluster, which means that drift won’t be detected and corrected._

## Should I abandon my CI tool?
_No, you need CI to validate the changes that GitOps is applying._

## Why is a PULL vs a PUSH pipeline important?
_This is the difference between orchestration and choreography, in a PUSH pipeline, code is pushed by scripts between each stage of the pipeline, in a PULL pipeline, changes are detected and automatically pulled into the next stage._

## Why choose Git and not a Configuration Database instead? / Why is git the source of truth?
_Git has strong auditability, and it fits naturally into a developer's flow._

## How do you manage secrets?
_We are going with Sealed Secrets because of it's low-maintenance, and because it requires little investment to get going, you need to consider that anything you put into Git might get leaked at some point, so if you’re keeping secrets in there, they might be made publicly available._

## How do I get started?
_Add some resources to a directory, and git commit and push, then ask ArgoCD to deploy the repository, change your resource, git commit and push, and the change should be deployed automatically._

## How are OpenShift pipelines used?
_They are used in the default setup to drive the CI from pushes to your application code repository_.

## How is GitOps different from DevOps?
_GitOps is a subset of DevOps, specifically focussed on deploying the application (and infrastructure) through a Git flow-like process._

## How could small teams benefit from GitOps?
_GitOps is about speeding up application feedback loops, with more automation, it frees up developers to work on the product features that customers love._

