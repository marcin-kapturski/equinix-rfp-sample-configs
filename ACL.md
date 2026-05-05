# ACL

**Priority:** P1  
**Feature group:** Forwarding

## Overview

**Access lists** on Cisco 8000 support **L2 (MAC)**, **IPv4/IPv6**, and **L4** matches for interface ingress/egress and for control-plane attachment via **IPv4/IPv6 access-list** and **object-group** where supported.

## Configuration source (Cisco 8000, IOS XR 26.x)

[System Security Configuration Guide](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/security/26xx/configuration/guide/b-system-security-cg-cisco8000-26xx.html); [Interface and Hardware Configuration Guide](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/Interfaces/26xx/configuration/guide/b-interfaces-config-guide-cisco8k-r26xx.html) (IPv4 access-group).

## Sample IOS XR configuration

```text
ipv4 access-list ACL-L3-ACCESS-IN
 10 permit tcp 198.51.100.0 0.0.0.255 any eq bgp
 20 deny ipv4 any any
!
ethernet-services access-list ACL-L2-ACCESS-IN
 10 permit host 0011.2233.4455 any
 20 deny any any
 !
interface HundredGigE0/0/0/0
 ipv4 access-group ACL-L3-ACCESS-IN ingress
!
interface HundredGigE0/0/0/1
  l2transport
  ethernet-services access-group ACL-L2-ACCESS-IN ingress
```

> **Note:** Examples are illustrative for Cisco IOS XR on Cisco 8000-class systems. Validate syntax, scale limits, and feature availability for your exact release (K100/P100) and interface types.
