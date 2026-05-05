# EVPL

**Priority:** P1  
**Feature group:** L2 Ethernet E-Line

## Overview

**EVPL** is a point-to-point **E-Line** on Cisco 8000 using **EVPN VPWS**: attachment circuits are **l2transport** subinterfaces; the **control plane** is **BGP EVPN** (**address-family l2vpn evpn**) with **EVI**-based signaling (**neighbor evpn evi** … **target** / **source**). Supports VLAN manipulation, multiple TPID values, control-word, and FAT where applicable—validate per release.

## Configuration source (Cisco 8000, IOS XR 26.x)

[L2VPN Configuration Guide](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/l2vpn/26xx/configuration/guide/b-l2vpn-cg-cisco8000-26xx.html); [EVPN Configuration Guide](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/evpn/b-evpn-config-cisco8000.html) (EVPN VPWS / xconnect).

## Sample IOS XR configuration

```text
! EVPL: prerequisite route-policy
route-policy PASS
  pass
end-policy
!
! EVPL (EVPN VPWS): subinterface AC + BGP EVPN control plane
evpn
 evi 1001
!
interface TenGigE0/3/0/8/0.12 l2transport
 encapsulation dot1q 12
 rewrite ingress tag pop 1 symmetric
!
interface TenGigE0/3/0/4/0.14 l2transport
 encapsulation dot1q 14 second-dot1q 1233
 rewrite ingress tag pop 2 symmetric
!
l2vpn
 pw-class FAT_CLASS
  load-balancing
   flow-label both    ! Options: both, transmit, or receive
  !
 !
 xconnect group XG-EVPL
  p2p VPWS-1001
   interface TenGigE0/3/0/8/0.12
   !
   interface TenGigE0/3/0/4/0.14
   !
   neighbor evpn evi 1001 service 10
    pw-class FAT_CLASS
!
router bgp 65001
 bgp router-id 192.0.2.1
 address-family l2vpn evpn
  retain route-target all
 !
 neighbor 198.51.100.2
  remote-as 65001
  update-source Loopback0
  address-family l2vpn evpn
   route-policy PASS in
   route-policy PASS out
!
! Align EVI, RD/RT, and pw-class (control-word / encapsulation) with your design
```
> **Note:** Examples are illustrative for Cisco IOS XR on Cisco 8000-class systems. Validate syntax, scale limits, and feature availability for your exact release (K100/P100) and interface types.
