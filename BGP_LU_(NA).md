# BGP LU (NA)

**Priority:** P1  
**Feature group:** Control Plane

## Overview

**BGP Labeled Unicast (LU)** distributes labeled **IPv4**/**IPv6** prefixes for transport scalability (often with **SR-MPLS** or hierarchical BGP).

**Note:** 'address-family ipv4 labeled-unicast' is not supported on all Cisco 8000 releases; use 'address-family ipv4 unicast' with 'allocate-label all' instead.

## Configuration source (Cisco 8000, IOS XR 26.x)

[BGP Configuration Guide](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/bgp/b-bgp-config-cisco8000/m-label-alloc-and-mpls-support.html#bgp-labeled-unicast)

## Sample IOS XR configuration

hw-module profile cef bgplu enable" is missing as a prerequisite.
```text
!! Reload Required !!
hw-module profile cef bgplu enable
!!
route-policy pass-all
  pass
end-policy
!
router bgp 1
 bgp router-id 1.1.1.1
  address-family ipv4 unicast
    allocate-label all
!
 neighbor 2.2.2.1
 remote-as 2
 update-source Loopback0
!
 address-family ipv4 unicast
 route-policy pass-all in
 route-policy pass-all out
!
```

> **Note:** Examples are illustrative for Cisco IOS XR on Cisco 8000-class systems. Validate syntax, scale limits, and feature availability for your exact release (K100/P100) and interface types.
