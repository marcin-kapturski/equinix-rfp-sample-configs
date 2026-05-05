# Control Plane Protection (COPP)

**Priority:** P2  
**Feature group:** Control Plane

## Overview

**Control Plane Protection (CoPP)** on Cisco 8000 Series routers is achieved using ***Local Packet Transport Services (LPTS)** by creating a virtual LPTS interface and applying user-managed control and management plane **Access Control Lists (ACLs)** to filter and police traffic destined for the control and management planes.

## Configuration source (Cisco 8000, IOS XR 26.x)

[System Security Configuration Guide](https://www.cisco.com/c/en/us/td/docs/iosxr/cisco8000/ip-addresses/26xx/configuration/guide/b-ip-addresses-cg-8k-26xx/implementing-lpts.html) (Implementing LPTS).

## Sample IOS XR configuration

LPTS ACL mode 
```text 
hw-module profile cef lpts acl 

ipv4 access-list test-umpp-v4-filter
 10 permit icmp net-group ACL_GROUP_1 any police 67 pps 
 20 permit icmp net-group ACL_GROUP_2 any priority Medium
 30 permit icmp net-group ACL_GROUP_3 any priority High
 40 permit icmp net-group ACL_GROUP_4 any police 100 pps
 50 permit icmp any any 0
 60 permit icmp any any 3
 priority-timeout 25

interface lpts 0
 ipv4 access-group test-umpp-v4-filter ingress compress level 2
```

LPTS policer
```text
pts pifib hardware police
 low ospf unicast default rate 3000
 flow bgp default rate 4000
```

> **Note:** Examples are illustrative for Cisco IOS XR on Cisco 8000-class systems. Validate syntax, scale limits, and feature availability for your exact release (K100/P100) and interface types.
