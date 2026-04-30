## Updating

Run:
```sh
podman compose down
podman compose pull
podman compose up -d
```

If container is a local Containerfile pulling from git or similar, rebuild without cache:
```
podman compose down
podmancompose up -d --build --no-cache
```

## Podman Socket

This service allows for behavior similar to docker in docker.
```sh
systemctl --user enable podman.socket
```
It'll give you are persistent socket at:
```
unix:///run/user/1000/podman/podman.sock
```
You can then use it to run containers within your containers, e.g. for [[Forgejo]] runners:
```yml
services:
  runner:
    image: data.forgejo.org/forgejo/runner:12
    container_name: forgejo-runner
    restart: always
    networks:
      - forgejo
    environment:
      - DOCKER_HOST=unix:///run/podman/podman.sock
    volumes:
      - ./runner-data:/data:U
      - /run/user/1000/podman/podman.sock:/run/podman/podman.sock
    user: 1000:1000
    userns_mode: keep-id
    command: forgejo-runner daemon --config /data/runner-config.yml
```
Important things here are:
- Adding `DOCKER_HOST` env var and pointing it to .sock
- Adding .sock volume
- `userns_mode: keep-id` - keeps the user id to .sock file can actually be accessed
