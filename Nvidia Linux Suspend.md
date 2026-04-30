---
created: 2026-02-23T09:49:00
tags:
  - note
  - tip
  - linux
  - nvidia
aliases:
---
1. Activate needed services
	```sh
	sudo systemctl enable nvidia-suspend.service
	sudo systemctl enable nvidia-hibernate.service
	sudo systemctl enable nvidia-resume.service
	```
	> [!note]
	> Some distros, such as [Arch] and [NixOS], already do this for you

2. Open your grub config
	```sh
	sudoedit /etc/default/grub
	```
3. Add the following to `GRUB_CMDLINE_LINUX_DEFAULT`
	```ini
	nvidia.NVreg_PreserveVideoMemoryAllocations=1
	```
4. Rebuild your grub config
	```sh
	sudo grub-mkconfig -o /boot/grub/grub.cfg
	```
# Sources

[Source](https://wiki.hypr.land/Nvidia/#suspendwakeup-issues)
[Credit](https://discord.com/channels/961691461554950145/967398886530515067/1475409907283923068)