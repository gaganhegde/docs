# Day 1 Operations

Day 1 Operations are actions that users take to  bootstrap a GitOps system.   Bootstrapping GitOps can be done in one of the two commands.

* [odo pipelines bootstrap](../../command_reference/bootstrap)
* [oad pipelines init](../../command_reference/init)

These commands are similar.  They are differ in what gets genreated.  The boostrap command generated a functional GitOps setup including your first application.  The init command only generates CI/CD pipelines and associated resources.  This document describes how to bootstrap GitOps to deliver your first application.

## Prerequisites

You need to have the following installed in the OCP 4.x cluster.
* OpenShift Pipelines Operators
* [ArgoCD](prerequisites/argocd)
* [Sealed Secrets](prerequisites/sealed_secrets)






