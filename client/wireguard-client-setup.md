WireGuard Client Configuration (Windows)

This document explains how to configure a Windows client to connect to the WireGuard VPN server deployed on the Ubuntu VPS.

The client will establish a secure encrypted tunnel to the server using WireGuard.

Client Environment

Client Device
Windows Laptop

VPN Client Software
WireGuard for Windows

VPN Server
Ubuntu 24.04 VPS

VPN Network
10.0.0.0/24

Server VPN IP
10.0.0.1

Client VPN IP
10.0.0.2

Step 1 – Install WireGuard Client

Download the official WireGuard client from the WireGuard website.

https://www.wireguard.com/install/

Select:

WireGuard for Windows

Install the application and launch it.

Step 2 – Create a New Tunnel

Inside the WireGuard application:

Click Add Tunnel

Select Add Empty Tunnel

WireGuard will automatically generate a private key and public key.

Example:

Private Key: XXXXXXXXXXXXXX
Public Key:  YYYYYYYYYYYYYY

Copy the Public Key, as it will be added to the server configuration.

Step 3 – Add Client to the Server

On the VPS server, edit the WireGuard configuration file.

sudo nano /etc/wireguard/wg0.conf

Add the following configuration:

[Peer]
PublicKey = CLIENT_PUBLIC_KEY
AllowedIPs = 10.0.0.2/32

Restart the WireGuard service.

sudo wg-quick down wg0
sudo wg-quick up wg0

Verify configuration.

sudo wg
Step 4 – Configure Client Tunnel

Return to the WireGuard Windows application.

Replace the default configuration with the following.

[Interface]
PrivateKey = CLIENT_PRIVATE_KEY
Address = 10.0.0.2/24
DNS = 8.8.8.8

[Peer]
PublicKey = SERVER_PUBLIC_KEY
Endpoint = VPS_PUBLIC_IP:51820
AllowedIPs = 0.0.0.0/0
PersistentKeepalive = 25

Replace the following values:

CLIENT_PRIVATE_KEY → private key generated in the client
SERVER_PUBLIC_KEY → server public key
VPS_PUBLIC_IP → public IP address of the VPS

Example:

Endpoint = 45.12.34.56:51820
Step 5 – Activate the VPN Tunnel

Click Activate in the WireGuard application.

If the configuration is correct, the tunnel status will change to:

Active
Step 6 – Verify VPN Connectivity

Open Command Prompt on the Windows client and test connectivity.

ping 10.0.0.1

Expected result:

Reply from 10.0.0.1

This confirms that the client can reach the VPN server through the encrypted tunnel.

Step 7 – Verify Connection on the Server

Run the following command on the VPS server.

sudo wg

Example output:

interface: wg0
peer: CLIENT_PUBLIC_KEY
endpoint: CLIENT_PUBLIC_IP
allowed ips: 10.0.0.2/32
latest handshake: few seconds ago

The latest handshake confirms that the client is connected to the VPN server.

Result

The Windows client is successfully connected to the WireGuard VPN server.

Client Device
Windows Laptop

VPN Server
Ubuntu 24.04 VPS

VPN Network
10.0.0.0/24

Connection
Encrypted WireGuard VPN Tunnel

Author

Muhammad Nauman Ayyub
Network & Cloud Security Engineer