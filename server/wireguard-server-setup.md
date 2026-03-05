# WireGuard VPN Server Installation

## Lab Environment

Server: Linux VPS  
VPN Software: WireGuard  
Protocol: UDP 51820

---

## Step 1 – Update System

Update package repositories and upgrade installed packages.

```bash
sudo apt update && sudo apt upgrade -y

## Step 2 – Install WireGuard

Install WireGuard VPN package.

```bash
sudo apt install wireguard -y

wg --version


