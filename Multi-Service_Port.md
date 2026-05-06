# Multi-service Port

**Priority:** P1  
**Feature group:** L2 Ethernet, L3 Services

## Overview

Configuring **multi-service** ports on Cisco 8000 Series routers involves defining physical interfaces, setting up Layer 2 or Layer 3 characteristics, and creating subinterfaces for specific services (like **L2VPN EFPs or L3 VRFs)**. The Cisco 8000 series uses IOS XR software, which supports flexible VLAN tagging (802.1Q) and encapsulation for diverse traffic types on a single port.

## Configuration source (Cisco 8000, IOS XR 26.x)

Cisco IOS XR **group** / **apply-group** (see [Cisco 8000 configuration guides](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/Interfaces/26xx/configuration/guide/b-interfaces-config-guide-cisco8k-r26xx/configuring-ethernet-interfaces.html)

## Sample IOS XR configuration

```text
! Multi-Service port + Jumbo frames
interface HundredGigE0/3/0/1
 mtu 9214
!
interface HundredGigE0/3/0/1.10
 vrf A
 ipv4 address 10.0.0.1 255.255.255.0
 encapsulation dot1q 10
!
interface HundredGigE0/3/0/1.110 l2transport
 encapsulation dot1q 110 second-dot1q 3110
 rewrite ingress tag pop 2 symmetric
!
```

> **Note:** Examples are illustrative for Cisco IOS XR on Cisco 8000-class systems. Validate syntax, scale limits, and feature availability for your exact release (K100/P100) and interface types.
