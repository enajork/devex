# Creating a developer workflow for enhanced productivity
## Installing Arch
- Create Arch installation media USB.
- Use UEFI to boot from USB.
- Run `archinstall` command to begin installation.
- Before running that command, you might have to connect to the network using `iwctl`. Once in interactive mode run:
  - `device list`
  - `station wlan0 scan`
  - `station wlan0 get-networks`
  - `station wlan0 connect SSID`
- Select preferences in `archinstall` script accordingly and begin installation.
- Most recently I chose to use ext4 for the file system and to skip installation of `NetworkManager` (used ISO settings instead). It is possible to install `NetworkManager` after Arch.
### Post-installation
- Once installation is complete, you may reboot. Alternatively, `archinstall` allows for post-installation steps to be completed as chroot as the final stage of installation.
- Install GNOME using `sudo pacman -S gnome` and select defaults for everything. Note: all installation related to GDM is optional. i3 will be the primary window manager whereas GDM is a backup.
- (**Optional**) Run `sudo systemctl enable gdm.service` and then `sudo systemctl start gdm.service`. This will take you into GNOME Desktop Manager and launch it by default on startup.
- To get SSH working, run `sudo systemctl enable sshd` and `sudo systemctl start sshd`.
- Next, it is time to install Git. Run `sudo pacman -S --needed git base-devel`.
- Git is a prerequisite for installing yay. To install yay that run the following:
  - `git clone https://aur.archlinux.org/yay.git && cd yay && makepkg -si`
### Xrdp
- Next install Xrdp using the following instructions:
  - https://wiki.archlinux.org/title/Xrdp
    - Install `xrdp` and `xorgxrdp` using `yay`.
    - (**Optional**) Create `~/.xinitrc` and add the one-liner `exec gnome-session`. Or, `exec i3` if GDM is not being used.
    - Run `sudo systemclt set-default multi-user.target`. This will allow for multiple user sessions to be exist at once.
    - Reboot using `sudo reboot`.
  - Remote desktop should be working for GDM. Next we will need to configure i3 for Xrdp.
### i3
- Run `sudo pacman -S i3` to install i3.
- Change `~/.xinitrc` to `exec i3`. Restart xrdp using `sudo systemctl restart xrdp` to pick up changes.
That's it. Arch, Xrdp, and i3 have been installed. The "ricing" (customization) of the system can now begin.
## Ricing
