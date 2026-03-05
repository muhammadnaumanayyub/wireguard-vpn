WireGuard VPN Server Installation
Lab Environment

Server: Linux VPS
Operating System: Ubuntu 24.04 LTS
VPN Software: WireGuard
Protocol: UDP 51820
VPN Network: 10.0.0.0/24

Step 1 – Update the System

Update the package repositories and upgrade installed packages.

sudo apt update && sudo apt upgrade -y

Step 2 – Install WireGuard

Install the WireGuard VPN package.

sudo apt install wireguard -y

Verify the installation.

wg --version

Step 3 – Generate WireGuard Server Keys

WireGuard uses public and private key cryptography for authentication.
Each peer in the VPN must have its own key pair.

Generate the server private key.

sudo wg genkey | sudo tee /etc/wireguard/server_private.key

Generate the server public key from the private key.

sudo cat /etc/wireguard/server_private.key | wg pubkey | sudo tee /etc/wireguard/server_public.key

Verify the generated keys.

sudo cat /etc/wireguard/server_private.key
sudo cat /etc/wireguard/server_public.key

Step 4 – Create WireGuard Server Configuration

WireGuard uses a configuration file called wg0.conf to define the VPN interface.

Configuration file location:

/etc/wireguard/wg0.conf

Create the configuration file.

sudo nano /etc/wireguard/wg0.conf

Add the following configuration.

[Interface]
Address = 10.0.0.1/24
ListenPort = 51820
PrivateKey = SERVER_PRIVATE_KEY

Replace SERVER_PRIVATE_KEY with the private key generated in Step 3.

Step 5 – Enable IP Forwarding

Linux does not forward packets between network interfaces by default.
To allow VPN clients to access the internet through the server, IP forwarding must be enabled.

Check the current status.

cat /proc/sys/net/ipv4/ip_forward

If the result is 0, forwarding is disabled.

Enable IP forwarding temporarily.

sudo sysctl -w net.ipv4.ip_forward=1

Enable IP forwarding permanently.

sudo nano /etc/sysctl.conf

Uncomment the following line.

net.ipv4.ip_forward=1

Apply the configuration.

sudo sysctl -p

Verify that IP forwarding is enabled.

cat /proc/sys/net/ipv4/ip_forward

If the output is 1, IP forwarding is successfully enabled.

Step 6 – Configure NAT (Network Address Translation)

VPN clients use private IP addresses that are not routable on the public internet.
To allow VPN clients to access the internet through the VPN server, NAT must be configured.

Identify the internet-facing interface.

ip a

Locate the interface connected to the internet (commonly eth0).

Add a NAT rule using iptables.

sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

Verify the NAT rule.

sudo iptables -t nat -L

If the MASQUERADE rule appears, NAT has been configured successfully.

Step 7 – Start the WireGuard VPN Interface

Start the WireGuard VPN interface.

sudo wg-quick up wg0

This command reads the configuration file and creates the virtual VPN interface.

Verify that the interface has been created.

ip a

You should see the wg0 interface with the VPN IP address.

Check the WireGuard status.

sudo wg

Enable WireGuard to start automatically when the server boots.

sudo systemctl enable wg-quick@wg0

Step 8 – Configure the WireGuard Client (Windows)

After configuring the server, a client device must be configured to connect to the VPN.

In this lab, the client device is a Windows laptop.

Step 8.1 – Install WireGuard on Windows

Download the official WireGuard client from:

https://www.wireguard.com/install/

Select WireGuard for Windows and install it.

After installation, open the WireGuard application.

Step 8.2 – Create a New Tunnel

Inside the WireGuard application:

Click Add Tunnel
Select Add Empty Tunnel

WireGuard will automatically generate a key pair.

Private Key: (generated automatically)
Public Key: (generated automatically)

Copy the Public Key, as it will be added to the server configuration.

Step 8.3 – Add Client to the Server Configuration

Edit the server configuration file.

sudo nano /etc/wireguard/wg0.conf

Add the following section at the bottom of the file.

[Peer]
PublicKey = CLIENT_PUBLIC_KEY
AllowedIPs = 10.0.0.2/32

Save the file.

Restart the WireGuard interface.

sudo wg-quick down wg0
sudo wg-quick up wg0

Verify the configuration.

sudo wg

The client should now appear as a peer.

Step 8.4 – Configure the Client Tunnel

In the WireGuard Windows application, replace the default configuration with the following.

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

CLIENT_PRIVATE_KEY → Client private key generated in the WireGuard app
SERVER_PUBLIC_KEY → Server public key from the VPS
VPS_PUBLIC_IP → Public IP address of the VPS

Example endpoint:

Endpoint = 45.12.34.56:51820

Step 8.5 – Activate the VPN Tunnel

Click Activate in the WireGuard application.

The tunnel status should change to Active, indicating that the VPN connection has been established.

Step 8.6 – Verify VPN Connectivity

Open Command Prompt on the Windows client and test connectivity.

ping 10.0.0.1

Expected output:

Reply from 10.0.0.1

This confirms that the VPN tunnel between the client and server is working.

Step 8.7 – Verify Connection on the Server

Run the following command on the VPS.

sudo wg

Example output:

interface: wg0
peer: CLIENT_PUBLIC_KEY
endpoint: CLIENT_PUBLIC_IP
allowed ips: 10.0.0.2/32
latest handshake: few seconds ago
transfer: data transmitted

The latest handshake confirms that the client is actively connected.

Result

The WireGuard VPN tunnel has been successfully established.

Client Device : Windows Laptop
VPN Server : Ubuntu 24.04 VPS
VPN Network : 10.0.0.0/24
Protocol : UDP 51820

The client can now securely communicate with the VPN server through an encrypted tunnel.