# BGP LU (NA)

**Priority:** P1  
**Feature group:** Control Plane

## Overview

**BGP Labeled Unicast (LU)** distributes labeled **IPv4**/**IPv6** prefixes for transport scalability (often with **SR-MPLS** or hierarchical BGP). Enable **`address-family ipv4 labeled-unicast`** (and **ipv6** if needed), then attach **neighbors** with the same address family and **route-policy** for label allocation and reachability—optionally **`allocate-label`**, **`maximum-prefix`**, **BFD**, etc.

## Configuration source (Cisco 8000, IOS XR 26.x)

[BGP Configuration Guide](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/bgp/b-bgp-config-cisco8000.html) (address-family ipv4 labeled-unicast).

## Sample IOS XR configuration

```text
! BGP-LU: prerequisite route-policies
hw-module profile cef bgplu enable
route-policy LU-IMPORT
  pass
end-policy
!
route-policy LU-EXPORT
  pass
end-policy
!
route-policy LU-EBGP-IN
  pass
end-policy
!
route-policy LU-EBGP-OUT
  pass
end-policy
!
! BGP-LU: global ipv4 unicast AF with allocate-label + iBGP and eBGP neighbors
! Note: 'address-family ipv4 labeled-unicast' is not supported on all Cisco 8000
! releases; use 'address-family ipv4 unicast' with 'allocate-label all' instead.
router bgp 65001
 bgp router-id 192.0.2.1
 address-family ipv4 unicast
  allocate-label all
 !
 ! iBGP LU (e.g. toward RR or core peer on loopbacks)
 neighbor 198.51.100.2
  remote-as 65001
  update-source Loopback0
  address-family ipv4 unicast
   route-policy LU-IMPORT in
   route-policy LU-EXPORT out
  !
 !
 ! eBGP LU toward another AS (transport / label peering)
 neighbor 203.0.113.10
  remote-as 65002
  address-family ipv4 unicast
   route-policy LU-EBGP-IN in
   route-policy LU-EBGP-OUT out
   maximum-prefix 500000 80 restart 5
  !
 !
!
```

> **Note:** Examples are illustrative for Cisco IOS XR on Cisco 8000-class systems. Validate syntax, scale limits, and feature availability for your exact release (K100/P100) and interface types.
