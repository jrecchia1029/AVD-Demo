# NoLa-Leaf-3B

## Table of Contents

- [Management](#management)
  - [Agents](#agents)
  - [Management Interfaces](#management-interfaces)
  - [IP Name Servers](#ip-name-servers)
  - [NTP](#ntp)
  - [Management API HTTP](#management-api-http)
- [Authentication](#authentication)
  - [Local Users](#local-users)
  - [Enable Password](#enable-password)
  - [AAA Authorization](#aaa-authorization)
- [Monitoring](#monitoring)
  - [TerminAttr Daemon](#terminattr-daemon)
- [MLAG](#mlag)
  - [MLAG Summary](#mlag-summary)
  - [MLAG Device Configuration](#mlag-device-configuration)
- [Spanning Tree](#spanning-tree)
  - [Spanning Tree Summary](#spanning-tree-summary)
  - [Spanning Tree Device Configuration](#spanning-tree-device-configuration)
- [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
  - [Internal VLAN Allocation Policy Summary](#internal-vlan-allocation-policy-summary)
  - [Internal VLAN Allocation Policy Device Configuration](#internal-vlan-allocation-policy-device-configuration)
- [VLANs](#vlans)
  - [VLANs Summary](#vlans-summary)
  - [VLANs Device Configuration](#vlans-device-configuration)
- [Interfaces](#interfaces)
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Port-Channel Interfaces](#port-channel-interfaces)
  - [VLAN Interfaces](#vlan-interfaces)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Static Routes](#static-routes)
- [Multicast](#multicast)
  - [IP IGMP Snooping](#ip-igmp-snooping)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)

## Management

### Agents

#### Agent KernelFib

##### Environment Variables

| Name | Value |
| ---- | ----- |
| KERNELFIB_PROGRAM_ALL_ECMP | 'true' |

#### Agents Device Configuration

```eos
!
agent KernelFib environment KERNELFIB_PROGRAM_ALL_ECMP='true'
```

### Management Interfaces

#### Management Interfaces Summary

##### IPv4

| Management Interface | Description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management1 | - | oob | default | - | - |

##### IPv6

| Management Interface | Description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management1 | - | oob | default | - | - |

#### Management Interfaces Device Configuration

```eos
!
interface Management1
   no lldp transmit
   no lldp receive
```

### IP Name Servers

#### IP Name Servers Summary

| Name Server | VRF | Priority |
| ----------- | --- | -------- |
| 169.254.169.254 | default | - |
| 8.8.8.8 | default | - |
| 1.1.1.1 | default | - |

#### IP Name Servers Device Configuration

```eos
ip name-server vrf default 1.1.1.1
ip name-server vrf default 8.8.8.8
ip name-server vrf default 169.254.169.254
```

### NTP

#### NTP Summary

##### NTP Servers

| Server | VRF | Preferred | Burst | iBurst | Version | Min Poll | Max Poll | Local-interface | Key |
| ------ | --- | --------- | ----- | ------ | ------- | -------- | -------- | --------------- | --- |
| pool.ntp.org | default | - | - | - | - | - | - | - | - |

#### NTP Device Configuration

```eos
!
ntp server pool.ntp.org
```

### Management API HTTP

#### Management API HTTP Summary

| HTTP | HTTPS | UNIX-Socket | Default Services |
| ---- | ----- | ----------- | ---------------- |
| False | True | - | - |

#### Management API VRF Access

| VRF Name | IPv4 ACL | IPv6 ACL |
| -------- | -------- | -------- |
| default | - | - |

#### Management API HTTP Device Configuration

```eos
!
management api http-commands
   protocol https
   no shutdown
   !
   vrf default
      no shutdown
```

## Authentication

### Local Users

#### Local Users Summary

| User | Privilege | Role | Disabled | Shell |
| ---- | --------- | ---- | -------- | ----- |
| cvpadmin | 15 | network-admin | False | - |

#### Local Users Device Configuration

```eos
!
username cvpadmin privilege 15 role network-admin secret sha512 <removed>
```

### Enable Password

Enable password has been disabled

### AAA Authorization

#### AAA Authorization Summary

| Type | User Stores |
| ---- | ----------- |
| Exec | local |

Authorization for configuration commands is disabled.

#### AAA Authorization Device Configuration

```eos
aaa authorization exec default local
!
```

## Monitoring

### TerminAttr Daemon

#### TerminAttr Daemon Summary

| CV Compression | CloudVision Servers | VRF | Authentication | Smash Excludes | Ingest Exclude | Bypass AAA |
| -------------- | ------------------- | --- | -------------- | -------------- | -------------- | ---------- |
| gzip | apiserver.arista.io:443 | default | token-secure,/tmp/cv-onboarding-token | ale,flexCounter,hardware,kni,pulse,strata | /Sysdb/cell/1/agent,/Sysdb/cell/2/agent | True |

#### TerminAttr Daemon Device Configuration

```eos
!
daemon TerminAttr
   exec /usr/bin/TerminAttr -cvaddr=apiserver.arista.io:443 -cvauth=token-secure,/tmp/cv-onboarding-token -cvvrf=default -disableaaa -smashexcludes=ale,flexCounter,hardware,kni,pulse,strata -ingestexclude=/Sysdb/cell/1/agent,/Sysdb/cell/2/agent -taillogs
   no shutdown
```

## MLAG

### MLAG Summary

| Domain-id | Local-interface | Peer-address | Peer-link |
| --------- | --------------- | ------------ | --------- |
| MLAG | Vlan4094 | 192.168.254.8 | Port-Channel49 |

Dual primary detection is disabled.

### MLAG Device Configuration

```eos
!
mlag configuration
   domain-id MLAG
   local-interface Vlan4094
   peer-address 192.168.254.8
   peer-link Port-Channel49
   reload-delay mlag 300
   reload-delay non-mlag 330
```

## Spanning Tree

### Spanning Tree Summary

STP mode: **mstp**

#### MSTP Instance and Priority

| Instance(s) | Priority |
| -------- | -------- |
| 0 | 4096 |

#### Global Spanning-Tree Settings

- Spanning Tree disabled for VLANs: **4094**

### Spanning Tree Device Configuration

```eos
!
spanning-tree mode mstp
no spanning-tree vlan-id 4094
spanning-tree mst 0 priority 4096
```

## Internal VLAN Allocation Policy

### Internal VLAN Allocation Policy Summary

| Policy Allocation | Range Beginning | Range Ending |
| ------------------| --------------- | ------------ |
| ascending | 1006 | 1199 |

### Internal VLAN Allocation Policy Device Configuration

```eos
!
vlan internal order ascending range 1006 1199
```

## VLANs

### VLANs Summary

| VLAN ID | Name | Trunk Groups |
| ------- | ---- | ------------ |
| 4092 | INBAND_MGMT | - |
| 4094 | MLAG | MLAG |

### VLANs Device Configuration

```eos
!
vlan 4092
   name INBAND_MGMT
!
vlan 4094
   name MLAG
   trunk group MLAG
```

## Interfaces

### Ethernet Interfaces

#### Ethernet Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |
| Ethernet49 | MLAG_NoLa-Leaf-3A_Ethernet49 | *trunk | *- | *- | *MLAG | 49 |
| Ethernet50 | MLAG_NoLa-Leaf-3A_Ethernet50 | *trunk | *- | *- | *MLAG | 49 |
| Ethernet51 | L2_NoLa-Spine-1_Ethernet5 | *trunk | *4092 | *- | *- | 51 |
| Ethernet52 | L2_NoLa-Spine-2_Ethernet5 | *trunk | *4092 | *- | *- | 51 |
| Ethernet53 | L2_NoLa-Leaf-3C_Ethernet26 | *trunk | *11-12,21-22,3401-3402,4092 | *- | *- | 53 |
| Ethernet54 | L2_NoLa-Leaf-3D_Ethernet26 | *trunk | *11-12,21-22,3401-3402,4092 | *- | *- | 54 |
| Ethernet55 | L2_NoLa-Leaf-3E_Ethernet26 | *trunk | *11-12,21-22,3401-3402,4092 | *- | *- | 55 |

*Inherited from Port-Channel Interface

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet49
   description MLAG_NoLa-Leaf-3A_Ethernet49
   no shutdown
   switchport access vlan 4092
   switchport mode access
   switchport
   channel-group 49 mode active
!
interface Ethernet50
   description MLAG_NoLa-Leaf-3A_Ethernet50
   no shutdown
   switchport access vlan 4092
   switchport mode access
   switchport
   channel-group 49 mode active
!
interface Ethernet51
   description L2_NoLa-Spine-1_Ethernet5
   no shutdown
   channel-group 51 mode active
!
interface Ethernet52
   description L2_NoLa-Spine-2_Ethernet5
   no shutdown
   channel-group 51 mode active
!
interface Ethernet53
   description L2_NoLa-Leaf-3C_Ethernet26
   no shutdown
   switchport access vlan 4092
   switchport mode access
   switchport
   channel-group 53 mode active
!
interface Ethernet54
   description L2_NoLa-Leaf-3D_Ethernet26
   no shutdown
   switchport access vlan 4092
   switchport mode access
   switchport
   channel-group 54 mode active
!
interface Ethernet55
   description L2_NoLa-Leaf-3E_Ethernet26
   no shutdown
   switchport access vlan 4092
   switchport mode access
   switchport
   channel-group 55 mode active
```

### Port-Channel Interfaces

#### Port-Channel Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | LACP Fallback Timeout | LACP Fallback Mode | MLAG ID | EVPN ESI |
| --------- | ----------- | ---- | ----- | ----------- | ------------| --------------------- | ------------------ | ------- | -------- |
| Port-Channel49 | MLAG_NoLa-Leaf-3A_Port-Channel49 | trunk | - | - | MLAG | 30 | individual | - | - |
| Port-Channel51 | L2_SPINES_Port-Channel4 | trunk | 4092 | - | - | - | - | 51 | - |
| Port-Channel53 | L2_NoLa-Leaf-3C_Port-Channel25 | trunk | 11-12,21-22,3401-3402,4092 | - | - | 30 | individual | 53 | - |
| Port-Channel54 | L2_NoLa-Leaf-3D_Port-Channel25 | trunk | 11-12,21-22,3401-3402,4092 | - | - | 30 | individual | 54 | - |
| Port-Channel55 | L2_NoLa-Leaf-3E_Port-Channel25 | trunk | 11-12,21-22,3401-3402,4092 | - | - | 30 | individual | 55 | - |

#### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel49
   description MLAG_NoLa-Leaf-3A_Port-Channel49
   no shutdown
   switchport mode trunk
   switchport trunk group MLAG
   switchport
   port-channel lacp fallback individual
   port-channel lacp fallback timeout 30
!
interface Port-Channel51
   description L2_SPINES_Port-Channel4
   no shutdown
   switchport trunk allowed vlan 4092
   switchport mode trunk
   switchport
   mlag 51
!
interface Port-Channel53
   description L2_NoLa-Leaf-3C_Port-Channel25
   no shutdown
   switchport trunk allowed vlan 11-12,21-22,3401-3402,4092
   switchport mode trunk
   switchport
   port-channel lacp fallback individual
   port-channel lacp fallback timeout 30
   mlag 53
!
interface Port-Channel54
   description L2_NoLa-Leaf-3D_Port-Channel25
   no shutdown
   switchport trunk allowed vlan 11-12,21-22,3401-3402,4092
   switchport mode trunk
   switchport
   port-channel lacp fallback individual
   port-channel lacp fallback timeout 30
   mlag 54
!
interface Port-Channel55
   description L2_NoLa-Leaf-3E_Port-Channel25
   no shutdown
   switchport trunk allowed vlan 11-12,21-22,3401-3402,4092
   switchport mode trunk
   switchport
   port-channel lacp fallback individual
   port-channel lacp fallback timeout 30
   mlag 55
```

### VLAN Interfaces

#### VLAN Interfaces Summary

| Interface | Description | VRF |  MTU | Shutdown |
| --------- | ----------- | --- | ---- | -------- |
| Vlan4092 | Inband Management | default | 1500 | False |
| Vlan4094 | MLAG | default | 1500 | False |

##### IPv4

| Interface | VRF | IP Address | IP Address Virtual | IP Router Virtual Address | ACL In | ACL Out |
| --------- | --- | ---------- | ------------------ | ------------------------- | ------ | ------- |
| Vlan4092 |  default  |  10.40.92.9/24  |  -  |  -  |  -  |  -  |
| Vlan4094 |  default  |  192.168.254.9/31  |  -  |  -  |  -  |  -  |

#### VLAN Interfaces Device Configuration

```eos
!
interface Vlan4092
   description Inband Management
   no shutdown
   mtu 1500
   ip address 10.40.92.9/24
!
interface Vlan4094
   description MLAG
   no shutdown
   mtu 1500
   no autostate
   ip address 192.168.254.9/31
```

## Routing

### Service Routing Protocols Model

Multi agent routing protocol model enabled

```eos
!
service routing protocols model multi-agent
```

### IP Routing

#### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | False |

#### IP Routing Device Configuration

```eos
```

### IPv6 Routing

#### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | False |
| default | false |

### Static Routes

#### Static Routes Summary

| VRF | Destination Prefix | Next Hop IP | Exit interface | Administrative Distance | Tag | Route Name | Metric |
| --- | ------------------ | ----------- | -------------- | ----------------------- | --- | ---------- | ------ |
| default | 0.0.0.0/0 | 10.40.92.1 | - | 1 | - | - | - |

#### Static Routes Device Configuration

```eos
!
ip route 0.0.0.0/0 10.40.92.1
```

## Multicast

### IP IGMP Snooping

#### IP IGMP Snooping Summary

| IGMP Snooping | Fast Leave | Interface Restart Query | Proxy | Restart Query Interval | Robustness Variable |
| ------------- | ---------- | ----------------------- | ----- | ---------------------- | ------------------- |
| Enabled | - | - | - | - | - |

#### IP IGMP Snooping Device Configuration

```eos
```

## VRF Instances

### VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |

### VRF Instances Device Configuration

```eos
```
