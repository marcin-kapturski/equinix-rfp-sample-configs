# ISIS SR (LER, LSR)

**Priority:** P1  
**Feature group:** Control Plane

## Overview

**IS-IS Segment Routing** follows the *Configure Segment Routing for IS-IS Protocol* procedure: **metric-style wide**, **router-id** on the loopback used for the prefix-SID, **segment-routing mpls** on the IPv4 unicast address family, and **prefix-sid index** (or **absolute**) on the IS-IS **Loopback** interface. Pair with **SRGB**/TI-LFA per design.

**Configuration reference:** Use the Cisco 8000 Series [Segment Routing Configuration Guide](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/segment-routing/26xx/configuration/guide/b-segment-routing-cg-cisco8000-26xx.html) as the authoritative source for segment routing syntax, options, and platform behavior on Cisco 8000 (validate against your software release).

## Configuration source (Cisco 8000, IOS XR 26.x)

[Segment Routing Configuration Guide](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/segment-routing/26xx/configuration/guide/b-segment-routing-cg-cisco8000-26xx.html); [Routing Configuration Guide](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/routing/26xx/configuration/guide/b-routing-cg-cisco8000-26xx.html) (IS-IS).

## Sample IOS XR configuration

```text
! Cisco 8000 SR guide: enable SR-MPLS on IS-IS + node prefix-SID on Loopback0 + custom SRGB
segment-routing
 global-block 600000 699999
 local-block 20000 21000
!
router isis 1
 net 49.0001.1920.0200.0001.00
 affinity-map GLOBAL bit-position 2
 affinity-map REGION bit-position 1
 affinity-map COUNTRY bit-position 0
 address-family ipv4 unicast
  metric-style wide level 1
  microloop avoidance segment-routing
  router-id Loopback0
  segment-routing mpls
 !
 flex-algo 129
  priority 200
  advertise-definition
  affinity include-any REGION COUNTRY
 !
 interface Loopback0
  passive
  address-family ipv4 unicast
   prefix-sid index 1001
  !
 interface Bundle-Ether1
  circuit-type level-2-only
  bfd minimum-interval 15
  bfd multiplier 3
  bfd fast-detect ipv4
  affinity flex-algo REGION
  point-to-point
  hello-padding sometimes
  hello-password keychain ISIS-GROQ
  address-family ipv4 unicast
   fast-reroute per-prefix
   fast-reroute per-prefix ti-lfa
   metric 200
  !
 !
 mpls oam
!
```

> **Note:** Examples are illustrative for Cisco IOS XR on Cisco 8000-class systems. Validate syntax, scale limits, and feature availability for your exact release (K100/P100) and interface types.
