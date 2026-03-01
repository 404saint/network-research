
## 🔬 Technical Breakdown

### 1️⃣ Traffic Flow Architecture

When the userspace VPN is active on Android, traffic follows this path:

PC Application  
→ Windows Network Stack  
→ ADB Port Forward  
→ Android Userspace VPN (TUN Interface)  
→ tun2socks Translation Layer  
→ SOCKS/Proxy Endpoint  
→ Mobile Data Network  
→ Internet

The PC does not have direct internet access.  
Instead, it tunnels traffic through the Android device’s VPN interface.

---

### 2️⃣ Why This Works

Android VPN applications create a virtual TUN interface.  

A TUN interface:
- Operates at Layer 3 (IP layer)
- Captures outbound IP packets
- Routes them through a userspace process

tun2socks converts raw IP packets into SOCKS proxy traffic, allowing:
- TCP session encapsulation
- Userspace routing control
- Forwarding without root privileges

ADB is used as the transport bridge between:
- Host (Windows)
- Android userspace network stack

The result is a controlled pivot without modifying kernel routing tables.

---

### 3️⃣ Key Observations

• The VPN only works while active on Android  
• If the VPN disconnects, PC internet access immediately drops  
• DNS behavior depends on the VPN configuration  
• This setup bypasses traditional NAT sharing methods  
• No root or system modification required  

---

### 4️⃣ Architectural Insight

This setup demonstrates:

- Application-layer pivoting
- Userspace packet interception
- Layered abstraction in mobile networking
- Practical constrained-environment routing

This is not a "free internet hack."  
It is a routing and tunneling experiment demonstrating how userspace VPNs manage traffic.

---

### 5️⃣ Security Perspective

From a defensive standpoint:

- Userspace VPN traffic can obscure true endpoints
- Monitoring must account for mobile-bridged connections
- Endpoint security solutions may not detect layered tunneling via ADB

Understanding this architecture helps in:

- Detecting unusual pivot paths
- Investigating endpoint-to-mobile traffic bridges
- Modeling lateral movement in hybrid environments
