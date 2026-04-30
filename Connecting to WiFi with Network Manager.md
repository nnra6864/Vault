---
created: 2025-12-31 13:57:47
tags:
  - note
  - linux
  - tip
---
To list networks:
```
nmcli device wifi list
```

To connect:
```
nmcli device wifi connect "Network Name" password "Password"
```
In case it says something similar to `nmcli security.key-mgmt: property is missing`, try appending `--wpa-psk` to the command.

To check for status:
```
nmcli device status
```