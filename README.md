# Creating a developer workflow for enhanced productivity
## What is this?
This is a collection of living documents that I started writing initially to serve as a recipe book of sorts. It was just a place for me to record the exact steps I used to create my ideal desktop enviroment in addition to creating my dotfiles. Later, I will convert these steps into an installation script to automate the process of setting up my dotfiles and other aspects of the desktop environment on new machines (virtual or physical).
## Why some certain tools were used instead of others
Rather than reinventing the wheel, I used off the shelf & open source solutions to create the desktop environment that I desired. In the same spirit of not reinventing something, I also chose not to spend the time creating a breakdown of pros and cons and/or write a verbose explanation of why specific components were chosen. I outsourced this to ChatGPT instead.

1. [Why Arch](https://github.com/enajork/devex/blob/main/ARCH.md)
2. [Why Xorg](https://github.com/enajork/devex/blob/main/XORG.md)
3. [Why a tiling window manager](https://github.com/enajork/devex/blob/main/TILING.md)
4. [Why RDP](https://github.com/enajork/devex/blob/main/REMOTE.md)

ChatGPT actually did a stellar job of summarizing why I chose each of the above components.

## What this guide does not cover
This does not cover how to use your custom desktop enviroment to its full extent. In order to do that, there really is no better way than getting your hands dirty. I might write more about very specific tools or use cases in the future, but it will not be written about in this document.

## Installing Arch, Xrdp, & i3
Note: Xrdp is not required if you intend on running Arch directly as your primary OS (or using dual boot) and do not need remote access to your Arch install. Xrdp can be useful if running in a VM.

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
### Post-installation
- Once installation is complete, you may reboot. Alternatively, `archinstall` allows for post-installation steps to be completed in chroot as a final stage.
- To get SSH working, run `sudo pacman -S openssh`, `sudo systemctl enable sshd` and `sudo systemctl start sshd`.
- Next, it is time to install Git. Run `sudo pacman -S --needed git base-devel`.
- Git is a prerequisite for installing yay. To install yay, run the following:
  - `git clone https://aur.archlinux.org/yay.git && cd yay && makepkg -si`
### Xrdp
- Next install Xrdp using the following instructions:
  - https://wiki.archlinux.org/title/Xrdp
    - Install `xrdp` and `xorgxrdp` using `yay -S xrdp xorgxrdp`.
    - Create `~/.xinitrc` and add the one-liner `exec i3`.
    - Run `sudo systemctl enable xrdp` and `sudo systemctl start xrdp`.
  - Remote desktop should now start your desired window manager when connected. Next we will need to configure i3 for Xrdp.
### i3
- Run `sudo pacman -S i3` to install i3.
- Change `~/.xinitrc` to `exec i3`. Restart xrdp using `sudo systemctl restart xrdp` to pick up changes.
That's it. Arch, Xrdp, and i3 have been installed. This is the bare minimum install and starting point for everything to come. The next step is customization.
## Customization
- Install some dependencies - `yay -Sy fastfetch fzf kitty picom polybar rofi tmux xorg-xrdb xclip zsh`.
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
 
## SSH without entering password
- Run `ssh-keygen`.
- Run `ssh-copy-id remote_username@remote_server_ip_address`.

## Pipewire audio over Xrdp
- `yay -S pipewire-module-xrdp`
- Add `exec --no-startup-id /usr/lib/pipewire-module-xrdp/load_pw_modules.sh` to i3 config

## Neovim setup
- Start by installing [LazyVim](https://www.lazyvim.org/installation)

## Handy links
- https://i3wm.org/docs/debugging.html

## TODO
- Window focus needs to be more obvious. Using xwindow in polybar for the time being. Waiting for PRs to i3 to get merged to fix this. Would rather not have to use something like [xborder](https://github.com/deter0/xborder).
  - https://github.com/i3/i3/issues/4292
  - https://github.com/i3/i3/pull/5384
  - https://github.com/i3/i3/pull/5944 (most recently active - 1 week ago)
- Make installation script to automate setup

## Future improvements
- Use Ansible for installation script
- More aliases
  - Create separate file for aliases
  - https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/archlinux
- Switch to btrfs instead of ext4

## Known issues
- picom flickers when changing workspaces - https://github.com/yshui/picom/issues/16 - still having this issue but it is not severe enough to ditch picom.
- ~~Some picom issue that causes windows to disappear when they are tiled with transparency enabled~~ Fixed by using `use-damage = false;` setting.
- ~~picom draws ugly black boxes around context & dropdown menus in chromium (similar to this [issue](https://github.com/orgs/regolith-linux/discussions/949)).~~ Fixed by installing GTK.
- Mouse warping does not work while using RDP. This is an inherent limitation. I might consider moving away from RDP and simply running Arch locally in the future.

## macOS
### AeroSpace
I still use a Mac laptop whenever I am on the go. While macOS does not offer the same level of customization, it still possible to use a tiling window manager and some of the same [dotfiles](https://github.com/enajork/dotfiles). kitty, nvim, p10, zsh and tmux are all transferable. AeroSpace is a tiling window manager for MacOS. It does not work as well as i3, but it serves its purpose.

On a slightly related note: I toyed with dual booting Fedora Asahi Remix (an M1 compatible distro), but quickly moved away from it. Ultimately, poor battery performance was my reason for switching back to macOS. My personal laptop is the only Mac I use with any semblance of regularity, so it is nice to have a machine to check out the latest features on Mac as well.

### kitty
I've been using iTerm2 for the past few years, but finally switched to using kitty on macOS. To install, either run with `curl` or `brew`:
- `curl -L https://sw.kovidgoyal.net/kitty/installer.sh | sh /dev/stdin`
- `brew install --cask kitty`

### stow
Install GNU stow on macOS using `brew install stow`.

### Tailscale & RDP
- Install [Tailscale](https://tailscale.com/download/) on Linux remote machine and on Mac client.
- Install [Windows App](https://apps.apple.com/us/app/windows-app/id1295203466?mt=12) on Mac.
- Now you can use Windows App to remote desktop into your Arch Linux machine. From very limited testing, it seems like power consumption of remote desktop is better than a local workflow with AeroSpace, kitty, and Chrome. Offloading more processing to the server can save battery life depending on the task. This also reinforces the decision to go with a Xorg + RDP setup because it requires less bandwidth/power than VNC. RDP and Xorg are generally more mature and robust than Wayland and VNC. Even if my primary use case was to use Linux locally instead of remotely, I would still probably not choose Wayland.
