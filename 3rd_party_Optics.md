# 3rd party Optics

**Priority:** P1  
**Feature group:** Optics

## Overview

Third-party **MSA-compliant** optics are supported when validated; **DOM** diagnostics are read via **show controllers** / **optics** CLIs.

## Configuration source (Cisco 8000, IOS XR 26.x)

[Interface and Hardware Configuration Guide](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/Interfaces/26xx/configuration/guide/b-interfaces-config-guide-cisco8k-r26xx.html) (transceiver); platform hardware docs.

## Sample IOS XR configuration

```text
! Third-party optics are supported by default on Cisco 8000 when MSA-compliant.
! Verify optic status with:
   show controllers optics <interface>
   show inventory

Available breakout options depend on the Optic type
controller optics <interface>
    breakout 4x10 | 4x25 | 8x25 | 1x40 | 2x50 | 8x50 | 1x100 | 2x100 | 3x100 | 4x100 | 8x100 | 2x200 | 2x400
!
```

> **Note:** Examples are illustrative for Cisco IOS XR on Cisco 8000-class systems. Validate syntax, scale limits, and feature availability for your exact release (K100/P100) and interface types.
