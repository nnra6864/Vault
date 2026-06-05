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
It tries to copy your system [[DNS]] config, which usually points to a local [[IP Address]] the container can't reach.

## Fix

The fix is rather simple - just configure podman properly.

Edit your podman config:
```sh
nvim ~/.config/containers/containers.conf
```

Add the following to it:
```toml
[containers]
dns_servers = ["1.1.1.1", "8.8.8.8"]
```

This hardcodes the default DNS servers avoiding any potential issues.

> [!NOTE]
> You need to restart affected containers for this change to take effect.