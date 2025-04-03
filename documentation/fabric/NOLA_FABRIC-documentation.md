# NOLA_FABRIC

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
| NOLA_FABRIC | leaf | NoLa-Leaf-1A | 192.168.0.13/24 | vEOS-Lab | Provisioned | 71ad9035eb744147ad17336d45d1ecf7 |
| NOLA_FABRIC | leaf | NoLa-Leaf-1B | 192.168.0.14/24 | vEOS-Lab | Provisioned | 29836c5c96c742fca0fb0c3423215a51 |
| NOLA_FABRIC | leaf | NoLa-Leaf-2A | 192.168.0.17/24 | vEOS-Lab | Provisioned | a9522b7e2e88434aadb6c390488fd732 |
| NOLA_FABRIC | leaf | NoLa-Leaf-3A | - | vEOS-Lab | Provisioned | caa829d9d7934b60a659ff51f24d5f44 |
| NOLA_FABRIC | leaf | NoLa-Leaf-3B | - | vEOS-Lab | Provisioned | aafb7c8c056e41b69801443bd3855464 |
| NOLA_FABRIC | leaf | NoLa-Leaf-3C | - | vEOS-Lab | Provisioned | ed54e843eda44a4d9e37cda870bf24c9 |
| NOLA_FABRIC | leaf | NoLa-Leaf-3D | - | vEOS-Lab | Provisioned | 11ef492f59d54586811b4aadd9c4b3e3 |
| NOLA_FABRIC | leaf | NoLa-Leaf-3E | - | vEOS-Lab | Provisioned | 2dddf9d81e6c4a629d7bb73ea7bd5cbb |
| NOLA_FABRIC | l3spine | NoLa-Spine-1 | 192.168.0.11/24 | vEOS-Lab | Provisioned | 70b2dff9b4984bfea948c308937c91d9 |
| NOLA_FABRIC | l3spine | NoLa-Spine-2 | 192.168.0.12/24 | vEOS-Lab | Provisioned | 35c8e2b9a28446a6bfcc27803d720fc2 |

> Provision status is based on Ansible inventory declaration and do not represent real status from CloudVision.

### Fabric Switches with inband Management IP

| POD | Type | Node | Management IP | Inband Interface |
| --- | ---- | ---- | ------------- | ---------------- |
| NOLA_FABRIC | leaf | NoLa-Leaf-1A | 10.40.92.4/24 | Vlan4092 |
| NOLA_FABRIC | leaf | NoLa-Leaf-1B | 10.40.92.5/24 | Vlan4092 |
| NOLA_FABRIC | leaf | NoLa-Leaf-2A | 10.40.92.6/24 | Vlan4092 |
| NOLA_FABRIC | leaf | NoLa-Leaf-3A | 10.40.92.8/24 | Vlan4092 |
| NOLA_FABRIC | leaf | NoLa-Leaf-3B | 10.40.92.9/24 | Vlan4092 |
| NOLA_FABRIC | leaf | NoLa-Leaf-3C | 10.40.92.10/24 | Vlan4092 |
| NOLA_FABRIC | leaf | NoLa-Leaf-3D | 10.40.92.11/24 | Vlan4092 |
| NOLA_FABRIC | leaf | NoLa-Leaf-3E | 10.40.92.12/24 | Vlan4092 |

## Fabric Topology

| Type | Node | Node Interface | Peer Type | Peer Node | Peer Interface |
| ---- | ---- | -------------- | --------- | ----------| -------------- |
| leaf | NoLa-Leaf-1A | Ethernet49 | mlag_peer | NoLa-Leaf-1B | Ethernet49 |
| leaf | NoLa-Leaf-1A | Ethernet50 | mlag_peer | NoLa-Leaf-1B | Ethernet50 |
| leaf | NoLa-Leaf-1A | Ethernet51 | l3spine | NoLa-Spine-1 | Ethernet1 |
| leaf | NoLa-Leaf-1A | Ethernet52 | l3spine | NoLa-Spine-2 | Ethernet1 |
| leaf | NoLa-Leaf-1B | Ethernet51 | l3spine | NoLa-Spine-1 | Ethernet2 |
| leaf | NoLa-Leaf-1B | Ethernet52 | l3spine | NoLa-Spine-2 | Ethernet2 |
| leaf | NoLa-Leaf-2A | Ethernet97/1 | l3spine | NoLa-Spine-1 | Ethernet3 |
| leaf | NoLa-Leaf-2A | Ethernet98/1 | l3spine | NoLa-Spine-2 | Ethernet3 |
| leaf | NoLa-Leaf-3A | Ethernet49 | mlag_peer | NoLa-Leaf-3B | Ethernet49 |
| leaf | NoLa-Leaf-3A | Ethernet50 | mlag_peer | NoLa-Leaf-3B | Ethernet50 |
| leaf | NoLa-Leaf-3A | Ethernet51 | l3spine | NoLa-Spine-1 | Ethernet4 |
| leaf | NoLa-Leaf-3A | Ethernet52 | l3spine | NoLa-Spine-2 | Ethernet4 |
| leaf | NoLa-Leaf-3A | Ethernet53 | leaf | NoLa-Leaf-3C | Ethernet25 |
| leaf | NoLa-Leaf-3A | Ethernet54 | leaf | NoLa-Leaf-3D | Ethernet25 |
| leaf | NoLa-Leaf-3A | Ethernet55 | leaf | NoLa-Leaf-3E | Ethernet25 |
| leaf | NoLa-Leaf-3B | Ethernet51 | l3spine | NoLa-Spine-1 | Ethernet5 |
| leaf | NoLa-Leaf-3B | Ethernet52 | l3spine | NoLa-Spine-2 | Ethernet5 |
| leaf | NoLa-Leaf-3B | Ethernet53 | leaf | NoLa-Leaf-3C | Ethernet26 |
| leaf | NoLa-Leaf-3B | Ethernet54 | leaf | NoLa-Leaf-3D | Ethernet26 |
| leaf | NoLa-Leaf-3B | Ethernet55 | leaf | NoLa-Leaf-3E | Ethernet26 |
| l3spine | NoLa-Spine-1 | Ethernet49/1 | mlag_peer | NoLa-Spine-2 | Ethernet49/1 |
| l3spine | NoLa-Spine-1 | Ethernet50/1 | mlag_peer | NoLa-Spine-2 | Ethernet50/1 |

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
| NOLA_FABRIC | NoLa-Spine-1 | 172.16.255.1/32 |
| NOLA_FABRIC | NoLa-Spine-2 | 172.16.255.2/32 |

### VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)

| VTEP Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------------ | ------------------- | ------------------ | ------------------ |

### VTEP Loopback Node allocation

| POD | Node | Loopback1 |
| --- | ---- | --------- |
