---
created: 2026-01-02 17:39:58
tags:
  - note
  - journal
  - dev/hosting
  - config
---
Making [[Syncthing]] available no matter where you are is surprisingly easy with [[Netbird]], [[Tailscale]](or any other [[VPN]]).
This guide assumes you have Syncthing and Netbird already up and running(shouldn't take more than 15 minutes).

## Setup

1. Navigate to the [Syncthing Web UI](https://127.0.0.1:8384/).
2. Either edit an existing device, or add a new one.
3. Go to the Advanced tab.
4. In the address field, replace `dynamic` with `tcp://device-vpn-ip:22000`.
5. Repeat on and for all devices.

## Access the Web UI over VPN/LAN

By default, the Web UI is limited to localhost.
Simply run the following command to make it available for devices on [[LAN]]/[[VPN]]:
```sh
sed -i 's/127.0.0.1:8384/0.0.0.0:8384/' ~/.local/state/syncthing/config.xml
```