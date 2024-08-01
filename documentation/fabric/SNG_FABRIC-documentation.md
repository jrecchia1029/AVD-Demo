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
| SNG_FABRIC | leaf | LEAF1A | 192.168.0.13/24 | vEOS-Lab | Provisioned | SN-LEAF1A |
| SNG_FABRIC | leaf | LEAF1B | 192.168.0.14/24 | vEOS-Lab | Provisioned | SN-LEAF1B |
| SNG_FABRIC | leaf | LEAF2A | 192.168.0.17/24 | vEOS-Lab | Provisioned | SN-LEAF2A |
| SNG_FABRIC | leaf | LEAF2B | 192.168.0.18/24 | vEOS-Lab | Provisioned | SN-LEAF2B |
| SNG_FABRIC | l3spine | SPINE1 | 192.168.0.11/24 | vEOS-Lab | Provisioned | SN-SPINE1 |
| SNG_FABRIC | l3spine | SPINE2 | 192.168.0.12/24 | vEOS-Lab | Provisioned | SN-SPINE2 |

> Provision status is based on Ansible inventory declaration and do not represent real status from CloudVision.

### Fabric Switches with inband Management IP

| POD | Type | Node | Management IP | Inband Interface |
| --- | ---- | ---- | ------------- | ---------------- |

## Fabric Topology

| Type | Node | Node Interface | Peer Type | Peer Node | Peer Interface |
| ---- | ---- | -------------- | --------- | ----------| -------------- |
| leaf | LEAF1A | Ethernet49 | mlag_peer | LEAF1B | Ethernet49 |
| leaf | LEAF1A | Ethernet50 | mlag_peer | LEAF1B | Ethernet50 |
| leaf | LEAF1A | Ethernet51 | l3spine | SPINE1 | Ethernet1 |
| leaf | LEAF1A | Ethernet52 | l3spine | SPINE2 | Ethernet1 |
| leaf | LEAF1B | Ethernet51 | l3spine | SPINE1 | Ethernet2 |
| leaf | LEAF1B | Ethernet52 | l3spine | SPINE2 | Ethernet2 |
| leaf | LEAF2A | Ethernet49 | mlag_peer | LEAF2B | Ethernet49 |
| leaf | LEAF2A | Ethernet50 | mlag_peer | LEAF2B | Ethernet50 |
| leaf | LEAF2A | Ethernet51 | l3spine | SPINE1 | Ethernet3 |
| leaf | LEAF2A | Ethernet52 | l3spine | SPINE2 | Ethernet3 |
| leaf | LEAF2B | Ethernet51 | l3spine | SPINE1 | Ethernet4 |
| leaf | LEAF2B | Ethernet52 | l3spine | SPINE2 | Ethernet4 |
| l3spine | SPINE1 | Ethernet49 | mlag_peer | SPINE2 | Ethernet49 |
| l3spine | SPINE1 | Ethernet50 | mlag_peer | SPINE2 | Ethernet50 |

## Fabric IP Allocation

### Fabric Point-To-Point Links

| Uplink IPv4 Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ---------------- | ------------------- | ------------------ | ------------------ |

### Point-To-Point Links Node Allocation

| Node | Node Interface | Node IP Address | Peer Node | Peer Interface | Peer IP Address |
| ---- | -------------- | --------------- | --------- | -------------- | --------------- |

### Loopback Interfaces (BGP EVPN Peering)

| Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------- | ------------------- | ------------------ | ------------------ |
| 172.16.255.0/24 | 256 | 2 | 0.79 % |

### Loopback0 Interfaces Node Allocation

| POD | Node | Loopback0 |
| --- | ---- | --------- |
| SNG_FABRIC | SPINE1 | 172.16.255.1/32 |
| SNG_FABRIC | SPINE2 | 172.16.255.2/32 |

### VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)

| VTEP Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| --------------------- | ------------------- | ------------------ | ------------------ |

### VTEP Loopback Node allocation

| POD | Node | Loopback1 |
| --- | ---- | --------- |
