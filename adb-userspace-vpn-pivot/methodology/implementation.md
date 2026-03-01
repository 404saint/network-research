# Implementation

## Overview

This section documents the exact steps required to reproduce the reverse tethering experiment using:

- ADB port forwarding
- A local SOCKS5 proxy on Android
- A userspace VPN (tun2socks-based)

All steps were executed in a controlled lab environment.

---

## Step 1 — Enable USB Debugging (Android)

1. Enable **Developer Options**
2. Enable **USB Debugging**
3. Connect device via USB
4. Authorize the host computer when prompted

Verify connection on the Linux host:

```bash
adb devices
````

Expected output:

```
<device_serial>    device
```

If no device appears:

* Check USB cable
* Verify USB mode (File Transfer / MTP)
* Reconnect and reauthorize

---

## Step 2 — Install ADB on Linux Host

```bash
sudo apt update
sudo apt install android-tools-adb
```

Confirm ADB version:

```bash
adb version
```

---

## Step 3 — Install and Configure Local SOCKS Proxy (Android)

Install a SOCKS proxy application on the Android device.

Configuration:

* **Protocol:** SOCKS5
* **Bind Address:** 127.0.0.1
* **Port:** 1081
* **Authentication:** Disabled (lab environment only)

Important:

The proxy must bind only to `localhost`.
Do NOT bind to `0.0.0.0`.

Start the proxy service.

---

## Step 4 — Activate VPN

Ensure the userspace VPN is active on the Android device.

Confirm:

* Internet works on the device
* VPN tunnel is connected

No system-level routing changes are required.

---

## Step 5 — Forward TCP Port via ADB

On the Linux host:

```bash
adb forward tcp:1081 tcp:1081
```

This creates:

```
Host 127.0.0.1:1081 → Android 127.0.0.1:1081
```

Verify active forwards:

```bash
adb forward --list
```

Expected:

```
tcp:1081 tcp:1081
```

---

## Step 6 — Test Connectivity via curl

Use SOCKS5h to ensure DNS resolution occurs inside the tunnel:

```bash
curl --socks5-hostname 127.0.0.1:1081 https://example.com
```

Expected outcome:

* HTML response returned
* No local DNS resolution errors

If this fails:

* Verify proxy is running
* Confirm VPN is active
* Check ADB forward is active

---

## Step 7 — Browser Configuration (Optional)

Launch Chromium or Chrome with proxy flag:

```bash
google-chrome --proxy-server="socks5h://127.0.0.1:1081"
```

This ensures:

* All browser traffic routes through proxy
* DNS resolution occurs inside VPN tunnel
* No system-wide proxy change is required

---

## Step 8 — DNS Leak Verification

Validate DNS containment using online DNS test services.

Expected:

* DNS servers match VPN provider
* No ISP-level DNS detected
* No host-based DNS resolution leakage

---

## Cleanup / Rollback

Remove port forward:

```bash
adb forward --remove tcp:1081
```

Then:

* Disable USB debugging
* Stop proxy application
* Disconnect USB

No persistent configuration remains on the host or device.

---

## Troubleshooting

**ADB not detecting device**

* Restart ADB:

  ```bash
  adb kill-server
  adb start-server
  ```

**Connection timeout**

* Ensure proxy is running
* Confirm correct port number
* Verify VPN connectivity on device

**DNS leaks**

* Confirm use of `--socks5-hostname`
* Ensure browser is using SOCKS5h, not plain SOCKS5
