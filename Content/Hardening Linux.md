---
created: 2026-11-06 11:32
tags:
  - note
  - guide
  - tip
aliases:
---

## Protect .env files

Always `chmod 600` newly created .env files.
For all the existing ones, run the following on your containers dir:
```sh
chmod 600 ~/Containers/*/.env ~/Containers/*/*.env
```

## Harden SSH

- Disable root login by editing:
```sh
sudoedit /etc/ssh/sshd_config
```
And setting `PermitRootLogin no`.

- Only allow [[LAN]] connections:
```sh
sudo ufw allow from 192.168.0.0/24 to any port 22 proto tcp
sudo ufw allow from 192.168.0.0/24 to any port 8384 proto tcp
sudo ufw delete allow 22
sudo ufw delete allow 8384
sudo ufw delete allow 8384/tcp
```

> [!NOTE]
> [[NetBird]] should still keep working after this change.
