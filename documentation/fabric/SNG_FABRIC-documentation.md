# SNG_FABRIC

## Table of Contents

- [Fabric Switches and Management IP](#fabric-switches-and-management-ip)
  - [Fabric Switches with inband Management IP](#fabric-switches-with-inband-management-ip)
- [Fabric Topology](#fabric-topology)
- [Fabric IP Allocation](#fabric-ip-allocation)
  - [Fabric Point-To-Point Links](#fabric-point-to-point-links)
  - [Point-To-Point Links Node Allocation](#point-to-point-links-node-allocation)
  - [Loopback Interfaces (BGP EVPN Peering)](#loopback-interfaces-bgp-evpn-peering)
  - [Loopback0 Interfaces Node Allocation](#loopback0-interfaces-node-allocation)
  - [VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)](#vtep-loopback-vxlan-tunnel-source-interfaces-vteps-only)
  - [VTEP Loopback Node allocation](#vtep-loopback-node-allocation)

## Fabric Switches and Management IP

| POD | Type | Node | Management IP | Platform | Provisioned in CloudVision | Serial Number |
| --- | ---- | ---- | ------------- | -------- | -------------------------- | ------------- |
| SNG_FABRIC | l3leaf | LEAF1A | 192.168.0.13/24 | vEOS-Lab | Provisioned | SN-LEAF1A |
| SNG_FABRIC | l3leaf | LEAF1B | 192.168.0.14/24 | vEOS-Lab | Provisioned | SN-LEAF1B |
| SNG_FABRIC | l2leaf | LEAF1C | 192.168.0.15/24 | vEOS-Lab | Provisioned | SN-LEAF1C |
| SNG_FABRIC | l2leaf | LEAF1D | 192.168.0.16/24 | vEOS-Lab | Provisioned | SN-LEAF1D |
| SNG_FABRIC | l3leaf | LEAF2A | 192.168.0.17/24 | vEOS-Lab | Provisioned | SN-LEAF2A |
| SNG_FABRIC | l3leaf | LEAF2B | 192.168.0.18/24 | vEOS-Lab | Provisioned | SN-LEAF2B |
| SNG_FABRIC | l2leaf | LEAF2C | 192.168.0.19/24 | vEOS-Lab | Provisioned | SN-LEAF2C |
| SNG_FABRIC | l2leaf | LEAF2D | 192.168.0.20/24 | vEOS-Lab | Provisioned | SN-LEAF2D |
| SNG_FABRIC | spine | SPINE1 | 192.168.0.11/24 | vEOS-Lab | Provisioned | SN-SPINE1 |
| SNG_FABRIC | spine | SPINE2 | 192.168.0.12/24 | vEOS-Lab | Provisioned | SN-SPINE2 |

> Provision status is based on Ansible inventory declaration and do not represent real status from CloudVision.

### Fabric Switches with inband Management IP

| POD | Type | Node | Management IP | Inband Interface |
| --- | ---- | ---- | ------------- | ---------------- |

## Fabric Topology

| Type | Node | Node Interface | Peer Type | Peer Node | Peer Interface |
| ---- | ---- | -------------- | --------- | ----------| -------------- |
| l3leaf | LEAF1A | Ethernet49 | mlag_peer | LEAF1B | Ethernet49 |
| l3leaf | LEAF1A | Ethernet50 | mlag_peer | LEAF1B | Ethernet50 |
| l3leaf | LEAF1A | Ethernet51 | spine | SPINE1 | Ethernet1 |
| l3leaf | LEAF1A | Ethernet52 | spine | SPINE2 | Ethernet1 |
| l3leaf | LEAF1A | Ethernet53 | l2leaf | LEAF1C | Ethernet51 |
| l3leaf | LEAF1A | Ethernet54 | l2leaf | LEAF1D | Ethernet51 |
| l3leaf | LEAF1B | Ethernet51 | spine | SPINE1 | Ethernet2 |
| l3leaf | LEAF1B | Ethernet52 | spine | SPINE2 | Ethernet2 |
| l3leaf | LEAF1B | Ethernet53 | l2leaf | LEAF1C | Ethernet52 |
| l3leaf | LEAF1B | Ethernet54 | l2leaf | LEAF1D | Ethernet52 |
| l3leaf | LEAF2A | Ethernet49 | mlag_peer | LEAF2B | Ethernet49 |
| l3leaf | LEAF2A | Ethernet50 | mlag_peer | LEAF2B | Ethernet50 |
| l3leaf | LEAF2A | Ethernet51 | spine | SPINE1 | Ethernet3 |
| l3leaf | LEAF2A | Ethernet52 | spine | SPINE2 | Ethernet3 |
| l3leaf | LEAF2A | Ethernet53 | l2leaf | LEAF2C | Ethernet51 |
| l3leaf | LEAF2A | Ethernet54 | l2leaf | LEAF2D | Ethernet51 |
| l3leaf | LEAF2B | Ethernet51 | spine | SPINE1 | Ethernet4 |
| l3leaf | LEAF2B | Ethernet52 | spine | SPINE2 | Ethernet4 |
| l3leaf | LEAF2B | Ethernet53 | l2leaf | LEAF2C | Ethernet52 |
| l3leaf | LEAF2B | Ethernet54 | l2leaf | LEAF2D | Ethernet52 |

## Fabric IP Allocation

### Fabric Point-To-Point Links

| Uplink IPv4 Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ---------------- | ------------------- | ------------------ | ------------------ |
| 172.16.200.0/24 | 256 | 16 | 6.25 % |

### Point-To-Point Links Node Allocation

| Node | Node Interface | Node IP Address | Peer Node | Peer Interface | Peer IP Address |
| ---- | -------------- | --------------- | --------- | -------------- | --------------- |
| LEAF1A | Ethernet51 | 172.16.200.1/31 | SPINE1 | Ethernet1 | 172.16.200.0/31 |
| LEAF1A | Ethernet52 | 172.16.200.3/31 | SPINE2 | Ethernet1 | 172.16.200.2/31 |
| LEAF1B | Ethernet51 | 172.16.200.5/31 | SPINE1 | Ethernet2 | 172.16.200.4/31 |
| LEAF1B | Ethernet52 | 172.16.200.7/31 | SPINE2 | Ethernet2 | 172.16.200.6/31 |
| LEAF2A | Ethernet51 | 172.16.200.9/31 | SPINE1 | Ethernet3 | 172.16.200.8/31 |
| LEAF2A | Ethernet52 | 172.16.200.11/31 | SPINE2 | Ethernet3 | 172.16.200.10/31 |
| LEAF2B | Ethernet51 | 172.16.200.13/31 | SPINE1 | Ethernet4 | 172.16.200.12/31 |
| LEAF2B | Ethernet52 | 172.16.200.15/31 | SPINE2 | Ethernet4 | 172.16.200.14/31 |

### Loopback Interfaces (BGP EVPN Peering)

| Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------- | ------------------- | ------------------ | ------------------ |
| 172.16.0.0/24 | 256 | 4 | 1.57 % |
| 172.16.255.0/24 | 256 | 2 | 0.79 % |

### Loopback0 Interfaces Node Allocation

| POD | Node | Loopback0 |
| --- | ---- | --------- |
| SNG_FABRIC | LEAF1A | 172.16.0.3/32 |
| SNG_FABRIC | LEAF1B | 172.16.0.4/32 |
| SNG_FABRIC | LEAF2A | 172.16.0.5/32 |
| SNG_FABRIC | LEAF2B | 172.16.0.6/32 |
| SNG_FABRIC | SPINE1 | 172.16.255.1/32 |
| SNG_FABRIC | SPINE2 | 172.16.255.2/32 |

### VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)

| VTEP Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------------ | ------------------- | ------------------ | ------------------ |
| 172.16.1.0/24 | 256 | 4 | 1.57 % |

### VTEP Loopback Node allocation

| POD | Node | Loopback1 |
| --- | ---- | --------- |
| SNG_FABRIC | LEAF1A | 172.16.1.3/32 |
| SNG_FABRIC | LEAF1B | 172.16.1.3/32 |
| SNG_FABRIC | LEAF2A | 172.16.1.5/32 |
| SNG_FABRIC | LEAF2B | 172.16.1.5/32 |
