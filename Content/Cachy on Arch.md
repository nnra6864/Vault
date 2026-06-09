---
created: 2026-02-20T19:27:00
tags:
  - note
  - tip
  - linux
  - guide
aliases:
---
## Cachy Repositories

Install cachy repos ([Source](https://wiki.cachyos.org/features/optimized_repos/#adding-our-repositories-to-an-existing-arch-linux-install))
```sh
curl https://mirror.cachyos.org/cachyos-repo.tar.xz -o cachyos-repo.tar.xz
tar xvf cachyos-repo.tar.xz && cd cachyos-repo
sudo ./cachyos-repo.sh
rm -rf cachyos-repo.tar.xz
rm -rf cachyos-repo
```

## Kernel

1. Install the kernel of your choice
	1. Performance
		```sh
		sudo pacman -Sy linux-cachyos-bore linux-cachyos-bore-headers
		```
	2. Power Saving(use `scx_bpfland` scheduler)
		```sh
		sudo pacman -Sy linux-cachyos linux-cachyos-headers
		```
	3. Server
		```sh
		sudo pacman -Sy linux-cachyos-server linux-cachyos-server-headers
		```
2. Make the grub config
	```sh
	sudo grub-mkconfig -o /boot/grub/grub.cfg
	```
	
[Source](https://wiki.cachyos.org/features/kernel/)
## Secure Boot
1. Install `sbctl` and `cachyos-settings`
	```sh
	sudo pacman -Sy sbctl cachyos-settings
	```
2. Enable secure boot in grub
	```sh
	sudo grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=cachyos --modules="tpm" --disable-shim-lock
	```
3. UEFI -> Secure Boot -> Reset to Setup Mode
	```sh
	systemctl reboot --firmware-setup
	```
4. Check if `Setup Mode` is enabled
	```sh
	sudo sbctl status
	```
5. Create custom secure boot keys
	```sh
	sudo sbctl create-keys
	```
6. Enroll custom keys with microsoft
	```sh
	sudo sbctl enroll-keys --microsoft
	```
7. Check if `sbctl` is installed
	```sh
	sudo sbctl status
	```
8. Sign the kernel image and boot manager and verify it worked
	```sh
	sudo sbctl-batch-sign &&
	sudo sbctl verify
	```
9. Reboot and enable secure boot

([Source](https://wiki.cachyos.org/configuration/secure_boot_setup/))

## Useful packages

### Gaming tools, libraries and launchers 

```sh
sudo pacman -Sy cachyos-gaming-meta cachyos-gaming-applications
```

([Source](https://wiki.cachyos.org/configuration/gaming/#essential-packages))
### [[Proton]] 

#### Standard
```sh
sudo pacman -Sy proton-cachyos
```
> [note]
> Better in most cases as it has much faster startup and performance

#### Steam Linux Runtime
```sh
sudo pacman -Sy proton-cachyos-slr
```
> [note]
> Good for compatibility on weird systems that might be missing libraries etc.

#### GE
```sh
paru -Sy proton-ge-custom-bin
```
> [note]
> Used for compatibility with older games

([Source](https://wiki.cachyos.org/configuration/gaming/#proton-cachyos))