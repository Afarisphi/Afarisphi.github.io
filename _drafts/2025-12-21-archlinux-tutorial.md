---
layout: post
title: "How I Installed Arch Linux in Dual Boot (UEFI + Archinstall)"
date: 2025-12-21
categories: linux archlinux
tags: archlinux dualboot uefi
published: false
---

# How I Installed Arch Linux in Dual Boot

## Introduction

This post documents how I installed **Arch Linux in a dual boot setup** using **UEFI**, **manual disk preparation**, and **Archinstall**.  
The installation was performed alongside an existing operating system using **unallocated disk space**, without removing the existing OS.

This tutorial is written from a **practical, step-by-step perspective**, explaining *what is being done* and *why it is needed*, while still keeping all commands in a single, readable flow.

This guide assumes:
- UEFI-based system
- NVMe disk (`/dev/nvme0n1`)
- Wi-Fi internet connection
- NVIDIA GPU

---

## Full Installation Command Flow

Below is the **complete command flow**, arranged in the same order as I executed it during installation.  
Commands are grouped logically, and inline comments explain the purpose of each step.

> ⚠️ Notes:
> - `iwctl`, `cfdisk`, and `archinstall` are **interactive tools**
> - Read the comments carefully before executing destructive commands

```bash
#################################
# 1. CONNECT TO THE INTERNET
#################################

# Enter iwd interactive shell
iwctl

# List available wireless devices
device list

# Scan and list available Wi-Fi networks
station wlan0 get-networks

# Connect to the desired Wi-Fi network
station wlan0 connect "Infinix HOT 11S NFC"

# Exit iwctl
exit

# Verify internet connectivity
ping archlinux.org


#################################
# 2. PREPARE INSTALLATION TOOLS
#################################

# Sync package database
pacman -Sy

# Update keyring to avoid signature issues
pacman -S archlinux-keyring

# Install Archinstall
pacman -S archinstall


#################################
# 3. CHECK CURRENT DISK LAYOUT
#################################

# View existing partitions and free space
lsblk


#################################
# 4. PARTITION THE DISK (DUAL BOOT)
#################################

# Open cfdisk on the NVMe disk
cfdisk /dev/nvme0n1

# Inside cfdisk:
# - Select unallocated / free space
# - Create 1GB partition → set type to EFI System
# - Use remaining space → Linux filesystem
# - Write changes
# - Quit

# Verify new partitions
lsblk


#################################
# 5. FORMAT THE NEW PARTITIONS
#################################

# Format EFI partition as FAT32
mkfs.fat -F32 /dev/nvme0n1p6

# Format root partition as ext4
mkfs.ext4 /dev/nvme0n1p7

# Verify formatting
lsblk


#################################
# 6. MOUNT PARTITIONS
#################################

# Mount root partition
mount /dev/nvme0n1p7 /mnt

# Create boot directory
mkdir /mnt/boot

# Mount EFI partition
mount /dev/nvme0n1p6 /mnt/boot

# Final verification before install
lsblk


#################################
# 7. INSTALL ARCH LINUX
#################################

# Start Archinstall (interactive installer)
archinstall

# Important options inside Archinstall:
# - Disk configuration → Pre-mounted configuration
# - Mountpoint → /mnt
# - Bootloader → GRUB
# - Swap → Enabled
# - Desktop → Hyprland
# - Graphics driver → NVIDIA (proprietary)
# - Network → NetworkManager
# - Timezone → Asia/Jakarta
# - Create user + root password


#################################
# 8. POST-INSTALL (CHROOT)
#################################

# Install required boot packages
pacman -S grub efibootmgr dosfstools mtools

# Install GRUB to EFI
grub-install \
  --target=x86_64-efi \
  --efi-directory=/boot \
  --bootloader-id=GRUB


#################################
# 9. ENABLE DUAL BOOT DETECTION
#################################

# Install os-prober to detect other OSes
sudo pacman -S os-prober

# Edit GRUB configuration
sudo nano /etc/default/grub

# Make sure these lines are set:
# GRUB_TIMEOUT=30
# GRUB_DISABLE_OS_PROBER=false

# Generate final GRUB configuration
sudo grub-mkconfig -o /boot/grub/grub.cfg
