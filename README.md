# Aviator - cf-app

An `aviator` repository, which sets up a `Concourse` pipeline for your Cloud Foundry Apps. The pipeline pushs your app to Cloud Foundry on a git-commit.

## Meta Configuration

Open `app/myapp.yml` with the following content, replace the placeholders with your app information and rename the file to match your app name:

```
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

You can add as many different app meta files to the `app` directory as you want. The pipeline will be udpated with the new app.

## Run Aviator

After you setup your meta configuration you can download the aviator cli tool (if not already happend) and execute the `aviator` cli tool: 

```
$ aviator 
```

That's It! ;)
