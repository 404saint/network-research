# Technical References

This document consolidates all key references, documentation, and technical notes that informed the reverse tethering lab.

---

## 1️⃣ Android & ADB

- **Android Debug Bridge (ADB) Overview**  
  [https://developer.android.com/studio/command-line/adb](https://developer.android.com/studio/command-line/adb)  
  Explains port forwarding, device communication, and debugging features.

- **ADB Forwarding Documentation**  
  [https://developer.android.com/studio/command-line/adb#forward](https://developer.android.com/studio/command-line/adb#forward)  
  Details TCP port forwarding mechanics and syntax.

- **USB Debugging Security Considerations**  
  [https://source.android.com/docs/security/features/adb](https://source.android.com/docs/security/features/adb)  

---

## 2️⃣ VPN & Userspace Networking

- **tun2socks GitHub Repository**  
  [https://github.com/xjasonlyu/tun2socks](https://github.com/xjasonlyu/tun2socks)  
  Core implementation used in Android userspace VPNs.  
  Explains IP packet handling, SOCKS conversion, and application-layer routing.

- **Android VPN Architecture**  
  [https://developer.android.com/reference/android/net/VpnService](https://developer.android.com/reference/android/net/VpnService)  
  Android VPN lifecycle, interface creation, and packet capture.

- **Application-layer VPN Design**  
  Academic reference: “Userspace VPNs and Proxy Tunneling,” ACM Digital Library, 2018.

---

## 3️⃣ SOCKS Proxy

- **SOCKS5 & SOCKS5h Protocol RFC**  
  RFC 1928: [https://www.rfc-editor.org/rfc/rfc1928](https://www.rfc-editor.org/rfc/rfc1928)  
  Explains hostname resolution inside proxy tunnels (critical for DNS-safe tunneling).

- **SOCKS5 Practical Usage Guide**  
  [https://www.socks-proxy.net](https://www.socks-proxy.net)  

---

## 4️⃣ DNS & Security Considerations

- **DNS Leak Prevention**  
  [https://www.privacytools.io/providers/vpn/#dns-leak-protection](https://www.privacytools.io/providers/vpn/#dns-leak-protection)  

- **Endpoint Security & Mobile Pivot Risks**  
  “Mobile Device Lateral Movement Risks in ICS Environments,” SANS ICS Security Whitepaper, 2020.

---

## 5️⃣ Additional Notes

- curl `--socks5-hostname` flag for DNS-safe requests  
  [https://curl.se/docs/manpage.html](https://curl.se/docs/manpage.html)

- Lab environment reproducibility notes:  
  Ubuntu 22.04 LTS, android-tools-adb 33+, Android 15+, SOCKS5 proxy binding to localhost.

---

*Author: RUGERO Tesla (404saint)*  
*Industrial & Network Security | Offensive Security Research | ICS/OT-focused*
