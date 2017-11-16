# Aviator - cf-app

An `aviator` repository, which sets up a `Concourse` pipeline for your Cloud Foundry Apps. The pipeline pushs your app to Cloud Foundry triggered by commit.

[Get Aviator](https://github.com/JulzDiverse/aviator#installation)

## Meta Configuration

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

You can add as many app meta files to the `apps` directory as you want. To uptdate the pipeline with the newly added app just re-execute aviator. 

### Add a preceding Job (e.g for unit-tests)

You can add a preceding job to the cf-push job by adding additionally beside the `git` and `cf`, the `pre` section to the `meta` section in the app YAML file for an app:

```yaml
meta:
  pre:
    name: <job-name>
    docker-image: <docker image to be used for that job>
    script: <path to the script inside the git repository to be executed>
```

## Run Aviator

After you setup your meta configuration just execute `aviator` as follows: 

```
$ target=<flytarget> name=<pipeline-name> aviator 
```

Note: make sure you are logged in with `fly` before you execute aviator. 

That's It! ;)
