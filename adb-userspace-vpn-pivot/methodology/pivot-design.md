# Pivot Design

## Strategic Reframing

The original problem focused on interface sharing:

> How do we make Android share the VPN connection?

After architectural analysis, this was reframed as:

> How do we reach a service on the Android device that already has VPN-routed connectivity?

Instead of modifying the routing stack, the design pivots to application-layer transport.

---

## Design Principles

The solution had to satisfy the following constraints:

- No root access
- No kernel routing changes
- No NAT or iptables manipulation
- Fully reversible
- DNS-safe
- Minimal attack surface exposure

This ruled out:

- Custom ROM solutions
- iptables-based forwarding
- TUN interface injection
- System-wide proxy hacks

The pivot had to operate above the routing layer.

---

## ADB Port Forwarding as Transport

ADB provides a mechanism for forwarding TCP ports over USB:

```bash
adb forward tcp:<host_port> tcp:<device_port>
````

Characteristics:

* Creates a direct TCP tunnel between host and device
* Independent of Android’s network stack
* Does not rely on tethering
* Requires only USB debugging authorization

This allows a service listening on `127.0.0.1` on the device to be accessed from the host.

Critically:

ADB transport is not tied to the VPN interface.
It is a direct device communication channel.

This makes it ideal for bypassing routing-layer restrictions.

---

## SOCKS5 Proxy as Application-Layer Gateway

A SOCKS proxy was selected as the application-layer pivot point because:

* It supports arbitrary TCP connections
* It allows hostname-based resolution
* It is simple and widely supported
* It avoids modifying system routing tables

The proxy runs locally on the Android device and:

* Receives traffic from the host (via ADB forward)
* Forwards traffic using the device’s active VPN connection
* Returns responses through the same tunnel

---

## Why SOCKS5h (Hostname Resolution Inside Tunnel)

Two proxy modes exist:

* SOCKS5 — DNS resolved on host
* SOCKS5h — DNS resolved by proxy

SOCKS5h was intentionally selected.

Reason:

If DNS is resolved on the host:

* DNS queries bypass the VPN tunnel
* Potential information leakage occurs
* Routing behavior becomes inconsistent

With SOCKS5h:

* DNS queries are forwarded to the proxy
* DNS resolution occurs inside the VPN tunnel
* Traffic remains fully encapsulated
* No host-level DNS leakage

This ensures tunnel integrity.

---

## Final Data Flow

The final architecture:

```
Linux Host
    ↓
SOCKS5h (localhost:1081)
    ↓
ADB Forward (USB TCP tunnel)
    ↓
Android Local Proxy
    ↓
tun2socks (userspace VPN)
    ↓
Encrypted VPN Tunnel
    ↓
Internet
```

Each layer operates independently:

* No kernel modifications
* No NAT dependency
* No interface sharing
* No persistent changes

---

## Design Advantages

* Works with any userspace VPN implementation
* Compatible with modern Android versions
* Fully reversible
* Minimal system footprint
* Maintains DNS containment
* Operates entirely in user-controlled layers

---

## Design Limitations

* Requires USB debugging enabled
* Dependent on ADB trust relationship
* Not optimized for high throughput
* Limited to TCP-based proxy routing

---

## Summary

The pivot design avoids fighting Android’s routing architecture.

Instead, it:

* Leverages ADB as a transport channel
* Uses SOCKS as an application-layer gateway
* Preserves DNS integrity with SOCKS5h
* Maintains clean separation of concerns

This transforms a routing-layer problem into an application-layer solution.
