# Cisco VLAN and LACP Network Lab

Small-business network built in Cisco Packet Tracer to practise switching, routing and troubleshooting. The network separates IT and Operations devices, provides DHCP and inter-VLAN routing, and uses LACP between two switches.

## Setup

- 1 Cisco 2911 router
- 2 Cisco 2960 switches
- 4 client PCs
- VLAN 10 for IT
- VLAN 20 for Operations
- 802.1Q trunks between network devices
- Router-on-a-stick for inter-VLAN routing
- DHCP configured on the router
- Two switch links combined as an LACP EtherChannel

## Addressing

| VLAN | Department | Network | Gateway |
|---:|---|---|---|
| 10 | IT | `192.168.10.0/24` | `192.168.10.1` |
| 20 | Operations | `192.168.20.0/24` | `192.168.20.1` |

DHCP assigns addresses from `.11` onwards in each subnet. Addresses `.1` to `.10` are reserved.

## Connections

| Device | Port | Use |
|---|---|---|
| SW1 | `Fa0/1` | IT-PC1, access VLAN 10 |
| SW1 | `Fa0/2` | OPS-PC1, access VLAN 20 |
| SW1 | `Gi0/1` | Trunk to R1 |
| SW2 | `Fa0/1` | IT-PC2, access VLAN 10 |
| SW2 | `Fa0/2` | OPS-PC2, access VLAN 20 |
| SW1 and SW2 | `Fa0/23-24` | LACP Port-channel 1 carrying VLANs 10 and 20 |

## Testing

I checked the configuration with:

```text
show vlan brief
show interfaces trunk
show etherchannel summary
show ip interface brief
show ip dhcp binding
```

LACP formed correctly on both switches:

```text
Po1(SU)    LACP    Fa0/23(P) Fa0/24(P)
```

An IT client received `192.168.10.11/24` through DHCP and successfully reached its gateway at `192.168.10.1`. It also successfully pinged the Operations client at `192.168.20.11`, confirming that inter-VLAN routing was working.

During testing, one client initially showed `0.0.0.0`. I checked the client configuration, enabled DHCP and tested the connection again successfully.

## Skills Practised

- IPv4 addressing and subnetting
- VLANs and access ports
- 802.1Q trunking
- Inter-VLAN routing
- DHCP
- LACP and EtherChannel
- Connectivity testing and troubleshooting

## Files

- `Cisco_VLAN_LACP_Network_Lab.pkt` - Packet Tracer project
- `images/` - topology and verification screenshots
