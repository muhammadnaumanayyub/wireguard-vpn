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

## Step 3 – Generate WireGuard Server Keys

WireGuard uses public/private key cryptography to authenticate VPN peers.

Each peer must have a unique key pair.

### Generate Private Key

```bash
sudo wg genkey | sudo tee /etc/wireguard/server_private.key

### Generate Public keys

sudo cat /etc/wireguard/server_private.key | wg pubkey | sudo tee /etc/wireguard/server_public.key

### Verify Keys

sudo cat /etc/wireguard/server_private.key
sudo cat /etc/wireguard/server_public.key


