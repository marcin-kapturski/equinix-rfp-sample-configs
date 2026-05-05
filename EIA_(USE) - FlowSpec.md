# EIA (USE)

**Priority:** P1  
**Feature group:** EIA

## Overview

**IPv6 Provider Edge** or IPv6 VPN Provider Edge **(6PE/6VPE)** uses the existing MPLS IPv4 core infrastructure for IPv6 transport. 6PE/6VPE enables IPv6 sites to communicate with each other over an MPLS IPv4 core network using MPLS label switched paths (LSPs).

## Configuration source (Cisco 8000, IOS XR 26.x)

[BGP Configuration Guide](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/bgp/b-bgp-config-cisco8000/m-bgp-flowspec.html) (BGP FlowSpec)

## Sample IOS XR configuration
```text
!  EIA (USE): FlowSpec
 vrf vrf1
  address-family ipv4 unicast
   import route-target
    101:2000
   export route-target
    101:2000
   !
   address-family ipv4 flowspec
    import route-target
    101:2000
    export route-target
    101:2000
   !
  address-family vpnv4 flowspec
    import route-target
    101:2000
    export route-target
    101:2000
   !
flowspec
 address-family ipv4
  local-install interface-all
  !
  address-family vpnv4
   local-install interface-all
  !

/* Configure the policy to accept all presented routes without modifying the routes. */
route-policy pass-all
 pass
 end-policy

/* Configure the policy to reject all presented routes without modifying the routes */
 route-policy drop-all
  drop
  end-policy

router bgp 1
 nsr
  bgp router-id 10.2.3.3
   address-family ipv4 flowspec
   !
   address-family vpnv4 flowspec
   !
    neighbor 10.2.3.4
     remote-as 1
      address-family ipv4 flowspec
       route-policy pass-all in
       route-policy drop-all out
       !
      address-family vpnv4 flowspec
       route-policy pass-all in
       route-policy drop-all out
       !
       update-source Loopback0
```

> **Note:** Examples are illustrative for Cisco IOS XR on Cisco 8000-class systems. Validate syntax, scale limits, and feature availability for your exact release (K100/P100) and interface types.
