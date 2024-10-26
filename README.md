# Eric's Blog
### Installing Arch Linux
- Create Arch install USB
- Use UEFI to boot from USB
- Run `archinstall` command to begin installation
- Before running that command, you might have to connect to the network using `iwctl`. Once in interactive mode run:
  - `device list`
  - `station wlan0 scan`
  - `station wlan0 get-networks`
  - `station wlan0 connect SSID`
- Select preferences in `archinstall` script accordingly and begin installation.
- Most recently I chose to use `ext4` and not to install `NetworkManager` (used ISO settings instead). My plan was to install `NetworkManager` post-install at some point.
### Post-installation
- Once installation is complete, you may reboot. Or you may choose to perform post-installation as chroot. It does not matter really.
- Install GNOME using `sudo pacman -S gnome` and select defaults for everything
- Run `sudo systemctl enable gdm.service` and then `sudo systemctl start gdm.service`. This will take you into GNOME Desktop Manager and launch it by default on startup.
- To get SSH working, run `sudo systemctl enable sshd` and `sudo systemctl start sshd`.
- Next, it is time to install Git! Run `sudo pacman -S --needed git base-devel`.
- This is a prerequisite for installing `yay`. To do that run the following:
  - `git clone https://aur.archlinux.org/yay.git && cd yay && makepkg -si`
- Next install Xrdp using the following instructions:
  - https://wiki.archlinux.org/title/Xrdp
