---
created: 2026-06-05 15:10
tags:
  - note
  - guide
  - tip
  - podman
  - container
aliases:
---
## What causes it

This is usually caused by rootless [[Podman]] [[Networking]] being a mess.
It sometimes starts before your network, or tries to copy your system [[DNS]] config which usually points to a local [[IP Address]] the container can't reach.

## Fix
1. Ensure containers are not started before the network.
	1. Edit the restart service:
	   ```sh
	   systemctl --user edit podman-restart.service
	   ```
	2. Add the following to it:
		```toml
		[Unit]
		Wants=podman-user-wait-network-online.service
		After=podman-user-wait-network-online.service
		```
	3. Make sure you are using the user restart service and not the system one:
		```sh
		systemctl disable podman-restart.service
		systemctl --user enable --now podman-restart.service
		```
2. Hardcode the DNS.
	1. Edit your podman config:
		```sh
		nvim ~/.config/containers/containers.conf
		```
	2. Add the following to it:
		```toml
		[containers]
		dns_servers = ["1.1.1.1", "8.8.8.8"]
		```
	This hardcodes the default DNS servers avoiding any potential issues.
3. Reboot.