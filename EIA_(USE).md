# EIA (USE)

**Priority:** P1  
**Feature group:** EIA

## Overview

**EIA (USE)** covers routing policy for anycast, BGP FlowSpec, communities, local-preference, and related edge features. **VRRP** and some **6PE** corner cases may differ by platform—verify your specific topology in the compliance matrix.

## Configuration source (Cisco 8000, IOS XR 26.x)
 [Routing Configuration Guide](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/routing/26xx/configuration/guide/b-routing-cg-cisco8000-26xx.html) (route-policy).


## Sample IOS XR configuration

```text
! EIA (USE): VRRP configuration
router vrrp
 interface TenGigE 0/4/0/4
  address-family ipv4
  vrrp 1 version 2
  priority 120
  text-authentication cisco
  timer 3
  address 10.0.0.1


! EIA (USE): prefix-list, protocol typle, single/multiple community match and add, local preference
prefix-set PFX_LIST
  192.168.10.0/24,
  22.34.0.0/16
end-set
!
community-set CM-SINGLE
  65000:100
end-set
!
community-set CM-SINGLE-ADD
  65000:101
end-set
!
community-set CM-MULTIPLE-ADD
  6500:101,
  6501:102
end-set
!
route-policy RP-EIA-USE
  if community matches-every CM-SINGLE then
    drop
  endif
  if destination in PFX_LIST then
    set community CM-SINGLE-ADD additive
  elseif protocol is bgp 65001 then
    set local-preference 200
    set community CM-MULTIPLE-ADD additive
  elseif community matches-every CM-SINGLE then
    set local-preference 100
  else
    drop
  endif
end-policy
!

! EIA (USE): static route - VRF and Global Table
router static
 address-family ipv4 unicast
  192.168.123.128/25 172.16.123.1
 !
 vrf A
  address-family ipv4 unicast
   10.23.11.0/24 192.168.1.2
   172.16.101.0/24 1.1.1.2
  !
  address-family ipv6 unicast
   2001:23:11::/64 fd00::2
  !
 !
!
```

> **Note:** Examples are illustrative for Cisco IOS XR on Cisco 8000-class systems. Validate syntax, scale limits, and feature availability for your exact release (K100/P100) and interface types.
