This project demonstrates the process of installing and configuring arch linux from scratch. The way I did it was that the installation was done manually, following the instructions from the wiki site, but also with the help of a youtube video called, "How To Install Arch Linux in VirtualBox (2025) | Arch Linux Installation"
Arch Linux Installation and Setup Documentation

Course: CYB-3353 System Administration
Project: Arch Install and Setup (Project 1 – Part A)
Student: Gerald Aldric Waworuntu
Date: November 2, 2025

1. Introduction

This project shows how I installed and set up Arch Linux on VirtualBox.
I followed the Arch Linux Wiki and used a video for help.
My goal was to make a working system with a desktop, users, SSH, and a better terminal.

2. Virtual Machine Setup

I used Oracle VM VirtualBox on Windows 11.
The ISO file was archlinux-2025.10.01-x86_64.iso.

VM Specs:

2 CPU cores

4 GB RAM

25 GB storage

Network: NAT

After booting, I connected to the internet and installed the base system using pacstrap.

3. Base Installation
ping archlinux.org
timedatectl set-ntp true
fdisk /dev/sda
mkfs.ext4 /dev/sda1
mount /dev/sda1 /mnt
pacstrap /mnt base linux linux-firmware vim nano networkmanager sudo
genfstab -U /mnt >> /mnt/etc/fstab


This set up the basic Arch files and system.

4. System Configuration
arch-chroot /mnt
ln -sf /usr/share/zoneinfo/America/Chicago /etc/localtime
hwclock --systohc
echo "archlinux" > /etc/hostname


Then I set my timezone and language, and generated locale with locale-gen.

5. Desktop Environment

I picked GNOME because it looks clean and is easy to use.

sudo pacman -S xorg gdm gnome gnome-tweaks
sudo systemctl enable gdm
sudo systemctl start gdm


After reboot, the system went straight into GNOME.
(Insert screenshot of your GNOME desktop here.)

6. User Accounts

I made two users: gerald and codi.
Both have admin (sudo) access.

useradd -m -G wheel -s /bin/zsh gerald
useradd -m -G wheel -s /bin/zsh codi
passwd gerald
passwd codi
EDITOR=nano visudo


Then I enabled the wheel group for sudo in visudo.

7. Shell Setup (ZSH)

I changed the shell from bash to zsh.

sudo pacman -S zsh
chsh -s /bin/zsh gerald
chsh -s /bin/zsh codi


(Insert screenshot showing zsh terminal.)

8. SSH Setup

SSH lets me connect to the system remotely.

sudo pacman -S openssh
sudo systemctl enable sshd
sudo systemctl start sshd
systemctl status sshd


(Insert screenshot showing sshd active.)

9. Terminal Colors

I made the terminal more colorful and readable.

alias ls='ls --color=auto'
export PS1='%F{green}%n@%m%f:%F{blue}%~%f$ '

10. Custom Aliases

I added shortcuts to make typing faster.

alias update='sudo pacman -Syu'
alias ll='ls -alF'
alias grep='grep --color=auto'
alias cls='clear'
alias ..='cd ..'
alias ports='ss -tuln'


(Insert screenshot showing aliases working.)

11. Boot to GUI

To make the system open the desktop right away:

sudo systemctl enable gdm
sudo reboot


It now loads GNOME by default.

12. Extra Tools

I added more apps and tools.

sudo pacman -S htop neofetch firefox libreoffice


These help with testing, browsing, and system checks.

13. Conclusion

The Arch Linux setup worked well.
Now it has:

GNOME desktop

Two user accounts

ZSH shell

SSH access

Aliases and color terminal

GUI auto boot

This project helped me understand Linux setup and system control better.
