# GitLab + Docker + NimbusServer

```
docker-compose up -d
```

Configured for demo-net.

## Set root password

```
gitlab-rails console -e production

user = User.where(id: 1).first

user.password = 'pass'
user.password_confirmation = 'pass'

user.save!
```

## Register runner

```
docker exec -it gitlab-runner /bin/bash/
gitlab-runner -register
```

## Modify config.toml in runner container after reg

```
in [runners.docker] add:

network_mode = "demo-net"
```

