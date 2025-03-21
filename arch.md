# Absolute Beginner Arch Linux Configuration Guide (From Terminal)
## Network Management
You will need to connect to ınternet so here is the code. If you are using `Networkmanager`.
### CLI Connection:
```bash
nmcli device wifi connect "SSID" password "PASSWORD"
```
### Diagnostics:
```bash
ip -c a                   # Colorized interface overview
iwctl                     # Alternative WiFi CLI (iwd)
journalctl -u NetworkManager -xe  # Filtered logs
```

## Package Management Hierarchy 1

Pacman is the offical package manager for Arch linux. If you want to install anything, you will need to know how it works.

### Official Packages (pacman)
```bash
sudo pacman -Syu          # Full system upgrade
sudo pacman -S <pkg>      # Install specific package
sudo pacman -Rns <pkg>    # Remove + dependencies
```

> [!WARNING]  
> Just try `sudo pacman -S cmatrix` then type `cmatrix` on the console.

#### List All Explicitly Installed Packages
```bash
pacman -Qe                # These are packages you installed manually (not dependencies)
pacman -Qdt               # List orphaned packages that might have been installed as dependencies
```


## Version Control Essentials and Github

### Git Fundamentals

`git` will be your friend. Install git with `sudo pacman -S git` and you will be able to use `git clone <github-url>`. Now you are able to install any package present on github.

With git you can also create your own repositories. Use them to store your dotfiles or create amazing projects. For that you will need these commands.
```bash
git add <file>              # 
git commit -m "your commit"
git push origin main
```

> [!WARNING]  
> Sometimes Github will ask you who you are. If you dont want to type your name and password everytime you try to do push or don't know what to do, just look at Github CLI Worflow part.

### GitHub CLI Workflow

`github-cli` will handle all your authentication process. You will never have to type your password again (I hope). Install it with `sudo pacman -S github-cli` and follow the steps.

#### Authentication:
```bash
gh auth login -w            # Web-based auth flow
gh config set git_protocol ssh  # Always use SSH
```

#### Repository Management:
```bash
gh repo create --private  # New private repo
gh repo clone user/repo   # SSH-based clone
gh issue create -t "Title" -b "Body"  # Quick issue
```

> [!WARNING]  
> Gıthub CLI is good for authentication but its not what git is. I strongly recommend learning git.


## Package Management Hierarchy 2

### AUR Management (yay)

Not every package is covered by pacman, at some point you will need to use aur helpers like yay. You can find entire list of AUR packages on https://aur.archlinux.org/

#### Installation:

If you are migrating from another AUR helper, you can simply install Yay with that helper.

> [!WARNING]  
> I am using `sudo` in these examples, you can switch that out for a different privilege escalation tool.

#### Source

The initial installation of Yay can be done by cloning the PKGBUILD and building with makepkg:

Make sure you have the `base-devel` package group installed.

```bash
sudo pacman -S --needed git base-devel
git clone https://aur.archlinux.org/yay-bin.git
cd yay-bin 
makepkg -si
```

If you want to do all of this at once, here is the commands like so:

```sh
sudo pacman -S --needed git base-devel && git clone https://aur.archlinux.org/yay.git && cd yay && makepkg -si
```

#### Usage Patterns:
It is recommended to **not** use `sudo` with yay

```bash
yay <pkg>                   # Search for packages
yay -S <pkg>                # Install specific package
yay -Rns <pkg>              # Remove + dependencies
```

#### List All AUR Packages Installed with yay
```bash
yay -Qm                   # To see which packages were installed from the AUR using yay
```

> [!WARNING]  
> If you run `yay` it will first update your system and THEN the AUR packages. So even if you do `sudo pacman -Syu` and then `yay`, it's just faster to do `yay`.



# Oher Things to Point Out

## Critical System Commands

### Storage Analysis
```bash
df -hT --exclude-type=tmpfs  # Filter temporary filesystems
du -sh ~/* | sort -h        # User directory sizes
fdisk -l                    # Partition table
```

### Process Inspection
```bash
htop                        # Interactive process viewer
lsof -i :<port>             # Identify port users
systemd-analyze critical-chain  # Boot performance
```

### Security Audit
```bash
sudo journalctl -k _TRANSPORT=kernel  # Kernel messages
sudo ausearch -k -m avc              # SELinux denials
sudo netstat -tulpn                  # Active connections
```


## Dotfiles Strategy

### Maintenance Protocol

#### Profile Isolation
- Separate work/personal/server profiles

#### Secret Management
```bash
git secret init         # Requires git-secret
git secret add .env
git secret hide
```

#### Stow Testing
```bash
stow -nv <profile>      # Dry-run simulation
stow -tv <profile>      # Verify symlinks
```

#### Bootstrap Script
Maintain `bootstrap.sh` with:
```bash
#!/bin/bash
sudo pacman -S --needed - < pkglist.txt
stow -vt ~ profiles/base
```

## System Recovery Essentials

### Chroot Access
```bash
mount /dev/sdX# /mnt
arch-chroot /mnt
```

### Boot Repair
```bash
pacman -S efibootmgr
efibootmgr -c -d /dev/sdX -p Y -L "Arch Linux" -l \vmlinuz-linux -u "initrd=\initramfs-linux.img"
```

### Kernel Rebuild
```bash
pacman -S linux-headers
mkinitcpio -P
```
