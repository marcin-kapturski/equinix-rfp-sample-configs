# BFD

**Priority:** P2  
**Feature group:** EIA, L3 Services

## Overview

**BFD** provides fast failure detection for **BGP**, **IS-IS**, and other clients. Enable **IS-IS BFD** under **`router isis`** on each interface (for example **Bundle-Ether**) with **`bfd fast-detect ipv4`** (or **ipv6**). For **L3 Bundle-Ether** (or other interfaces), you can also set **`bfd address-family ipv4`** **minimum-interval** / **multiplier** / **fast-detect** on the interface; **LAG** may use **micro-BFD** on member links where supported—validate for your release and platform.

## Configuration source (Cisco 8000, IOS XR 26.x)

[BGP Configuration Guide](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/bgp/b-bgp-config-cisco8000.html) (BFD under BGP neighbor); [Routing Configuration Guide](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/routing/26xx/configuration/guide/b-routing-cg-cisco8000-26xx.html) (IS-IS BFD); [Interface and Hardware Configuration Guide](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/Interfaces/26xx/configuration/guide/b-interfaces-config-guide-cisco8k-r26xx.html) (BFD on Bundle-Ether and interfaces).

## Sample IOS XR configuration

```text
! IPv4 Multihop BFD
bfd
 multihop ttl-drop-threshold 225
 multipath include location 0/X
router bgp 100
 neighbor 209.165.200.225
 remote-as 2000
 update-source loopback 1
 bfd fast-detect
 bfd multiplier 3
 bfd minimum-interval 1200
!
! BFD with IS-IS
router isis 65444
 interface HundredGige 0/3/0/1
  bfd minimum-interval 1200
  bfd multiplier 7
  bfd fast-detect ipv4
 !
! BFD over Bundle(BOB) forwith hardware offload (Micro-BFD) 
interface Bundle-Ether10
 bfd mode ietf
 bfd address-family ipv4 fast-detect
 bfd address-family ipv4 destination X.X.X.X
 bfd address-family ipv4 minimum-interval 50
 bfd address-family ipv4 multiplier 3
!

```

> **Note:** Examples are illustrative for Cisco IOS XR on Cisco 8000-class systems. Validate syntax, scale limits, and feature availability for your exact release (K100/P100) and interface types.
