---
created: 2025-12-31 15:23:35
tags:
  - note
  - linux
  - config
---
# WiFi

If you don't use a wired connection, you must connect to a wireless one.
You should follow the IW guide if you are in the [[Arch]] iso.

1. [[Connecting to WiFi with IW]]
2. [[Connecting to WiFi with Network Manager]]

# Setup

## Installation

1. Use `archinstall` or a manual installation
2. Install packages needed for this guide
	```sh
	sudo pacman -S git reflector bat nvim tree-sitter-cli
	```
	> [!note]
	> `sudoedit` won't use `nvim` as the editor unless you explicitly set `$EDITOR` and `$VISUAL` env vars to `nvim`
	> This is already done for you in the fish config of [[#Setup#Configuration#Ricerland]]

3. Install [[paru]] or [[yay]]
	```sh
	sudo pacman -S --needed base-devel &&
	git clone https://aur.archlinux.org/paru.git &&
	cd paru &&
	makepkg -si
	```
## Configuration

### Ricerland

1. Install needed packages
	```sh
	sudo pacman -S eza zoxide fastfetch oh-my-posh-bin
	```
2. Clone the [[Ricerland]] repo
	```sh
	git clone --recurse https://github.com/nnra6864/Ricerland ~/.config/
	```
3. Replace [[bash]] with [[fish]]
	```sh
	sudo pacman -S fish && echo '[ -x /bin/fish ] && SHELL=/bin/fish exec fish' >> ~/.bashrc && source ~/.bashrc
	```

### pacman

1. Edit the [[pacman]] config
	```sh
	sudoedit /etc/pacman.conf
	```
2. Uncomment `Color` to enable colorful output
3. Add `ILoveCandy` to use pacman as a progress bar
4. Uncomment `VerbosePkgLists` to get a detailed table view of packages
5. Set `ParallelDownloads` to 3-10, 3 being a slow laptop and 10 being a good mid-high end pc
6. Uncomment `multilib`

### paru

1. Edit the [[paru]] config
	```sh
	sudoedit /etc/paru.conf
	```
2. Uncomment `CombinedUpgrade` to get a single review for all the packages
	> [!note]
	> Make sure you have `bat` installed to get nice syntax highlighting

3. Uncomment `UpgradeMenu` to get an interactive menu for upgrading packages

### Package Repositories

> [!note]
> Repos in the pacman config should follow the same order as in this guide

#### CachyOS

[[CachyOS]] repos provide highly optimized, prebuilt packages.
Follow [[Cachy on Arch#Cachy Repositories]].

#### LizardByte

[LizardByte repo](https://github.com/LizardByte/pacman-repo) provides official builds for [[Sunshine]] [(Source)](https://docs.lizardbyte.dev/projects/sunshine/).
Add the following to `/etc/pacman.conf`
```ini
[lizardbyte]
SigLevel = Optional
Server = https://github.com/LizardByte/pacman-repo/releases/latest/download
```

#### Chaotic-AUR

[[Chaotic-AUR]] contains a lot of precompiled packages.
It's great if no other repo has the bin version of the package you need.

1. Retrieve the primary key to enable the installation of chaotic keyring and mirror list
	```sh
	sudo pacman-key --recv-key 3056513887B78AEB --keyserver keyserver.ubuntu.com
	sudo pacman-key --lsign-key 3056513887B78AEB
	```
2. Install the chaotic keyring and mirrorlist
	```sh
	sudo pacman -U 'https://cdn-mirror.chaotic.cx/chaotic-aur/chaotic-keyring.pkg.tar.zst'
	sudo pacman -U 'https://cdn-mirror.chaotic.cx/chaotic-aur/chaotic-mirrorlist.pkg.tar.zst'
	```
3. Add the following to `/etc/pacman.conf`
	```ini
	[chaotic-aur]
	Include = /etc/pacman.d/chaotic-mirrorlist
	```

### Mirrors

1. To get the best mirrors, run
	```sh
	sudo reflector --country 'Serbia,Hungary,Germany,Austria' --age 12 --latest 20 --protocol https --sort rate --save /etc/pacman.d/mirrorlist
	```
	> [!note]
	> You should adjust `--country` to whatever is close to you

2. Update repos
	```sh
	paru -Sy
	```

## Packages

1. Optionally look into [[Cachy on Arch]]
2. Edit `~/.config/Packages`
3. Install `iptables-nft` to replace `iptables` and avoid conflicts later on
4. Install all the packages
	```sh
	paru -Syu --needed --sudoloop (cat ~/.config/Packages)
	```
5. Downgrade `kiconthemes` to avoid weird qt app theming
	1. downgrade to `6.5`
		```sh
		sudo downgrade kiconthemes
		```
	2. Add `kiconthemes` to `/etc/pacman.conf` `IgnorePkg`
6. Add/update hyprpm packages
	```sh
	hyprpm update
	```
7. Rice
	11. Install icons(or run ricer)
	12. Install fonts
	13. Install themes(or run ricer)
	14. Install cursors
8. Setup services
	1. Tailscale
	2. Syncthing
	3. KDE Connect
9. Edit hosts if needed
	```sh
	sudoedit /etc/hosts
	```
	```
	192.168.0.10 nnstdios.xyz
	192.168.0.10 git.nnstdios.xyz
	192.168.0.10 nnra.nnstdios.xyz
	192.168.0.10 chat.nnstdios.xyz
	```
10. Assign a static ip to the mac address in router settings(e.g. 192.168.0.20)
11. Fix dolphin missing apps in the Open With menu ([Source](https://discuss.kde.org/t/dolphin-doesnt-show-a-single-app-in-the-open-with-menu/14799/11))
    ```sh
    sudo ln -s /etc/xdg/menus/plasma-applications.menu /etc/xdg/menus/applications.menu
    ```

## Nvidia

In case you have an [[Nvidia]] [[GPU]], you might be able to squeeze out some extra performance.
Check out [[Increase Default Power Limit for Nvidia GPUs]].