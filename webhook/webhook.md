# Odo Pipelines Webhook Command


## Webhook 

* [webhook create](#Wehbook-create)
* [webhook delete](#Webhook-delete)
* [webhook list](#Webhook-list)

## Webhook create

_Webhook create_ command creates a webhook on target Git repository using secret and Event Listener address retrieved from cluster.   If a webhook (with the same Event Listener address URL) already exists, a webhook will not be created.  Otherwise, a webhook will be created and the ID of the new webhook is written to standard output.

```shell
$ odo pipelines webhook create 
    --access-token 
    [--cidi] | [--env-name --service-name]
    [--manifest]
```
| Option                  | Description |
| ----------------------- | ----------- |
| --access-token | Access token to be used to operate on Git repository.|
| --cidi, --env-name, --service-name | Specify --cicd flag if the target Git repository is a CI/CD configuration repository.  Otherwise, provide environment and service names if the target Git repository is a service's source repository.  Either --cicd or both --env-name and --service-name must be provided.|  
| --manifest | Optional.  Path to manifest file.  Default is _pipelines.yaml_. |

## Webhook delete

_Webhook delete_ command deletes all webhooks from Git repository that contains the target Event Listener address.  Event Listener address is retrieved from cluster based on the options passed to the command.  The IDs of the deleted webhooks will be written to standard output.

```shell
$ odo pipelines webhook delete 
    --access-token 
    [--cidi] | [--env-name --service-name]
    [--manifest]
```
| Option                  | Description |
| ----------------------- | ----------- |
| --access-token | Access token to be used to operate on Git repository.|
| --cidi, --env-name, --service-name | Specify --cicd flag if the target Git repository is a CI/CD configuration repository.  Otherwise, provide environment and service names if the target Git repository is a service's source repository.  Either --cicd or both --env-name and --service-name must be provided.|  
| --manifest | Optional.  Path to manifest file.  Default is _pipelines.yaml_. |

## Webhook list

_Webhook list_ command displays webhook IDs of Git repository that contains the target Event Listener address.  Event Listener address is retrieved from cluster based on the options passed to the command.  The IDs of the found webhooks will be written to standard output.

```shell
$ odo pipelines webhook list 
    --access-token 
    [--cidi] | [--env-name --service-name]
    [--manifest]
```
| Option                  | Description |
| ----------------------- | ----------- |
| --access-token | Access token to be used to operate on Git repository.|
| --cidi, --env-name, --service-name | Specify --cicd flag if the target Git repository is a CI/CD configuration repository.  Otherwise, provide environment and service names if the target Git repository is a service's source repository.  Either --cicd or both --env-name and --service-name must be provided.|  
| --manifest | Optional.  Path to manifest file.  Default is _pipelines.yaml_. |