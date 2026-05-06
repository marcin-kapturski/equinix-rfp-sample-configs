# EIA (USE)

**Priority:** P1  
**Feature group:** EIA

## Overview

**EIA (USE)** covers routing policy for anycast, BGP FlowSpec, communities, local-preference, and related edge features. **VRRP** and some **6PE** corner cases may differ by platform—verify your specific topology in the compliance matrix.

## Configuration source (Cisco 8000, IOS XR 26.x)

[uRPF Configuration Guide](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/security/26xx/configuration/guide/b-system-security-cg-cisco8000-26xx/implementing-urpf.html);
[BGP Configuration Guide](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/bgp/b-bgp-config-cisco8000.html)

## Sample IOS XR configuration

```text
! EIA (USE/IBR): uRPF loose
interface HundredGigE 0/2/0/2
 VRF A
 ipv4 verify unicast source reachable-via any
 ipv6 verify unicast source reachable-via any

! EIA (USE/IBR): uRPF strict
hw-module profile cef unipath-surpf enable // Requires reload

interface HundredGigE 0/2/0/2
 VRF A
 ipv4 address 10.0.0.1 255.255.255.0
 ipv4 verify unicast source reachable-via rx
 ipv6 enable
 ipv6 address 2001::1/64
 ipv6 verify unicast source reachable-via rx

interface TenGigE0/3/0/8/2
 VRF A
 ipv6 address fe80::1 link-local
 ipv6 enable

! EIA (USE/IBR): EBGP MD5, ipv6 peer, 4-byte ASN, local-as, remove-private-AS
key chain BGP-MD5
 key 1
  accept-lifetime 00:00:00 january 01 2020 infinite
  key-string password 002A34282479295541715E4F11
  send-lifetime 00:00:00 january 01 2020 infinite
  cryptographic-algorithm MD5
 !
!
router bgp 40214
 address-family ipv6 unicast
  maximum-paths ebgp 4
 vrf A
  neighbor 2001:1234::1
   remote-as 111999999
   local-as 12200000
   keychain BGP-MD5
   address-family ipv6 unicast
    route-policy PASS in
    route-policy PASS out
    remove-private-AS
   !
  !
  neighbor fd80::2
   remote-as 1234
   address-family ipv6 unicast
    route-policy PASS in
    route-policy PASS out
   !
 !
!
```

> **Note:** Examples are illustrative for Cisco IOS XR on Cisco 8000-class systems. Validate syntax, scale limits, and feature availability for your exact release (K100/P100) and interface types.
