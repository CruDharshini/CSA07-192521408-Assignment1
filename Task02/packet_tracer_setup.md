# Task 2 – Office network Packet Tracer build guide

Create this topology in Cisco Packet Tracer and save it as `office.pkt`.

## Devices

- 1 router with at least two Gigabit Ethernet interfaces (for example, 2911).
- 2 x 24-port switches (2960).
- 3 hubs.
- 24 PCs (8 PCs per hub).
- 12 IP phones.

## Cable / port plan

1. Connect PC1–PC8 to Hub1, PC9–PC16 to Hub2, and PC17–PC24 to Hub3 using copper straight-through cables.
2. Connect Hub1, Hub2 and Hub3 to Switch1 ports Fa0/1, Fa0/2 and Fa0/3.
3. Connect Switch1 Fa0/24 to Router G0/0.
4. Connect Router G0/1 to Switch2 Fa0/24.
5. Connect the 12 IP phones to Switch2 Fa0/1–Fa0/12.

Use the actual hub uplink ports available in your Packet Tracer device model.

## Suggested addressing (for testing only)

| Segment | Network | Router address |
|---|---|---|
| PCs behind the three hubs | 192.168.10.0/24 | 192.168.10.1 |
| IP phone segment | 192.168.20.0/24 | 192.168.20.1 |

Assign PCs addresses 192.168.10.11–192.168.10.34 with mask 255.255.255.0 and gateway 192.168.10.1. Assign phones 192.168.20.11–192.168.20.22 with mask 255.255.255.0 and gateway 192.168.20.1. These are example addresses; adjust if your lab requires a different scheme.

## Router CLI template

Enter in the router CLI:

```text
enable
configure terminal
interface gigabitEthernet 0/0
 description OFFICE_HUBS_AND_SWITCH
 ip address 192.168.10.1 255.255.255.0
 no shutdown
exit
interface gigabitEthernet 0/1
 description IP_PHONE_SWITCH
 ip address 192.168.20.1 255.255.255.0
 no shutdown
exit
end
show ip interface brief
```

## Switch setup

The basic topology works with default Layer-2 switch settings. Optional descriptions:

```text
enable
configure terminal
interface range fastEthernet 0/1-3
 description HUB_UPLINKS
exit
interface fastEthernet 0/24
 description ROUTER_G0_0
exit
end
show interfaces status
```

On Switch2, use Fa0/1–Fa0/12 for phones and Fa0/24 for the router uplink. If your Packet Tracer phone model requires a voice VLAN or call-control server, configure that only if your instructor's expected simulation requires it.

## What to save

- Save the Packet Tracer project as `office.pkt` in this folder.
- Take a screenshot named `office_topology_marked.png`.
- Mark each hub and its attached PCs as one shared collision domain; each switch port is its own collision domain; each router interface separates broadcast domains.
- Test connectivity with ping and save any actual test output.

**Important:** This is a build guide and configuration template, not a Packet Tracer project file. The genuine `.pkt` must be created and saved in Packet Tracer.