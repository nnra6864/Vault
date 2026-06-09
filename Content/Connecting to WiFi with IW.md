---
created: 2025-12-31 12:10:25
tags:
  - note
  - linux
  - tip
---
1. Unblock the wifi device(not always necessary but can prevent issues with no network devices found):
```
rfkill unblock wifi
```
2. Start the `iwd` service
```
systemctl start iwd
```
3. Run iwctl
```
iwctl
```
4. List stations
```
station list
```
5. Scan for networks, replace `wlan0` with the device name listed in step 3
```
station wlan0 scan
```
6. List networks
```
station wlan0 get-networks  
```
7. Connect to a network
```
station wlan0 connect 'NetworkName'
```
8. Exit iwctl
```
exit
```