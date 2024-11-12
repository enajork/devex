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
## ~~Mutagen~~
~~More details at https://mutagen.io/. Think of this as a more performant `sshfs` (https://github.com/libfuse/sshfs). It enables us to reap the benefits of our local editor customization even on remote machines. We can mount a remote machine's file system and edit as if the files were located on the client using our neovim config and more.~~
After some testing, Mutagen is not suitable for my purposes. It poses a security risk, consumes too much disk space, and initial syncing takes too long. SSHFS is also not a viable option.
### ~~Installation~~
- ~~Installation instructions can be found [here](https://mutagen.io/documentation/introduction/installation).~~
- ~~Assuming you are not using `homebrew`, run `curl -O -L https://github.com/mutagen-io/mutagen/releases/download/v0.18.0/mutagen_linux_amd64_v0.18.0.tar.gz` (replace version and architecture accordingly).~~
- ~~Extract the executable to the user's local binary directory by running `tar -xzf mutagen_linux_amd64_v0.18.0.tar.gz -C /usr/local/bin`.~~
- ~~Remove leftover tarball with `rm mutagen_linux_amd64_v0.18.0.tar.gz`.~~
  - ~~Alternatively, install with the one-liner `curl -O -L https://github.com/mutagen-io/mutagen/releases/download/v0.18.0/mutagen_linux_amd64_v0.18.0.tar.gz && tar -xzf mutagen_linux_amd64_v0.18.0.tar.gz -C /usr/local/bin && rm mutagen_linux_amd64_v0.18.0.tar.gz`.~~
### ~~Usage~~
- ~~Usage instructions can be found [here](https://mutagen.io/documentation/introduction/getting-started).~~
- ~~To create a sync session run `mutagen sync create --name=<session name> <client_dir> <username>@<host>:<remote_dir>`. This will sync the remote directory to the client directory specified. It can take a few minutes for the sync to complete.~~
## Neovim setup
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
- ~~Some picom issue that causes windows to disappear when they are tiled with transparency enabled~~ Fixed by using `use-damage = false;` setting.
