WireGuard VPN Deployment on Ubuntu VPS

This repository documents the process of deploying a WireGuard VPN server on an Ubuntu VPS and connecting a Windows client to establish a secure encrypted tunnel.

The objective of this deployment was to build a simple and secure VPN environment using WireGuard while gaining practical experience with VPN configuration, Linux networking, routing, and NAT.

Environment

Server
Ubuntu 24.04 LTS VPS

Client
Windows Laptop

VPN Software
WireGuard

Protocol
UDP 51820

VPN Network
10.0.0.0/24

Network Topology
Windows Laptop (WireGuard Client)
        │
        │ Encrypted VPN Tunnel
        │ UDP 51820
        │
Internet
        │
        │
Ubuntu VPS (WireGuard Server)
VPN IP: 10.0.0.1

The VPN network used in this deployment:

Server VPN IP : 10.0.0.1
Client VPN IP : 10.0.0.2
Deployment Overview

The following steps were performed during the deployment of the WireGuard VPN server.

Updated the system packages

Installed WireGuard on the VPS

Generated server public and private keys

Created the WireGuard interface configuration

Enabled IP forwarding for packet routing

Configured NAT so VPN clients can access external networks

Started the WireGuard interface

Configured and connected a Windows client

Server Configuration

The server configuration includes:

• Installing WireGuard on Ubuntu
• Creating the VPN interface configuration (wg0.conf)
• Enabling routing and NAT
• Starting and verifying the VPN service

Detailed server setup steps can be found in:

server/wireguard-server-installation.md
Client Configuration

A Windows system was configured as the VPN client using the official WireGuard application.

The client configuration includes:

• Installing WireGuard for Windows
• Generating a client key pair
• Adding the client public key to the server configuration
• Creating the client tunnel configuration
• Activating the VPN connection

Client configuration details are available here:

client/wireguard-client-setup.md
Verification

After completing the configuration, the VPN tunnel was tested to confirm connectivity between the client and the server.

On the Windows client:

ping 10.0.0.1

Successful replies confirm that the client can reach the server through the encrypted VPN tunnel.

On the server:

sudo wg

The output displays the peer connection along with handshake and traffic statistics.

Result

The WireGuard VPN deployment was successfully completed.

Client Device
Windows Laptop

Server
Ubuntu 24.04 VPS

VPN Network
10.0.0.0/24

Protocol
UDP 51820

The client is now able to securely communicate with the server through an encrypted VPN tunnel.

Author

Muhammad Nauman Ayyub
Network & Cloud Security Engineer