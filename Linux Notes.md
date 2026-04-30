---
created: 2025-12-12 06:09:03
tags:
  - note
  - linux
  - tip
  - config
---

## Allow apps to use as much memory as needed

`sudo nvim /etc/security/limits.conf`
```
*   -   memlock   unlimited
*   -   rtprio    99
*   -   nice      -10
```

## [Gamemode](https://github.com/FeralInteractive/gamemode)

Use to launch games:
```sh
gamemode run %command%
```

## Run services before login

```sh
sudo loginctl enable-linger $USER
```

## Toggle screen(works via ssh too)

- Off
	```sh
	TERM=linux sudo setterm --blank force --powersave powerdown < /dev/tty1
	```
- On
	```sh
	TERM=linux sudo setterm --blank poke < /dev/tty1
	```
- Systemd service:
	```sh
	sudoedit /etc/systemd/system/disable-screen.service
	```

	```toml
	[Unit]
	Description=Disable screen backlight at boot
	After=getty.target

	[Service]
	Type=oneshot
	ExecStartPre=/usr/bin/sleep 2
	ExecStart=/bin/sh -c 'TERM=linux setterm --blank force --powersave powerdown < /dev/tty1'
	RemainAfterExit=yes

	[Install]
	WantedBy=multi-user.target
	```

	```sh
	sudo systemctl enable --now disable-screen.service
	```

## Don't suspend on laptop lid close

1. Edit logind config
	```sh
	sudoedit /etc/systemd/logind.conf
	```
2. Set values
	```ini
	HandleLidSwitch=ignore
	HandleLidSwitchExternalPower=ignore
	HandleLidSwitchDocked=ignore
	```
3. Restart logind
	```sh
	sudo systemctl restart systemd-logind
	```

## Apps crashing due to audio server dying

Run
```sh
sysctl -w kernel.pid_max=65535
```

Create `/etc/sysctl.d/99-sysctl.conf` for permanent change with the following content:
```
kernel.pid_max=65535
```

## Fish Tips

- Edit the current buffer by pressing `alt+e`
- View command history by pressing `ctrl+r`

> [!NOTE]
> All the binds can be found in [fish docs](https://fishshell.com/docs/current/interactive.html#shared-bindings).