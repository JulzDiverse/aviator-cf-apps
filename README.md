# Aviator - cf-app

This is an `aviator` repository, which sets up a **dynamic/scalable** `Concourse` pipeline for your Cloud Foundry Apps. The pipeline pushs your app to an Cloud Foundry instance triggered by a git commit.

[Get Aviator](https://github.com/JulzDiverse/aviator#installation)

## Meta Configuration

This pipeline uses the following Concourse resources:

- [Concourse CF-Resource](https://github.com/concourse/cf-resource)
- [Concourse Git-Resource](https://github.com/concourse/git-resource)

To get more information about the settings, please check the README files of the provided concourse resources. 

### Basic Configuration

Open `app/myapp.yml` with the following content, replace the placeholders with your app information and rename the file to match your app name:

```yaml
meta:
  git:
    app-name: <app-name>
    ssh: <git-uri>
    branch: master
    trigger: false
    private-key: |+
      -----BEGIN RSA PRIVATE KEY-----
      MIIEowIBALJDIFOWLEA9K9SJOX/MAG0ivT4JdCljeouau6UUhwzOXaN0C9Fr4vw8
      RSv3SiBRfqZ/maDVz+9J9ex5T7KOBHELSFIJFSLKELFJK/TyX40OF+hOnBb1O91R
      ... some more lines ...
      GqRcQNuaMa6Q5G7bK25/LOAf8B5Dd1mHT7Dm2yp7l0AhTuHklHRz
      -----END RSA PRIVATE KEY-----
  cf:
    api: <api-endpoint>
    username: <cf-username>
    password: <cf-password>
    org: <cf-org>
    space: <cf-space>
 ```

**Git Section** 

- **app-name** (required): the name of your app
- **uri** (required): the git resource URI (either ssh or http)
- **branch** (required): branch of the git resource that should be pushed
- **trigger** (required): Concourse configuration for the git resource. When set to `true` the pipeline will automatically be triggered when a commit is pushed to the given branch.
- **private-key** (optional): ssh key to pull the git resource using ssh.  

**CF Section**

- **api** (required): the API endpoint of the CF instance you're going to push your app to. 
- **username** (required): your cf username 
- **password** (required): your cf password
- **org** (required): your cf organisation, where your is going to be pushed. 
- **space** (required): your cf space, where your app is going to be pushed. 
- **current_app_name:** This should be the name of the application that this will re-deploy over. If this is set the resource will perform a zero-downtime deploy. 
- **docker_username** (optional): This is used as the username to authenticate against a protected docker registry.
- **docker_password** (optional): This should be the users password when authenticating against a protected docker registry.
You can add as many app meta files to the `apps` directory as you want. To uptdate the pipeline with the newly added app just re-execute aviator. 

### Add a preceding Job (e.g for unit-tests)

You can add a preceding job to the cf-push job by adding additionally - beside the `git` and `cf` - the `pre` section to the `meta` section in the app YAML file for an app:

```yaml
meta:
  pre:
    name: <job-name>
    docker-image: <docker image to be used for that job>
    script: <path to the script inside the git repository to be executed>
```

**PRE**

- **name** (required): name of the job (e.g unit-test-myapp)
- **docker-image**: the docker image that should be used to execute the job
- **script**: path to the script that should be executed inside your git repository. For example, if you have a script in your git resource `myapp/scripts/myscript.sh`, your script path would be `scripts/myscript.sh`.  

## Run Aviator

After you setup your meta configuration just execute `aviator` as follows: 

```
$ target=<flytarget> name=<pipeline-name> aviator 
```

Note: make sure you are logged in with `fly` before you execute aviator. 

That's It! ;)

## BEFORE YOU START:

Requirements:

- You have a Concourse server up and running
- You have the Fly CLI installed on your machine. More info about Fly [here](https://concourse.ci/fly-cli.html)
- You have a Cloud Foundry instance you can push your apps to (e.g IBM Cloud - Bluemix, Pivotal Cloud Foundry, Bosh-Lite, etc.)
	- You can create an trial IBM Bluemix account [here](https://console.bluemix.net/)
