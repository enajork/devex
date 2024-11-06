# Creating a developer workflow for enhanced productivity
## Installing Arch, Xrdp, & i3
Note: Xrdp is not required if you intend on running Arch directly as your primary OS (or using dual boot) and do not need remote access to your Arch install. Xrdp can be useful if running in a VM.
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
- Once installation is complete, you may reboot. Alternatively, `archinstall` allows for post-installation steps to be completed in chroot as the final stage of installation.
- To get SSH working, run `sudo pacman -S openssh`, `sudo systemctl enable sshd` and `sudo systemctl start sshd`.
- Next, it is time to install Git. Run `sudo pacman -S --needed git base-devel`.
- Git is a prerequisite for installing yay. To install yay that run the following:
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
That's it. Arch, Xrdp, and i3 have been installed. This is the bare minimum install and starting point for everything to come. The customization of the system can now begin.
## Customization
- Install terminal, status bar, and window switcher/application launcher `yay -Sy fastfetch fzf kitty polybar rofi tmux zsh`.
- Install tmux package manager (tpm) - `git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm`
- Install Oh My Zsh with `sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`.
- Run `yay -S --noconfirm zsh-theme-powerlevel10k-git && echo 'source /usr/share/zsh-theme-powerlevel10k/powerlevel10k.zsh-theme' >>~/.zshrc` to install powerlevel10k.
- Install more zsh plugins with the following commands:
  - `git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions`
  - `git clone https://github.com/zsh-users/zsh-completions ${ZSH_CUSTOM:-${ZSH:-~/.oh-my-zsh}/custom}/plugins/zsh-completions`
  - `git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting`
- Configure dotfile using stow (`yay -Sy stow`). First, clone dotfiles in your home directory from <link>. Then, run `stow <config_name>` (i.e `stow i3`) for each directory in the repo.
- Run `p10k configure` to setup powerlevel10k.
## Neovim setup
TBD
## tmux setup
TBD

## Handy links
- https://i3wm.org/docs/debugging.html

## Future improvements
- Create dotfiles repo
- Make installation script to automate setup
- Use docker for automated testing of install script
- Fix picom settings
- Beautify things
- Consider using https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/archlinux

## Known issues
- picom flickers when changing workspaces - https://github.com/yshui/picom/issues/16
- Some picom issue that causes windows to disappear when they are tiled with transparency enabled
