# Lab Environment

## Objective

Document the controlled environment used to evaluate reverse tethering through a userspace VPN using ADB port forwarding and a SOCKS5h proxy.

This setup intentionally avoids:

- Root access
- Kernel-level routing changes
- System-wide proxy enforcement
- Permanent configuration modifications

The experiment is fully reversible.

---

## Host System (Client)

- **OS:** Ubuntu 22.04 LTS (or equivalent Debian-based distribution)
- **ADB Version:** android-tools-adb (installed via package manager)
- **Browser Used for Testing:** Chromium / Google Chrome
- **Testing Tools:** curl, DNS leak test platforms

### Installed Packages

```bash
sudo apt update
sudo apt install android-tools-adb curl
````

---

## Android Device

* **Android Version:** 15+
* **Root Access:** Disabled
* **Developer Options:** Enabled
* **USB Debugging:** Enabled (trusted host authorized)

### VPN Characteristics

The VPN application used in this lab:

* Operates using a **userspace tun2socks implementation**
* Does NOT expose a kernel-level `tun0` interface
* Bypasses Android's native tethering stack
* Routes traffic internally via application-layer handling

Because of this architecture:

* USB tethering fails
* Wi-Fi hotspot sharing fails
* NAT-based sharing is unavailable

---

## Proxy Application

A local SOCKS proxy was installed on the Android device with:

* **SOCKS Version:** SOCKS5
* **Bind Address:** 127.0.0.1
* **Authentication:** Disabled (lab environment)
* **Port:** 1081 (non-conflicting port)

Important:

The proxy must bind only to `localhost` to prevent unintended network exposure.

---

## Connection Method

### Step 1 – Verify ADB Connectivity

```bash
adb devices
```

Expected output:

```
<device_serial>    device
```

### Step 2 – Forward TCP Port

```bash
adb forward tcp:1081 tcp:1081
```

This maps:

```
Host 127.0.0.1:1081 → Android 127.0.0.1:1081
```

---

## Verification

### Proxy Test via curl

```bash
curl --socks5-hostname 127.0.0.1:1081 https://example.com
```

Expected:

* Valid HTML response
* No DNS resolution via local ISP
* Successful traffic flow through VPN tunnel

### DNS Leak Testing

Confirmed:

* DNS resolution occurs inside VPN tunnel
* No host-based DNS leakage detected

---

## Environmental Constraints Simulated

This lab models scenarios such as:

* Constrained enterprise mobile devices
* Locked-down environments without root
* Segmented infrastructure with restricted routing
* Systems where traditional NAT or tethering is unavailable

---

## Reversibility

To restore system state:

* Remove ADB port forward:

  ```bash
  adb forward --remove tcp:1081
  ```
* Disable USB debugging
* Stop proxy application

No persistent system changes are introduced.

---

## Summary

This lab environment was intentionally designed to:

* Emphasize application-layer pivoting
* Demonstrate constrained routing adaptation
* Avoid invasive or persistent modifications
* Maintain clean rollback capability
