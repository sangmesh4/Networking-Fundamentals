<div align="center">

# 🔐 𝗩𝗣𝗡 & 𝗧𝘂𝗻𝗻𝗲𝗹𝗶𝗻𝗴

### 𝘈 𝘊𝘰𝘮𝘱𝘭𝘦𝘵𝘦 𝘕𝘦𝘵𝘸𝘰𝘳𝘬𝘪𝘯𝘨 & 𝘋𝘦𝘷𝘖𝘱𝘴 𝘎𝘶𝘪𝘥𝘦

![Networking](https://img.shields.io/badge/Topic-Networking-blue?style=for-the-badge&logo=cisco&logoColor=white)
![Security](https://img.shields.io/badge/Focus-Security-red?style=for-the-badge&logo=letsencrypt&logoColor=white)
![WireGuard](https://img.shields.io/badge/WireGuard-88171A?style=for-the-badge&logo=wireguard&logoColor=white)
![OpenVPN](https://img.shields.io/badge/OpenVPN-EA7E20?style=for-the-badge&logo=openvpn&logoColor=white)
![DevOps](https://img.shields.io/badge/Level-DevOps-success?style=for-the-badge&logo=linux&logoColor=white)

*Encrypted tunnels • Protocols • Setup • Troubleshooting • Real-world use cases*

</div>

---

## 📑 Table of Contents

| # | Section | Topics |
|:-:|---------|--------|
| 1️ | [What is a VPN?](#1--what-is-a-vpn) | Core problem, what a VPN does, components |
| 2️ | [Types of VPNs](#2--types-of-vpns) | Remote access, site-to-site, cloud |
| 3️ | [How VPN Works](#3--how-vpn-works) | Data flow, IP masking |
| 4️ | [VPN Protocols](#4--vpn-protocols) | OpenVPN, WireGuard, IPSec, SSL/TLS |
| 5️ | [Tunneling Concepts](#5--tunneling-concepts) | GRE, VXLAN, IP-in-IP, split tunnel, overhead |
| 6️ | [Real-World Examples](#6--real-world-examples) | Remote work, AWS, bastion host |
| 7️ | [Setting Up a Simple VPN](#7--setting-up-a-simple-vpn) | WireGuard server & client |
| 8️ | [SSH Tunneling](#8--ssh-tunneling-port-forwarding) | Local, remote, dynamic forwarding |
| 9️ | [Cloud VPN Examples](#9--cloud-vpn-examples) | AWS, Azure |
| 10 | [VPN vs Proxy vs Tor](#10--vpn-vs-proxy-vs-tor) | Comparison |
| 1️1️ | [Common Commands](#11--common-commands) | Check status, WireGuard, OpenVPN |
| 1️2️ | [Split Tunneling (Quick Reference)](#12--split-tunneling-quick-reference) | Config example |
| 1️3️ | [Troubleshooting](#13--troubleshooting) | Connection, internet, speed |
| 1️4️ | [DevOps Use Cases](#14--devops-use-cases) | Private access, hybrid, CI/CD |
| 1️5️ | [Key Takeaways](#15--key-takeaways) | Summary |

---

# 1. 🌐 What is a VPN?

> **VPN (Virtual Private Network)** creates a secure, encrypted connection over a public network (like the internet).

## 1.1 🧩 The Core Problem VPNs Solve

**Scenario**: Company has office in New York and branch in London.

**Option 1: Dedicated Line** (Pre-VPN Solution)
```
New York Office ←------- Leased Line -------→ London Office
                  (Very expensive physical cable)

Cost: $10,000+/month
Advantages: Secure, private, high bandwidth
Disadvantages: Extremely expensive, inflexible, long setup time
```

**Option 2: VPN** (Modern Solution)
```
New York Office ←------ Internet ------→ London Office
            (Encrypted tunnel through public internet)

Cost: $50/month + internet
Advantages: Cheap, flexible, quick setup
Disadvantages: Dependent on internet quality
```

> 💡 **VPN creates "virtual" private network over public infrastructure.**

## 1.2 ⚙️ What VPN Actually Does

### 1.2.1 📦 Encapsulation (Tunneling)
```
Original Packet:
[IP Header][TCP Header][Data: "secret message"]

After VPN Encapsulation:
[VPN IP Header][VPN Header][Encrypted:[IP Header][TCP Header][Data: "secret message"]]
     ↑                            ↑
  Outer packet             Inner packet (encrypted)
  (for internet routing)   (actual data, hidden)
```

### 1.2.2 🔒 Encryption
```
Without VPN:
Your Computer → "username=admin&password=secret" → Website
               (ISP, government, hackers can read this)

With VPN:
Your Computer → VPN: "a7f3b9c2d..." → Website
               (ISP sees encrypted gibberish)
               
VPN Server → "username=admin&password=secret" → Website
            (Decrypted at VPN server)
```

### 1.2.3 🎭 IP Address Masking
```
Your Real IP: 203.0.113.5 (New York)
              ↓
            VPN Tunnel
              ↓
VPN Server IP: 198.51.100.10 (London)
              ↓
            Website

Website sees: 198.51.100.10 (thinks you're in London)
```

## 1.3 🧱 VPN Components

| Component | Role |
|-----------|------|
| 💻 **VPN Client** | Software on your device |
| 🖥️ **VPN Server** | Endpoint on remote network |
| 📜 **VPN Protocol** | Rules for establishing and maintaining connection |
| 🚇 **VPN Tunnel** | Virtual encrypted connection |

**💻 VPN Client**: Software on your device
- Initiates VPN connection
- Encrypts outbound traffic
- Decrypts inbound traffic
- Routes traffic through tunnel

**🖥️ VPN Server**: Endpoint on remote network
- Accepts VPN connections
- Decrypts traffic from clients
- Encrypts traffic to clients
- Routes traffic to destination

**📜 VPN Protocol**: Rules for establishing and maintaining connection
- Authentication method
- Encryption algorithm
- Key exchange mechanism
- Encapsulation format

**🚇 VPN Tunnel**: Virtual encrypted connection
- Logical path through public network
- All traffic encrypted within tunnel
- Appears as single connection to outside observer

---

# 2. 🗂️ Types of VPNs

## 2.1 🏠 Remote Access VPN
Connects individual users to a private network.

```
Remote Worker (Home)
        |
    [VPN Client]
        |
    Internet
        |
  [VPN Server]
        |
  Company Network
```

**Use case**: Working from home, accessing company resources.

## 2.2 🏢 Site-to-Site VPN
Connects entire networks together.

```
Office A Network
        |
  [VPN Gateway]
        |
    Internet
        |
  [VPN Gateway]
        |
Office B Network
```

**Use case**: Connecting branch offices, connecting on-premise to cloud.

## 2.3 ☁️ Cloud VPN
Connects your network to cloud resources.

```
On-Premise Network
        |
  [VPN Gateway]
        |
    Internet
        |
  [Cloud VPN Gateway]
        |
  AWS/Azure/GCP VPC
```

**Use case**: Hybrid cloud architecture.

---

# 3. 🔄 How VPN Works

```
1. Your Computer
   ↓ (Data is encrypted)
2. VPN Client
   ↓ (Encrypted tunnel through internet)
3. VPN Server
   ↓ (Data is decrypted)
4. Destination (Website/Server)
```

**Without VPN**:
```
Your IP: 203.0.113.5 → Website sees: 203.0.113.5
```

**With VPN**:
```
Your IP: 203.0.113.5 → VPN → Website sees: 198.51.100.10 (VPN server IP)
```

---

# 4. 📡 VPN Protocols

VPN protocols define how the VPN tunnel is created and maintained. Each has different characteristics.

## 4.1 🟠 OpenVPN

**Architecture**:
- Open-source (auditable code)
- Uses OpenSSL library for encryption
- Custom protocol built on SSL/TLS
- Highly configurable

**How it works**:
```
1. Client initiates connection to OpenVPN server
2. TLS handshake (like HTTPS):
   - Certificate verification
   - Key exchange
   - Encryption negotiation
3. Virtual network interface created (tun/tap)
4. Traffic routed through encrypted tunnel
```

**Encryption**:
- Supports multiple algorithms: AES-256, ChaCha20, Blowfish
- Perfect Forward Secrecy (PFS): New keys per session
- HMAC authentication: SHA-256, SHA-512

**Transport**:
- **TCP Mode**: Reliable, slower, works everywhere
- **UDP Mode**: Faster, preferred for VPN

**Why TCP vs UDP matters**:
```
UDP Mode (Recommended):
- Lower latency
- Better for VoIP, gaming, streaming
- Handles packet loss at application layer

TCP Mode:
- Works through restrictive firewalls
- Reliable, but slower
- TCP-over-TCP problem (reliability overkill)
```

| ✅ Advantages | ❌ Disadvantages |
|--------------|-----------------|
| Most secure (proven track record) | Slower than WireGuard |
| Works on all platforms | Complex configuration |
| Bypasses most firewalls | Requires third-party client software |
| Flexible configuration | |

## 4.2 🐉 WireGuard

**Architecture**:
- Modern, minimalist design
- ~4,000 lines of code (vs OpenVPN's 400,000)
- Built into Linux kernel
- Uses state-of-the-art cryptography

**Cryptography (No Options - Simplicity)**:
- Encryption: ChaCha20
- Authentication: Poly1305
- Key Exchange: Curve25519
- Hash: BLAKE2s

**Why Fixed Cryptography?**:
```
OpenVPN: User chooses algorithm (can choose weak ones)
WireGuard: Only modern, secure algorithms available
Result: Simpler, impossible to misconfigure
```

**How it works**:
```
1. Pre-shared public keys (like SSH keys)
2. No handshake needed (stateless)
3. Cryptokey routing: IP addresses mapped to public keys
4. Extremely fast connection setup
```

**Key Features**:

**📶 Roaming**:
```
You're on WiFi (IP: 192.168.1.100)
↓
Switch to 4G (IP: 10.0.0.50)
↓
WireGuard automatically maintains connection
(Traditional VPNs would disconnect)
```

**🚀 Performance**:
```
Benchmark (approximate):
OpenVPN: 100 Mbps
IPSec: 400 Mbps
WireGuard: 1000 Mbps

WireGuard is 10x faster than OpenVPN
```

| ✅ Advantages | ❌ Disadvantages |
|--------------|-----------------|
| Extremely fast | Newer (less proven than OpenVPN) |
| Simple configuration | Static IP assignment (privacy concern for consumer VPNs) |
| Built into Linux kernel (5.6+) | Requires modern systems |
| Mobile-friendly (battery efficient) | |
| Strong security by default | |

## 4.3 🛡️ IPSec (Internet Protocol Security)

**Architecture**:
- Industry standard
- Built into many operating systems
- Suite of protocols, not single protocol
- Complex but powerful

### 4.3.1 🔀 IPSec Modes

**1. Transport Mode**:
```
Original Packet:
[IP Header][TCP Header][Data]

IPSec Transport Mode:
[IP Header][IPSec Header][Encrypted: [TCP Header][Data]]
    ↑                           ↑
  Original IP              Only payload encrypted
  (visible)
```
Used for: Host-to-host communication

**2. Tunnel Mode**:
```
Original Packet:
[IP Header][TCP Header][Data]

IPSec Tunnel Mode:
[New IP Header][IPSec Header][Encrypted: [IP Header][TCP Header][Data]]
       ↑                              ↑
  Gateway IPs                  Entire original packet encrypted
```
Used for: Site-to-site VPNs, gateway-to-gateway

### 4.3.2 🧰 IPSec Components

**AH (Authentication Header)**:
```
- Provides integrity and authentication
- No encryption
- Protects entire packet
- Rarely used alone
```

**ESP (Encapsulating Security Payload)**:
```
- Provides encryption + authentication
- Most commonly used
- Protects payload
```

**IKE (Internet Key Exchange)**:
```
Phase 1: Establish secure channel between peers
- Authenticate each peer
- Negotiate encryption algorithms
- Exchange keys

Phase 2: Establish IPSec tunnel
- Create Security Associations (SAs)
- Generate session keys
- Configure encryption parameters
```

### 4.3.3 🤯 Why IPSec is Complex
```
Must Configure:
- IKE version (IKEv1 or IKEv2)
- Authentication method (pre-shared key, certificates)
- Encryption algorithms (AES-128, AES-256)
- Hash algorithms (SHA-256, SHA-512)
- DH groups (for key exchange)
- Perfect Forward Secrecy
- Dead Peer Detection
- NAT Traversal

Each side must match perfectly
```

| ✅ Advantages | ❌ Disadvantages |
|--------------|-----------------|
| Industry standard (interoperable) | Complex configuration |
| Built into most OSes (no client needed) | Difficult to troubleshoot |
| Strong encryption | NAT traversal issues |
| Widely supported in enterprise | Firewall unfriendly (multiple ports/protocols) |

## 4.4 🔑 SSL/TLS VPN

**How it works**:
```
Uses HTTPS (port 443) for VPN tunnel
- Runs in web browser or thin client
- Leverages SSL/TLS for encryption
- Same technology as secure websites
```

**Two Modes**:

**1. Clientless (Portal)**:
```
User → Web Browser → HTTPS → VPN Gateway → Internal Resources

Access via browser:
- https://vpn.company.com/internal-app
- No client software needed
- Limited to web-based applications
```

**2. Client-Based (Tunnel)**:
```
User → VPN Client → SSL/TLS Tunnel → VPN Gateway → Internal Resources

Full network access:
- All applications work
- Requires client software
- Like IPSec, but over SSL/TLS
```

| ✅ Advantages | ❌ Disadvantages |
|--------------|-----------------|
| Works through any firewall (uses port 443) | Slower than IPSec/WireGuard |
| No special client for portal mode | Limited in portal mode |
| Easy to use | Less efficient encapsulation |
| Granular access control | |

## 4.5 📊 Protocol Comparison

| Feature | 🟠 OpenVPN | 🐉 WireGuard | 🛡️ IPSec | 🔑 SSL VPN |
|---------|---------|-----------|-------|---------|
| Speed | Medium | Fastest | Fast | Slowest |
| Security | Excellent | Excellent | Excellent | Good |
| Ease of Setup | Medium | Easy | Difficult | Easy |
| Platform Support | All | Most | All | All |
| Mobile Performance | Medium | Excellent | Good | Medium |
| Code Complexity | High | Very Low | Very High | High |
| Firewall Friendly | Yes | Yes | No | Yes |

## 4.6 🎯 When to Use Each

**🟠 OpenVPN**:
- Need cross-platform compatibility
- Want proven, mature solution
- Require flexible configuration

**🐉 WireGuard**:
- Need maximum performance
- Modern infrastructure
- Mobile users
- Simple setup preferred

**🛡️ IPSec**:
- Enterprise site-to-site VPN
- Vendor equipment requirement
- Compliance requirements
- Already have IPSec infrastructure

**🔑 SSL VPN**:
- Remote access through restrictive firewalls
- Browser-based access needed
- Quick deployment
- Non-technical users

---

# 5. 🚇 Tunneling Concepts

Tunneling is the process of encapsulating one network protocol inside another. VPNs use tunneling, but tunneling has many other uses.

## 5.1 ❓ What is a Tunnel?

**The Concept**:
```
You want to send Protocol A through Network B
Network B doesn't support Protocol A
Solution: Wrap Protocol A packets inside Protocol B packets

Like: Mailing a letter (Protocol A) inside a package (Protocol B)
```

**Packet in Packet**:
```
Original Packet (Inner):
[IP: 10.0.1.5 → 10.0.2.10][TCP][Data]

Tunneled Packet (Outer):
[IP: 203.0.113.5 → 198.51.100.10][Tunnel Protocol][Encrypted:[Inner Packet]]
        ↑                                   ↑                ↑
   Public IPs                        Tunnel Header    Original packet
```

## 5.2 🤔 Why Tunneling?

**1. 🔒 Security**:
```
Encrypt private traffic over public network
Examples: VPNs, SSH tunnels
```

**2. 🔌 Protocol Compatibility**:
```
Send IPv6 over IPv4 network
Send non-IP protocols over IP network
Examples: 6to4, GRE
```

**3. 🌍 Network Extension**:
```
Connect remote networks as if local
Examples: Site-to-site VPN, VXLAN
```

**4. 🚧 Bypass Restrictions**:
```
Access blocked services
Hide traffic type from ISP
Examples: SSH tunnels, VPNs
```

## 5.3 ⚖️ Tunneling vs Encryption

**Important Distinction**:
```
Tunneling ≠ Encryption

Tunneling: Packet encapsulation (may or may not be encrypted)
Encryption: Making data unreadable

GRE Tunnel: Tunneling without encryption
VPN: Tunneling WITH encryption
```

## 5.4 🧵 Common Tunneling Protocols

### 5.4.1 GRE (Generic Routing Encapsulation)

**Purpose**: Simple tunneling protocol, wraps any protocol inside IP

**Packet Structure**:
```
[IP Header: GW1 → GW2][GRE Header][Inner Packet]
```

**Characteristics**:
- No encryption (plaintext)
- Stateless (no session tracking)
- Low overhead
- Often combined with IPSec for security

**Use Case**:
```
Branch Office Network (10.0.0.0/24)
        ↓
    GRE Tunnel (over internet)
        ↓
HQ Network (192.168.0.0/16)

Networks connected as if directly connected
```

### 5.4.2 VXLAN (Virtual Extensible LAN)

**Purpose**: Create virtual Layer 2 networks over Layer 3 infrastructure

**The Problem VXLAN Solves**:
```
Traditional VLANs:
- Limited to 4096 VLANs (12-bit VLAN ID)
- Can't span multiple datacenters
- Tied to physical infrastructure

VXLAN:
- 16 million virtual networks (24-bit VNI)
- Works over IP networks (across datacenters)
- Overlay network (decoupled from physical)
```

**How VXLAN Works**:
```
VM1 (VXLAN 100) on Host A wants to talk to VM2 (VXLAN 100) on Host B

1. VM1 sends Ethernet frame
2. Host A encapsulates in VXLAN:
   [UDP][VXLAN Header: VNI=100][Original Ethernet Frame]
3. Sent to Host B over IP network
4. Host B decapsulates and delivers to VM2

Result: VM1 and VM2 think they're on same L2 network
```

**Use Cases**:
- Multi-tenant cloud environments
- Container networking (Docker, Kubernetes)
- Datacenter network virtualization
- Connecting VMs across locations

### 5.4.3 IP-in-IP

**Purpose**: Encapsulate IP packet in another IP packet

**Structure**:
```
[Outer IP Header][Inner IP Header][TCP][Data]
     ↑                  ↑
  For routing      Original packet
```

**Use Case**:
```
Mobile IP: Device keeps same IP when moving networks
IPv6 over IPv4: Tunnel IPv6 through IPv4 network
```

## 5.5 🕸️ Tunnel Modes

**Point-to-Point Tunnel**:
```
Endpoint A ←-------- Tunnel --------→ Endpoint B

One-to-one connection
Examples: SSH tunnel, PPP, L2TP
```

**Hub-and-Spoke Tunnel**:
```
       Branch 1
          ↓
       Tunnel
          ↓
Hub (HQ) ←-- Tunnel --- Branch 2
          ↓
       Tunnel
          ↓
       Branch 3

Multiple branches connect to central hub
```

**Mesh Tunnel**:
```
Office A ←--→ Office B
   ↕             ↕
Office C ←--→ Office D

Every office connected to every other
Full redundancy, but complex to manage
```

## 5.6 ✂️ Split Tunneling

**Full Tunnel** (All traffic through VPN):
```
Your Computer → VPN → Corporate Network
             → VPN → Internet

All traffic protected, but:
- Slower (everything routes through VPN)
- Corporate network sees all traffic
- Corporate bandwidth used for personal traffic
```

**Split Tunnel** (Only corporate traffic through VPN):
```
Your Computer → VPN → Corporate Network (10.0.0.0/8)
             → Direct → Internet (everything else)

Faster, but:
- Less secure (split security boundary)
- Corporate network can't see/control all traffic
- Potential for data leakage
```

**Configuration**:
```
Full Tunnel:
Route: 0.0.0.0/0 via VPN (all traffic)

Split Tunnel:
Route: 10.0.0.0/8 via VPN (only corporate)
Route: 0.0.0.0/0 via local gateway (everything else)
```

**When to Use Each**:

**Full Tunnel**:
- Maximum security required
- Compliance requirements
- Want to monitor all traffic
- Accessing sensitive data

**Split Tunnel**:
- Better performance needed
- High bandwidth apps (streaming, gaming)
- Trust user network
- VPN capacity limited

## 5.7 📏 Tunnel Overhead

Every tunnel adds overhead:

**Packet Size Calculation**:
```
Original Packet: 1500 bytes (Ethernet MTU)
- IP Header: 20 bytes
- TCP Header: 20 bytes
- Data: 1460 bytes

After Tunneling (VPN):
- Outer IP Header: 20 bytes
- VPN Header: 20-50 bytes (varies by protocol)
- Encrypted Original Packet: 1500 bytes
Total: 1540-1570 bytes

Problem: Exceeds Ethernet MTU!
```

**MTU Issues**:
```
Without proper configuration:
1. Large packet created (1570 bytes)
2. Exceeds network MTU (1500 bytes)
3. Packet fragmented
4. Fragments may be dropped
5. Connection fails or is very slow

Solution:
- Reduce MTU on tunnel interface (1400 bytes)
- Enable Path MTU Discovery
- Configure MSS clamping
```

**Performance Impact**:
```
Overhead per packet:
- No tunnel: 40 bytes (IP+TCP)
- GRE: 64 bytes (IP+GRE+IP+TCP)
- IPSec: 70-100 bytes
- OpenVPN: 80-120 bytes

For small packets, overhead can be 50%+ of packet size
```

---

# 6. 🌎 Real-World Examples

## 6.1 🏡 Example 1: Remote Work Setup
```
Employee at Home
       ↓
  VPN Client (OpenVPN)
       ↓
    Internet
       ↓
  Company VPN Server
       ↓
Access: File servers, databases, internal apps
```

## 6.2 ☁️ Example 2: AWS Site-to-Site VPN
```
On-Premise Data Center (10.0.0.0/16)
       ↓
  Customer Gateway
       ↓
   IPSec Tunnel
       ↓
  AWS Virtual Private Gateway
       ↓
AWS VPC (172.16.0.0/16)
```

## 6.3 🏰 Example 3: Bastion Host with SSH Tunnel
```
Developer
       ↓
SSH Tunnel to Bastion (public IP)
       ↓
Access Database (private IP, no public access)
```

---

# 7. 🛠️ Setting Up a Simple VPN

## 7.1 🐉 WireGuard (Modern & Simple)

### 7.1.1 🖥️ Server Setup
```bash
# Install WireGuard
sudo apt install wireguard

# Generate keys
wg genkey | tee privatekey | wg pubkey > publickey

# Configure (/etc/wireguard/wg0.conf)
[Interface]
Address = 10.0.0.1/24
PrivateKey = <server-private-key>
ListenPort = 51820

[Peer]
PublicKey = <client-public-key>
AllowedIPs = 10.0.0.2/32

# Start VPN
sudo wg-quick up wg0

# Enable on boot
sudo systemctl enable wg-quick@wg0
```

### 7.1.2 💻 Client Setup
```bash
# Configure (/etc/wireguard/wg0.conf)
[Interface]
Address = 10.0.0.2/24
PrivateKey = <client-private-key>

[Peer]
PublicKey = <server-public-key>
Endpoint = vpn-server.example.com:51820
AllowedIPs = 0.0.0.0/0  # Route all traffic through VPN
PersistentKeepalive = 25

# Connect
sudo wg-quick up wg0
```

---

# 8. 🔌 SSH Tunneling (Port Forwarding)

## 8.1 ⬅️ Local Port Forwarding
Access remote service through local port.

```bash
# Forward local port 8080 to remote server's port 80
ssh -L 8080:localhost:80 user@remote-server

# Now access: http://localhost:8080
```

**Use case**: Access web interface on remote server.

## 8.2 ➡️ Remote Port Forwarding
Expose local service to remote server.

```bash
# Expose local port 3000 on remote server's port 8080
ssh -R 8080:localhost:3000 user@remote-server

# Remote users can access: http://remote-server:8080
```

**Use case**: Demo local development to others.

## 8.3 🧦 Dynamic Port Forwarding (SOCKS Proxy)
```bash
# Create SOCKS proxy
ssh -D 8080 user@remote-server

# Configure browser to use SOCKS proxy: localhost:8080
```

**Use case**: Route all browser traffic through remote server.

---

# 9. ☁️ Cloud VPN Examples

## 9.1 🟧 AWS VPN
```
Components:
- Virtual Private Gateway (VPC side)
- Customer Gateway (your side)
- VPN Connection (IPSec tunnel)

Configuration:
- Pre-shared key for authentication
- BGP or static routing
- Redundant tunnels for high availability
```

## 9.2 🟦 Azure VPN Gateway
```
Components:
- VPN Gateway in Azure VNet
- Local Network Gateway (your on-premise)
- VPN Connection

Types:
- Point-to-Site (client VPN)
- Site-to-Site (network VPN)
```

---

# 10. 🥊 VPN vs Proxy vs Tor

| Feature | 🔐 VPN | 🔀 Proxy | 🧅 Tor |
|---------|-----|-------|-----|
| Encryption | Yes | Depends | Yes |
| Speed | Fast | Fastest | Slow |
| Privacy | Good | Limited | Best |
| Cost | Paid/Free | Free | Free |
| Use Case | General privacy | Bypass filters | Anonymity |

---

# 11. 💻 Common Commands

## 11.1 🔍 Check VPN connection
```bash
# List network interfaces
ip addr show
# Look for wg0, tun0, or similar VPN interface

# Check routing
ip route show

# Test if traffic is going through VPN
curl ifconfig.me  # Shows your public IP
```

## 11.2 🐉 WireGuard commands
```bash
# Start VPN
sudo wg-quick up wg0

# Stop VPN
sudo wg-quick down wg0

# Check status
sudo wg show

# Check interface
ip addr show wg0
```

## 11.3 🟠 OpenVPN commands
```bash
# Connect to VPN
sudo openvpn --config client.ovpn

# Run in background
sudo systemctl start openvpn@client

# Check status
sudo systemctl status openvpn@client
```

---

# 12. ✂️ Split Tunneling (Quick Reference)

Send some traffic through VPN, some directly.

```
Banking App → VPN → Company Network
Netflix → Direct → Internet (faster streaming)
```

**Configuration**:
```bash
# Only route specific network through VPN
AllowedIPs = 10.0.0.0/8, 172.16.0.0/12

# Not:
AllowedIPs = 0.0.0.0/0  # Routes everything
```

---

# 13. 🩺 Troubleshooting

## 13.1 🚫 Issue: Can't connect to VPN
```bash
# Check if VPN server is reachable
ping vpn-server.example.com

# Check if port is open
nc -zv vpn-server.example.com 51820

# Check firewall
sudo ufw allow 51820/udp
```

## 13.2 🌐 Issue: Connected but no internet
```bash
# Check DNS
cat /etc/resolv.conf

# Check routing
ip route show

# Ensure IP forwarding is enabled (server)
sudo sysctl -w net.ipv4.ip_forward=1
```

## 13.3 🐢 Issue: Slow VPN speed
- Try UDP instead of TCP (if available)
- Choose closer VPN server
- Check for ISP throttling
- Use WireGuard instead of OpenVPN (faster)

---

# 14. 🚀 DevOps Use Cases

## 14.1 🔒 Secure Access to Private Resources
```
Developer → VPN → Private Database (no public IP)
```

## 14.2 🌉 Hybrid Cloud Connectivity
```
On-Premise Servers → VPN → Cloud Resources
Enables seamless hybrid deployments
```

## 14.3 🗺️ Multi-Region Networking
```
US Datacenter → VPN → EU Datacenter → VPN → Asia Datacenter
```

## 14.4 🔁 Secure CI/CD Access
```
CI/CD Server → VPN → Production Environment
Deploy safely without exposing production
```

---

# 15. ✅ Key Takeaways

| | Takeaway |
|:-:|----------|
| ✅ | VPNs create encrypted tunnels over public networks |
| ✅ | Remote access VPN for individuals, site-to-site for networks |
| ✅ | WireGuard is modern and fast, OpenVPN is mature and flexible |
| ✅ | SSH tunneling is simple for quick port forwarding |
| ✅ | Cloud VPNs connect on-premise to cloud (hybrid cloud) |
| ✅ | Split tunneling sends only some traffic through VPN |
| ✅ | Always use VPN on public WiFi for security |
| ✅ | Check firewall and routing when troubleshooting VPN issues |

---

<div align="center">

### 🔐 𝘚𝘵𝘢𝘺 𝘚𝘦𝘤𝘶𝘳𝘦 • 𝘚𝘵𝘢𝘺 𝘗𝘳𝘪𝘷𝘢𝘵𝘦 • 𝘚𝘵𝘢𝘺 𝘊𝘰𝘯𝘯𝘦𝘤𝘵𝘦𝘥



[⬆ Back to Top](#-𝗩𝗣𝗡--𝗧𝘂𝗻𝗻𝗲𝗹𝗶𝗻𝗴)

</div>
