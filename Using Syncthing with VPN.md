---
created: 2026-01-02 17:39:58
tags:
  - note
  - journal
---
Making [[Syncthing]] available no matter where you are is surprisingly easy with [[Tailscale]](or any other [[VPN]]).
This guide assumes you have Syncthing and Tailscale already up and running(shouldn't take more than 15 minutes).
1. Navigate to the [Syncthing Web UI](https://127.0.0.1:8384/)
2. Either edit an existing device, or add a new one
3. Go to the Advanced tab
4. In the address field, replace `dynamic` with `tcp://device-vpn-ip:22000`
5. Repeat on and for all devices
6. Enjoy