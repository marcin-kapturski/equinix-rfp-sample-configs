# Routing Policies

**Priority:** P1 
**Feature group:** EIA

## Overview

The routing policy language (RPL) provides a single, straightforward language in which all routing policy needs can be expressed. RPL was designed to support large-scale routing configurations. It greatly reduces the redundancy inherent in previous routing policy configuration methods. RPL streamlines the routing policy configuration, reduces system resources required to store and process these configurations, and simplifies troubleshooting.

## Configuration source (Cisco 8000, IOS XR 26.x)

[BGP Configuration Guide](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/routing/26xx/configuration/guide/b-routing-cg-cisco8000-26xx/implement-routing-policy.html) (Implementing Routing Policy).

## Sample IOS XR configuration

```text
route-policy RPL-ISP-ADV
  if community in CM-SINGLE-ADD then
    apply RPL-PREPEND
  endif
end-policy
!
route-policy RPL-PREPEND
  prepend as-path own-as 2
end-policy
!
```

> **Note:** Examples are illustrative for Cisco IOS XR on Cisco 8000-class systems. Validate syntax, scale limits, and feature availability for your exact release (K100/P100) and interface types.
