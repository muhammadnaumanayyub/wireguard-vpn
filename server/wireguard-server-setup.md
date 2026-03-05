# WireGuard VPN Server Installation

## Lab Environment

Server: Linux VPS  
VPN Software: WireGuard  
Protocol: UDP 51820

---

## Step 1 – Update System

# Update package repositories and upgrade installed packages.

sudo apt update && sudo apt upgrade -y

## Step 2 – Install WireGuard

# Install WireGuard VPN package.

sudo apt install wireguard -y

wg --version

## Step 3 – Generate WireGuard Server Keys

# WireGuard uses public/private key cryptography to authenticate VPN peers.

# Each peer must have a unique key pair.

# Generate Private Key

sudo wg genkey | sudo tee /etc/wireguard/server_private.key

# Generate Public keys

sudo cat /etc/wireguard/server_private.key | wg pubkey | sudo tee /etc/wireguard/server_public.key

# Verify Keys

sudo cat /etc/wireguard/server_private.key
sudo cat /etc/wireguard/server_public.key

### Step 4 – Create WireGuard Server Configuration

# WireGuard uses a configuration file called `wg0.conf` to define the VPN interface settings.

# The configuration file is stored in:

/etc/wireguard/wg0.conf

### Create Configuration File

```bash
sudo nano /etc/wireguard/wg0.conf

### Add interface Configuration

[Interface]
Address = 10.0.0.1/24
ListenPort = 51820
PrivateKey = SERVER_PRIVATE_KEY

# Replace SERVER_PRIVATE_KEY with the private key generated earlier.

## Step 5 – Enable IP Forwarding

# Linux does not forward network packets between interfaces by default.
# To allow the VPN server to route traffic between the VPN clients and the internet, IP forwarding must be enabled.

# Check Current Status

cat /proc/sys/net/ipv4/ip_forward

#If the result is 0, forwarding is disabled.

# Enable IP Forwarding Temporarily

sudo sysctl -w net.ipv4.ip_forward=1

# Enable IP Forwarding Permanently
sudo nano /etc/sysctl.conf

# Uncomment the following line:
net.ipv4.ip_forward=1
# Apply the Configuration
sudo sysctl -p

# Verify Configuration
cat /proc/sys/net/ipv4/ip_forward

# If the output is 1, IP forwarding is successfully enabled.
