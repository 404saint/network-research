# Problem Analysis

## Research Question

How can a Linux host route traffic through an Android device running a userspace VPN implementation when traditional tethering mechanisms fail?

The objective was to understand the architectural limitation and identify an alternative transport strategy without requiring:

- Root access
- Kernel modifications
- Custom ROMs
- Permanent configuration changes

---

## Observed Behavior

The Android device had active internet connectivity through a VPN application.

However:

- USB tethering failed.
- Wi-Fi hotspot sharing failed.
- Reverse Wi-Fi repeater methods failed.
- Tethering-based applications failed.

Despite working connectivity on the device itself, no sharable network interface was exposed to the host system.

---

## Architectural Root Cause

Modern Android VPN applications commonly use a **userspace VPN model** based on `tun2socks`.

### tun2socks Characteristics

- Operates in userspace
- Creates a virtual TUN interface internally
- Does not expose a traditional kernel-level `tun0` interface
- Bypasses Android’s NAT and tethering stack
- Handles packet encapsulation and routing inside the application process

This architecture differs from traditional VPN implementations that:

- Register kernel-level interfaces
- Participate in Android’s routing table
- Allow NAT forwarding for tethering

Because tun2socks operates entirely in userspace:

- Android’s tethering subsystem cannot detect a sharable upstream interface.
- NAT rules are not applied.
- Traffic remains confined to the VPN application's internal routing logic.

---

## Why Traditional Tethering Fails

Android tethering depends on:

- A routable upstream interface
- Kernel-level packet forwarding
- NAT (iptables-based)
- System routing table participation

In this environment:

- No routable VPN interface exists at kernel level.
- The VPN application intercepts and processes packets internally.
- The tethering stack has no visibility into the VPN transport.

Result:

The phone has internet access.
The host does not.

---

## Constraint Analysis

The lab environment intentionally enforced:

- No root privileges
- No kernel manipulation
- No iptables changes
- No system proxy enforcement
- No modification of Android framework components

This required an alternative method that:

- Does not rely on Android routing stack
- Does not require interface sharing
- Operates at the application layer
- Remains fully reversible

---

## Pivot Opportunity Identified

Observation:

While Android's tethering stack was inaccessible, **ADB port forwarding remains available through USB debugging**.

ADB port forwarding:

- Creates a TCP tunnel between host and device
- Does not depend on network routing
- Operates independently of tethering
- Can expose device-local services to the host

This shifted the problem from:

"How do we share the VPN interface?"

To:

"How do we reach a service on the device that already has VPN access?"

That reframing enabled an application-layer pivot instead of fighting the routing stack.

---

## Conclusion

The failure was not due to network connectivity.

It was due to architectural separation between:

- Android’s tethering subsystem (kernel-level)
- Userspace VPN implementation (application-level)

By recognizing this separation, the solution path became clear:

Avoid routing-layer modification.
Leverage application-layer transport instead.
