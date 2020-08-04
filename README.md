# GitOps for Application Delivery

## Why

As a software organization, I would like to:

* Audit all changes made to pipelines, infrastructure, application make-up
* Roll forward/back to desired state in case of issues
* Consistently configure all environments
* Reduce manual effort by automating application and environment setup, remediation
* Have an easy way to manage application and infrastructure state across clusters/environments

## What

* GitOps is a natural evolution of Agile and DevOps methodologies.
* GitOps is when the infrastructure and/or application state is fully represented by the contents of a git repository. Any changes to the git repository are reflected in the corresponding state of the associated infrastructure and applications through automation.


## Principles

* Git as the source of truth.
* Separate application source code (Java/Go) from deployment manifests i.e the application source code and the gitops configuration reside on seperate git repositories.
* Deployment manifests are standard Kubernetes (k8s) manifests i.e kuberenetes manifests in the Gitops repository can be simply applied with nothing more than a kubectl apply.
* Kustomize for defining the differences between environments i.e resuable parameters with extra resources described using kustomization.yaml



## Getting Started

### Steps

The following steps will guide you to setup GitOps for application delivery:

1. [GitOps Day 1 Operations](journey/day1): Install the prequisites and setup your Gitops Pipeline.
2. [GitOps Day 2 Operations](journey/day2): Continue adding more applications.


### Reference

The command reference to generate and manage the above can be found [here](commands).
This project provides the tooling to generate the recommended directories and manifests as per the [Application Pipelines Model] (model). 


### Sample

A sample Gitops repository can be found [here](https://github.com/rhd-gitops-example/gitops).


## Business Scenario

A company called *Pet Clinic Supplies Inc* has a big vision but limited investment, they decide to start developing code for their online portal that currently sells supplies for pet clinics.

They begin by developing code using legacy systems with five engineers who contribute to their own version control system/database and perform testing, quality and security checks themselves, all the code and business decisions can be reviewed within the team. Soon they start to notice a sudden surge in the business and to broaden their business they decide to incorporate more commodities (i.e hardware). To make this happen they now need more developers and realize that their current methods are no longer working well for them. Reason being they cant really tell who makes or breaks their code, accountability is lost.

They now need a quick fix, on a quick google search they decide to incorporate Gitops, as a [GitOps Day 1 Operations](journey/day1) they want to quickly get started to get their teams up and running . This is when they use our Gitops tools odo bootstrap command to quickly set up their dev, stage and prod environment along with the config repos that create a pipeline for seamless CI/CD with the bootstrapped environments incorporating the [Pipelines Model (aka manifest model)](model). Now the management is happy as they can keep track of the different teams within the organization that are making changes to  their code base and can hold the teams accountable for their actions.

Now that we’ve given the team its ability and power to use version control to monitor progress on its different teams/environments , the engineering management decides to make modifications to its current bootstrapped Gitops configuration, they want to incorporate their own online wallet for users to shop and spend more easily. But this requires the use of an additional security environment and that has specific micro-services to test traffic or test vulnerability only particular to this environment, the management has to look no further as they can leverage the use of our [GitOps Day 2 Operations](journey/day2), simply running the add environment command to add their security environment , and chose the services they wish to add to the environment with a simple service add command.

The bootstrapped environment sets up the necessary pipelines using Tekton required for developers/teams to perform CI with their respective environments they are allocated to , they can then check the progress of their development as CD for the applications is performed using Argocd constantly maintaining the application on the cluster. Giving users the ability to simply modify the pipelines to their own needs at any point to a more customized approach. A simple push to the Gitops repo can alter the configuration of the Gitops repo and hence the style by which you want to run your organisation, by triggering a CI pipeline , further a merge to master triggers a deployment that further changes the state of your application on the cluster. Giving teams to use the power of Openshift/kubernetes to run powerful applications.

Now we have an a team that set out as a start up but went on to be a full fledged organization with a multiple verticals and business set up in different geographical locations leveraging the use of open shift and Gitops with “ Git as the source of truth”. Now's your chance to ramp up your organization using our modernised approach for end to end application delivery. A simple gitops configuration can be seen [here](https://github.com/ishitasequeira/gitops), all achieved with a list of commands listed in [Odo Pipelines Command Reference](commands) , why wait lets get started.

## Disclaimer
GitHub and GitLab are supported.  However, only one Git type is supported in a setup.  Git type is determined by the GitOps Reposiotry URL used during bootstrapping/initialization.  For example, if GitHub repostory URL is specificed during bootstraping, all service/application Git repsostiories must be GitHub repositories.
