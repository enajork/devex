# Dotfiles guide
- [Intro](https://github.com/enajork/dotfiles-guide/blob/main/README.md#intro)
- [Why Arch, Xorg, i3, & RDP](https://github.com/enajork/dotfiles-guide/blob/main/README.md#why-arch-xorg-i3--rdp)
- [Installing Arch, Xrdp, & i3](https://github.com/enajork/dotfiles-guide/blob/main/README.md#installing-arch-xrdp--i3)
  - [Post-installation of OS](https://github.com/enajork/dotfiles-guide/blob/main/README.md#installing-arch-xrdp--i3)
    - [Xrdp](https://github.com/enajork/dotfiles-guide/blob/main/README.md#xrdp)
    - [i3](https://github.com/enajork/dotfiles-guide/blob/main/README.md#i3)
    - [Essential packages](https://github.com/enajork/dotfiles-guide/blob/main/README.md#essential-packages)
    - [Pipewire audio over Xrdp](https://github.com/enajork/dotfiles-guide/blob/main/README.md#pipewire-audio-over-xrdp)
    - [Enable Avahi (only needed for Sunshine)](https://github.com/enajork/dotfiles-guide/blob/main/README.md#enable-avahi-only-needed-for-sunshine)
    - [Install AMD graphics drivers (only for AMD GPUs)](https://github.com/enajork/dotfiles-guide/blob/main/README.md#install-amd-graphics-drivers-only-for-amd-gpus)
    - [Install xstart](https://github.com/enajork/dotfiles-guide/blob/main/README.md#install-xstart)
    - [Neovim setup](https://github.com/enajork/dotfiles-guide/blob/main/README.md#neovim-setup)
  - [SSH without entering password](https://github.com/enajork/dotfiles-guide/blob/main/README.md#ssh-without-entering-password)
  - [startx on boot (optional)](https://github.com/enajork/dotfiles-guide/blob/main/README.md#startx-on-boot-optional)
- [Handy links](https://github.com/enajork/dotfiles-guide/blob/main/README.md#handy-links)
- [TODO](https://github.com/enajork/dotfiles-guide/blob/main/README.md#todo)
- [Future improvements](https://github.com/enajork/dotfiles-guide/blob/main/README.md#future-improvements)
- [Known issues](https://github.com/enajork/dotfiles-guide/blob/main/README.md#known-issues)
- [macOS](https://github.com/enajork/dotfiles-guide/blob/main/README.md#macos)
  - [AeroSpace](https://github.com/enajork/dotfiles-guide/blob/main/README.md#macos)
  - [kitty](https://github.com/enajork/dotfiles-guide/blob/main/README.md#kitty)
  - [stow](https://github.com/enajork/dotfiles-guide/blob/main/README.md#stow)
- [Tailscale & RDP](https://github.com/enajork/dotfiles-guide/blob/main/README.md#tailscale--rdp)

## Intro
This guide outlines how to manually install my dotfiles and all the packages required on Arch Linux.

You can automatically install my dotfiles and required packages using the following script on Arch Linux:
```sh
curl -sL https://raw.githubusercontent.com/enajork/install_dotfiles/main/run.sh | bash
```
## Why Arch, Xorg, i3, & RDP
A few sections (written by ChatGPT) about why each of these were chosen:
  1. [Why Arch](https://github.com/enajork/dotfiles-guide/blob/main/ARCH.md)
  2. [Why Xorg](https://github.com/enajork/dotfiles-guide/blob/main/XORG.md)
  3. [Why a tiling window manager](https://github.com/enajork/dotfiles-guide/blob/main/TILING.md)
  4. [Why RDP](https://github.com/enajork/dotfiles-guide/blob/main/REMOTE.md)

## Installing Arch, Xrdp, & i3
Xrdp is not required when running on bare-metal.

Details on how to install Arch can be found [here](https://wiki.archlinux.org/title/Installation_guide).
- Create Arch installation media USB.
- Use UEFI to boot from USB.
- Run `archinstall` command to begin installation.
- Before running that command, you might have to connect to the network using `iwctl`. Once in interactive mode run:
  - `device list`
  - `station wlan0 scan`
  - `station wlan0 get-networks`
  - `station wlan0 connect SSID`
- Select preferences in `archinstall` script accordingly and begin installation.
- Most recently, I chose to use ext4 for the file system and to skip installation of `NetworkManager` (used ISO settings instead). It is possible to install `NetworkManager` after Arch.
### Post-installation of OS
#### Automated install
- Run [script](https://github.com/enajork/install_dotfiles) to automatically install dotfiles and dependencies.
```sh
curl -sL https://raw.githubusercontent.com/enajork/install_dotfiles/main/run.sh | bash
```

This automates the rest of the steps below.
#### SSH and Yay
- To get SSH working, run `sudo pacman -S openssh`, `sudo systemctl enable sshd` and `sudo systemctl start sshd`.
- Next, it is time to install Git. Run `sudo pacman -S --needed git base-devel`.
- Git is a prerequisite for installing yay. To install yay, run the following:
  - `git clone https://aur.archlinux.org/yay.git && cd yay && makepkg -si` 
#### Xrdp
- Next install Xrdp using the following instructions:
  - https://wiki.archlinux.org/title/Xrdp
    - Install `xrdp` and `xorgxrdp` using `yay -S xrdp xorgxrdp`.
    - Create `~/.xinitrc` and add the one-liner `exec i3`.
    - Run `sudo systemctl enable xrdp` and `sudo systemctl start xrdp`.
  - Remote desktop should now start your desired window manager when connected. Next we will need to configure i3 for Xrdp.
#### i3
- Run `sudo pacman -S i3` to install i3.
- Change `~/.xinitrc` to `exec i3`. Restart xrdp using `sudo systemctl restart xrdp` to pick up changes.
That's it. Arch, Xrdp, and i3 have been installed. This is the bare minimum install and starting point for everything to come. The next step is customization.
#### Essential packages
- Install some dependencies - `yay -Sy fastfetch fzf kitty maim picom polybar rofi tmux xorg-xrdb xclip zsh`.
- Install tmux package manager (tpm) - `git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm`
- Install Oh My Zsh with `sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`.
- Run `yay -S --noconfirm zsh-theme-powerlevel10k-git && echo 'source /usr/share/zsh-theme-powerlevel10k/powerlevel10k.zsh-theme' >>~/.zshrc` to install powerlevel10k.
- Install more zsh plugins with the following commands:
  - `git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions`
  - `git clone https://github.com/zsh-users/zsh-completions ${ZSH_CUSTOM:-${ZSH:-~/.oh-my-zsh}/custom}/plugins/zsh-completions`
  - `git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting`
- Configure dotfile using stow (`yay -Sy stow`). First, clone [dotfiles](https://github.com/enajork/dotfiles) in your home directory from <link>. Then, run `stow <config_name>` (i.e `stow i3`) for each directory in the repo.
- Run `p10k configure` to setup powerlevel10k or `stow p10k` from [dotfiles](https://github.com/enajork/dotfiles) directory.
- Install dark mode https://wiki.archlinux.org/title/Dark_mode_switching
  - `yay -S qt6-base qt5-base gtk4 gtk3 gnome-themes-extra adwaita-qt5-git adwaita-qt6-git`
  - Add dark mode exports to Zsh as described in the Arch Wiki page (or use `stow zsh`).

#### Pipewire audio over Xrdp
- `yay -S pipewire-module-xrdp`
- Add `exec --no-startup-id /usr/lib/pipewire-module-xrdp/load_pw_modules.sh` to i3 config

#### Enable Avahi (only needed for Sunshine)
- `systemctl enable avahi-daemon`

#### Install AMD graphics drivers (only for AMD GPUs)
- `sudo pacman -S mesa vulkan-radeon libva-mesa-driver libva-utils`

#### Install xstart
- `sudo pacman -S xorg xorg-server xorg-xinit`

#### Neovim setup
- Start by installing [LazyVim](https://www.lazyvim.org/installation)

### SSH without entering password
- Run `ssh-keygen`.
- Run `ssh-copy-id remote_username@remote_server_ip_address`.

### startx on boot (optional)
Instead of running startx from the tty, or from a display manager, run it on boot directly & automatically instead.
- Edit the getty service for tty1 using `sudo systemctl edit getty@tty1`
- Add the following to your configuration:
```
[Service]
ExecStart=
ExecStart=-/usr/bin/agetty --autologin your_username --noclear %I $TERM
```
Replace `your_username` with your actual username.

Add the following to `.zshrc`.
```
if [[ -z $DISPLAY ]] && [[ $(tty) == /dev/tty1 ]]; then
    exec startx
fi
```
## Tailscale & RDP
- Install [Tailscale](https://tailscale.com/download/) on Linux remote machine and on Mac client. Tailscale allows users to create their own VPN ("Tailnets") networks in seconds.
- Install [Windows App](https://apps.apple.com/us/app/windows-app/id1295203466?mt=12) on Mac. This is a RDP client for Mac which can be used with Tailscale to connect to an installation of Arch Linux running these "dots".

## macOS
### AeroSpace
AeroSpace is a tiling window manager for MacOS. I have a basic config for it in my [dotfiles](https://github.com/enajork/dotfiles).

### kitty
Either:
```sh
curl -L https://sw.kovidgoyal.net/kitty/installer.sh | sh /dev/stdin
```
Or:
```sh
brew install --cask kitty
```

### stow
Install GNU stow on macOS using `brew install stow`.
 
## Known issues
- picom flickers when changing workspaces - https://github.com/yshui/picom/issues/16 - still having this issue but it is not severe enough to ditch picom.
- ~~Some picom issue that causes windows to disappear when they are tiled with transparency enabled~~
  - Fixed by using `use-damage = false;` setting.
- ~~picom draws ugly black boxes around context & dropdown menus in chromium (similar to this [issue](https://github.com/orgs/regolith-linux/discussions/949)).~~
  - Fixed by installing GTK.
- Mouse warping does not work while using RDP. This is an inherent limitation.

## Future improvements
- More aliases
  - Create separate file for aliases
  - https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/archlinux

## TODO
- Window focus needs to be more obvious. Using xwindow in polybar for the time being. Waiting for PRs to i3 to get merged to fix this. Would rather not have to use something like [xborder](https://github.com/deter0/xborder).
  - https://github.com/i3/i3/issues/4292
  - https://github.com/i3/i3/pull/5384
  - https://github.com/i3/i3/pull/5944 (most recently active - 1 week ago)
  - Now looking into https://github.com/andreykaere/ixwindow too

## Handy links
- https://i3wm.org/docs/debugging.html
