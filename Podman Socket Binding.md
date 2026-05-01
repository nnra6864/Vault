---
created: 2026-05-01 10:26
tags:
  - note
  - guide
  - tip
  - container
  - podman
  - server
  - hosting
aliases:
  - PINP
  - Podman In Podman
---
Podman Socket Binding, often referred to as Podman in Podman, is a common approach for running jobs inside of a container, e.g. [[Forgejo Actions]].

This behaves similarly to [[Docker In Docker]], but it's not the same.

And best of all, it **doesn't** require root.

> [!NOTE]
> It's important to mention that unlike [[Docker]], this doesn't run an additional [[Podman]] instance within your container, but instead uses your system Podman.
> 
> Think of nested containers running like sibling containers to your system ones, they are not actually nested.
> 
> This is called [[Socket Binding]].

> [!WARNING]
> Binding the Podman socket gives the container full control over your user containers.
> 
> Only use this with trusted images, as it effectively grants container management access to the host.

## Socket Listener

Start the service that listens for API commands at Podman's socket:

1. Rootless:
	```sh
	systemctl --user enable --now podman.socket
	```
2. Rootful:
	```sh
	systemctl enable --now podman.socket
	```

Get the socket path:

1. Rootless:
	```sh
	podman info | grep sock
	```
2. Rootful:
	```sh
	sudo podman info | grep sock
	```

> [!NOTE]
> This guide assumes the rootless path for the first created user, which is `/run/user/1000/podman/podman.sock`.

## Container Example

The following example demonstrates how to make Forgejo Actions work with this setup.

### Docker

By default, [[Forgejo]] uses Docker in Docker:

```yml
runner:
  image: 'data.forgejo.org/forgejo/runner:12'
  links:
    - docker-in-docker
  depends_on:
    docker-in-docker:
      condition: service_started
  container_name: 'runner'
  environment:
    DOCKER_HOST: tcp://docker-in-docker:2375
  # User without root privileges, but with access to `./data`.
  user: 1001:1001
  volumes:
    - ./data:/data
  restart: 'unless-stopped'
  command: 'forgejo-runner daemon --config runner-config.yml'
```

As you can see, it's fairly complicated, requiring an entire container (docker-in-docker), as well as a different user.

### Podman

With Podman, we can get rid of most of that bloat:

```yml
runner:
  image: data.forgejo.org/forgejo/runner:12
  container_name: runner
  environment:
    - DOCKER_HOST=unix:///run/podman/podman.sock
  volumes:
    - ./data:/data
    - /run/user/1000/podman/podman.sock:/run/podman/podman.sock
  user: 1000:1000
  userns_mode: keep-id
  restart: unless-stopped
  command: forgejo-runner daemon --config /data/runner-config.yml
```

1. We get rid of the `docker-in-docker` container entirely, it's not needed since we are using the host Podman.
2. Instead of using the Docker host, we set it to the Podman socket.
3. We bind the container socket volume to the system one.
	- *Make sure to use the right path for your setup.*
4. If using rootless Podman, you must specify the `user` in case Containerfile changed it.
5. If using rootless Podman, you must set `userns` to `keep-id` to make sure your system UID/GID is preserved inside the container.

That's literally all it takes to get Podman Socket Binding to work, much simpler than what Docker has to offer.

### Podman Tips

Here are a few tweaks to make this container even better:

```yml
runner:
  image: data.forgejo.org/forgejo/runner:12
  container_name: forgejo-runner
  networks:
    - forgejo
  environment:
    - DOCKER_HOST=unix:///run/podman/podman.sock
  volumes:
    - ./runner-data:/data:U
    - /run/user/1000/podman/podman.sock:/run/podman/podman.sock:z
  user: 1000:1000
  userns_mode: keep-id
  restart: always
  command: forgejo-runner daemon --config /data/runner-config.yml
```

1. Replace generic names with container specific ones:
    - `runner` -> `forgejo-runner`
    - `data` -> `runner-data`
2. Add the `:U` flag to the data volume to make sure it's owned by the container user.
3. Add the `:z` flag to the socket volume to avoid issues on [[SELinux]] systems.
4. Set `restart` to `always`, this makes sure the container is started whenever you reboot.
5. Add the container to your Forgejo network, in my case it's `forgejo`.
   - *This allows you to easily access your Forgejo container with its host name, in my case also `forgejo`.*

Here's an example workflow that triggers a different repository's build workflow:

```yml
on: [push]

jobs:
  notify:
    runs-on: alpine
    steps:
      - name: Install deps
        run: apk add --no-cache curl

      - name: Trigger build
        run: >-
          curl -X POST "http://forgejo:3000/api/v1/repos/nnra6864/quartz-Vault/actions/workflows/build.yml/dispatches"
          -H "Authorization: token ${{ secrets.TOKEN }}"
          -H "Content-Type: application/json"
          -d '{ "ref": "v4", "inputs": {} }'
```

As you can see, it's extremely easy to communicate with your Forgejo server if you added the runner container to the same network.
We can simply reference it by its host name, `forgejo`, with the port it uses, in my case `3000`.

## Sources

[Podman System Service](https://docs.podman.io/en/latest/markdown/podman-system-service.1.html)
[Docker-in-Docker](https://www.docker.com/resources/docker-in-docker-containerized-ci-workflows-dockercon-2023/)
[Forgejo](https://forgejo.org/)
[Forgejo Actions](https://forgejo.org/docs/latest/admin/actions/)