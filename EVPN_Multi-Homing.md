# EVPN all Types

**Priority:** P3  
**Feature group:** L2 Services

## Overview

An **EVPN E-LAN all-active multihoming** mode is a networking model that enables multiple simultaneous active connections from an EVPN to a single Ethernet LAN allows all PE routers attached to a particular Ethernet segment to forward traffic to and from that Ethernet segment, and provides redundancy and load balancing by allowing traffic distribution across all available links, enhancing network reliability and efficiency by utilizing all potential paths without requiring a failover mechanism.

## Configuration source (Cisco 8000, IOS XR 26.x)

[EVPN MPLS Multihoming](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/evpn/b-evpn-config-cisco8000/evpn-mpls-multihoming.html#evpn-e-lan-all-active-multihoming-mode)

## Sample IOS XR configuration

```text
/* PE1 Configuration */
router bgp 100
 bgp router-id 54.54.54.54
 address-family l2vpn evpn
 !
 neighbor 51.51.51.51
  remote-as 100
  update-source Loopback0
  address-family l2vpn evpn
  !
 !
 neighbor 55.55.55.55
  remote-as 100
  update-source Loopback0
  address-family l2vpn evpn
  !
 !
!
mpls ldp
 router-id 54.54.54.54
 interface FourHundredGigE0/0/0/2
 !
!/* PE2 Configuration */
router bgp 100
 bgp router-id 55.55.55.55
 address-family l2vpn evpn
 !
 neighbor 51.51.51.51
  remote-as 100
  update-source Loopback0
  address-family l2vpn evpn
  !
 !
 neighbor 54.54.54.54
  remote-as 100
  update-source Loopback0
  address-family l2vpn evpn
  !
 !
!
mpls ldp
 router-id 55.55.55.55
 interface FourHundredGigE0/0/0/2
 !
!
/* Configuration on PE1 and PE2 */
l2vpn
 bridge group bg1
  bridge-domain bd1
   interface Bundle-Ether11.1
   !
   evi 1
   !
  !
 !
 bridge group bg2
  bridge-domain bd2
   interface Bundle-Ether11.2
   !
   evi 2
   !
  !
 !
!
evpn
 evi 1
  advertise-mac
  !
 !
 evi 2
  advertise-mac
  !
 !
 interface Bundle-Ether11
  ethernet-segment
   identifier type 0 40.00.00.00.00.00.00.00.01
  !
 !
!
```
> **Note:** Examples are illustrative for Cisco IOS XR on Cisco 8000-class systems. Validate syntax, scale limits, and feature availability for your exact release (K100/P100) and interface types.
