---
created: 2026-03-24T15:42:00
tags:
  - note
  - tip
  - container
  - podman
  - hosting
aliases:
---
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

## Host Network

To access the host network, use:
```
host.containers.internal
```

## Broken Container DNS

This is often caused by [[systemd-resolved]].
It tries to play smart by pointing to `127.0.0.53` (itself), and containers don't know how to handle this when they copy the host dns.
To check if that's the case, inspect `/etc/resolv.conf`:
```sh
cat /etc/resolv.conf
```

If that's the case, simply replace it with a symlink to your actual dns:
```sh
sudo ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.conf
sudo resolvectl flush-caches
```

If you want to undo this, simply delete `/etc/resolv.conf` and it'll be regenerated:
```sh
sudo rm /etc/resolv.conf
sudo systemctl restart systemd-resolved
```

## Keep UID

In order to be able to edit config files etc. that you mounted with `:U` flag, you can use the following option:
```yml
userns: keep-id
```
It preserves your UID in the container so you have the full access to mounted files etc.

> [!WARNING]
> There is a very small chance a 0 day vulnerability could be abused on your server, and the attacker would have all the same permissions as your user, but the ease of use is worth taking this minimal risk.

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
