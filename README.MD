# GitLab + Docker + NimbusServer

## Create docker volume for external config

Not necessary if you don't need persistence.

```
docker volume create --name=gitlab-runner-config
```

## Start containers

```
docker-compose up -d
```

Configured for demo-net.

## Set root password

Use rails console to change the initial root password.
Also includes the gitlab-ctl reconfigure cmd, which is necessary for future config changes to GitLab.

```
docker exec -it gitlab /bin/bash

gitlab-rails console -e production

user = User.where(id: 1).first

user.password = 'pass'
user.password_confirmation = 'pass'

user.save!

reconfigure cmd (should not be needed):
gitlab-ctl reconfigure
```

## Create users

You can create a default user on the GitLab login page (default: http://nimbusserver.aos.com:8929)
Allowing user reg can be disabled in Admin settings, where you can manually create extra users as well.

Login using root, approve or create user in admin section.

## Register runner

Runner section should be available in admin section, instructions to set up shared runner manually should contain host and token information needed.
Runner can be added to group or project, and can be shared or dedicated.  Depends on what you're using the instance for.

This example uses the `docker` runner (as opposed to `shell` or `kubernetes`) and should be privileged by default.

It's a single runner, without auto scaling capabilities.

```
docker exec -it gitlab-runner /bin/bash/

gitlab-runner register
```

>My personal example uses node:latest as the default image, but it doesn't matter as long as we specify an image in the pipeline.


## Modify config.toml in runner container after reg

```
docker exec -it gitlab-runner /bin/bash/

/etc/gitlab-runner/config.toml

in [runners.docker] add:

network_mode = "demo-net"
```

