# GitLab + Docker + NimbusServer

## Create docker volume for external config
```
docker volume create --name=gitlab-runner-config
```

## Start containers

```
docker-compose up -d
```

Configured for demo-net.

## Set root password

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

## Register runner

```
docker exec -it gitlab-runner /bin/bash/

gitlab-runner -register
```

## Modify config.toml in runner container after reg

```
docker exec -it gitlab-runner /bin/bash/

/etc/gitlab-runner/config.toml

in [runners.docker] add:

network_mode = "demo-net"
```

