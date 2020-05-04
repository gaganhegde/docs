# WIP
### Command structures [inner loop and outer loop end-to-end](https://docs.google.com/presentation/d/1wyXQpL1dOASaNO1VFK3SiIA606IWRN6WNhCJxXv34OY)
## odo manifest bootstrap 

Generate pipeline resources yaml files for the following environments.   
* CI/CD envrionment with pipelines and other resources
* Dev envrionment with an applicatin/service 
* Stage environment 

```shell
$ odo manifest bootstrap \
  --app-repo-url <source Git repository URL> \
  --app-webhook-secret <application webhook secret> \
  --gitops-repo-url <CI/CD pipeline configuration repostory URL> \
  --gitops-webhook-secret <GitOps webhook secret> \
  --image-repo <application image repository> \
  --dockercfgjson ~/Downloads/<username>-auth.json 
```

| Option                  | Description |
| ----------------------- | ----------- |
| --app-repo              | This is the source code to your first application.   E.g. https://github.com/user/app.git |
| --app-webhook-secret    | Creates a secret used to validate incoming hooks. |
| --gitops-repo-url       | This is where your configuration and manifest live. E.g. https://github.com/user/gitops.git|
| --gitops-webhook-secret | This is used to validate incoming hooks. |
| --image-repo            | Where should we configure your builds to push to? E.g. quay.io/wtam/app|
| --dockercfgjson         | This is used to authenticate image pushes to your image-repo. |
| --prefix                | Optional.  This is used to help separate user namespaces. |
| --output                | Optional.  Output path.  |
