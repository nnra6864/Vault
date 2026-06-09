---
created: 2026-01-26 11:19:50
tags:
  - note
  - linux
  - guide
  - tip
  - audio
---
Make sure you have your audio server correctly setup on both server and client:
```sh
pactl info | grep "Server Name"
```
Should output:
```
Server Name: PulseAudio (on PipeWire)
```
If not, install the needed packages:
```sh
sudo pacman -S pipewire pipewire-pulse wireplumber
```

## Server

1. Edit the pipewire network config using either method
	1. Allow all connections
		```sh
		mkdir -p ~/.config/pipewire/pipewire-pulse.conf.d && echo 'context.exec = [
		    {
		        path = "pactl"
		        args = "load-module module-native-protocol-tcp auth-anonymous=1"
		    }
		]' | tee ~/.config/pipewire/pipewire-pulse.conf.d/network.conf > /dev/null
		```
	2. Allow only specific IPs
		```sh
		mkdir -p ~/.config/pipewire/pipewire-pulse.conf.d && echo 'context.exec = [
		    {
		        path = "pactl"
		        args = "load-module module-native-protocol-tcp auth-ip-acl=127.0.0.1;192.168.0.20"
		    }
		]' | tee ~/.config/pipewire/pipewire-pulse.conf.d/network.conf > /dev/null
		```
		> [!NOTE]
		> You can add as many IPs as you want, just separate them with `;`.

2. Restart the pipewire service
	```sh
	systemctl --user restart pipewire-pulse
	```

## Client

1. Manually connect to the server to test if the setup works
	```sh
	pactl load-module module-tunnel-sink server=server-ip
	```
	> [!NOTE]
	> Make sure to replace `server-ip` with your actual server ip

2. If that worked, you can create a `share-audio.sh` script in one of your binary paths
	```sh
	#!/bin/bash

	if [ -z "$1" ]; then
	    echo "Usage: $0 <target-ip>"
	    echo "Example: $0 192.168.0.20"
	    exit 1
	fi
	
	TARGET_IP="$1"
	
	MODULE_ID=$(pactl list modules short | grep "module-tunnel-sink.*server=${TARGET_IP}" | awk '{print $1}')
	
	if [ -n "$MODULE_ID" ]; then
	    echo "Tunnel sink exists, removing it..."
	    pactl unload-module "$MODULE_ID"
	    echo "Disconnected from audio server"
	else
	    echo "Creating tunnel sink..."
	    pactl load-module module-tunnel-sink server="$TARGET_IP"
		sleep 0.5
		SINK_NAME=$(pactl list sinks short | grep -i tunnel | awk '{print $2}' | head -n1)
	    if [ -n "$SINK_NAME" ]; then
	        pactl set-default-sink "$SINK_NAME"
	        echo "Connected to audio server at $TARGET_IP (sink: $SINK_NAME)"
	    else
	        echo "Connected to audio server at $TARGET_IP (couldn't auto-set as default)"
	    fi
	fi   
	```
	> 
	> [!NOTE]
	> Binary path is just a path in which your system looks for binaries.
	> 
	> For example, you can easily add such a path to the fish shell by adding the following to your fish config:
	> ```fish
	> fish_add_path ~/.config/fish/scripts
	> ```

3. Make the script executable
	```sh
	chmod +x share-audio.sh
	```
4. Reload your shell and test the script
	```sh
	share-audio.sh server-ip
	```
	> NOTE
	> Make sure to replace `server-ip` with your actual server ip.

5. If everything works, it should look similar to this
	```
	share-audio.sh 192.168.0.20
	Creating tunnel sink...
	536870916
	Connected to audio server at 192.168.0.20 (sink: tunnel-sink.192.168.0.20)
	```