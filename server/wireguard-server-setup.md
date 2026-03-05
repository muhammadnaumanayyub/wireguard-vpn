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

## Step 4 – Create WireGuard Server Configuration

WireGuard uses a configuration file called `wg0.conf` to define the VPN interface settings.

The configuration file is stored in:

/etc/wireguard/wg0.conf

### Create Configuration File

```bash
sudo nano /etc/wireguard/wg0.conf

### Add interface Configuration

[Interface]
Address = 10.0.0.1/24
ListenPort = 51820
PrivateKey = SERVER_PRIVATE_KEY

##Replace SERVER_PRIVATE_KEY with the private key generated earlier.



