# EIA (IBR)

**Priority:** P1  
**Feature group:** EIA

## Overview

**Remotely triggered blackhole (RTBH)** is a technique that drops undesirable traffic at the network edge by forwarding it to a null interface, mitigating attacks and enforcing blocklist filtering.

## Configuration source (Cisco 8000, IOS XR 26.x)

[Remote Triggered Blackhole](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/bgp/bgp-config-cisco8000/r-wrapper-route-filtering-and-policy-enforcement/remotely-triggered-blackhole-filtering.html)

## Sample IOS XR configuration

```text
route-policy RTBH
 if community matches-any (1234:4321) then
  set next-hop discard
  else
   pass
   endif
 end-policy

 router bgp 65001
  address-family ipv4 unicast

neighbor 192.168.102.2
 remote-as 65001
 address-family ipv4 unicast
  route-policy RTBH in
  route-policy bgp_all out
!
```

> **Note:** Examples are illustrative for Cisco IOS XR on Cisco 8000-class systems. Validate syntax, scale limits, and feature availability for your exact release (K100/P100) and interface types.
