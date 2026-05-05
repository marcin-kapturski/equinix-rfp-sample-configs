# EIA (USE)

**Priority:** P1  
**Feature group:** EIA

## Overview

**IPv6 Provider Edge** or IPv6 VPN Provider Edge **(6PE/6VPE)** uses the existing MPLS IPv4 core infrastructure for IPv6 transport. 6PE/6VPE enables IPv6 sites to communicate with each other over an MPLS IPv4 core network using MPLS label switched paths (LSPs).

## Configuration source (Cisco 8000, IOS XR 26.x)

[BGP Configuration Guide](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/l3vpn/26xx/configuration/guide/b-l3vpn-cg-cisco8000-26xx/implementing-ipv6-vpn-provider-edge-transport-over-mpls.html) (BGP 6PE/6VPE)

## Sample IOS XR configuration
```text
!  EIA (USE): 6PE
router bgp 10
bgp router-id 11.11.11.11
bgp graceful-restart
bgp log neighbor changes detail
!
address-family ipv6 unicast
 redistribute connected
  redistribute ospfv3 7
  allocate-label all
!
!
neighbor 66:1:2::2
  remote-as 201
  address-family ipv6 unicast
   route-policy pass-all in
   route-policy pass-all out
  !
!
neighbor 13.13.13.13
  remote-as 10
  update-source Loopback0
  address-family vpnv4 unicast
  !
  address-family ipv6 labeled-unicast
  !
  address-family vpnv6 unicast
!
vrf red
  rd 500:1
  address-family ipv4 unicast
   label mode per-vrf
   redistribute connected
   redistribute static
  !
  address-family ipv6 unicast
   label mode per-vrf
   redistribute connected
   redistribute static
  !
 !
!
interface HundredGigE0/0/1/0
vrf red
Ipv6 address 4002:110::1/128
!
exit
vrf red
address-family ipv4 unicast
import route-target
500:1
!
export route-target
500:1
!
!
address-family ipv6 unicast
import route-target
500:1
!
export route-target
500:1
!


```

> **Note:** Examples are illustrative for Cisco IOS XR on Cisco 8000-class systems. Validate syntax, scale limits, and feature availability for your exact release (K100/P100) and interface types.
