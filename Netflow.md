# Netflow

**Priority:** P1  
**Feature group:** —

## Overview

**NetFlow / IPFIX** exports flow records to a collector. Configure **flow monitors**, **exporters**, and apply to **interfaces**.

## Configuration source (Cisco 8000, IOS XR 26.x)

[NetFlow and sFlow Configuration Guide](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/netflow/configuration/b-netflow-configuration-ios-xr-8000.html).

## Sample IOS XR configuration

```text
! Netflow
flow monitor-map FM-IPFIX
 record ipv4
 exporter EXP1
!
flow exporter-map EXP1
 destination 198.51.100.250
 source Loopback0
 transport udp 4739
!
sampler-map SM1
 random 1 out-of 1000
!
interface GigabitEthernet0/0/0/0
 flow ipv4 monitor FM-IPFIX sampler SM1 ingress
!

!Sflow
flow exporter-map EXP-MAP
 version sflow v5
 packet-length 9000
 transport udp 6343
 source HundredGigE 0/0/0/1
 source-address 192.127.10.1
 destination 192.127.0.1
 dfbit set
```

> **Note:** Examples are illustrative for Cisco IOS XR on Cisco 8000-class systems. Validate syntax, scale limits, and feature availability for your exact release (K100/P100) and interface types.
