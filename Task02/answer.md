# Task 2 – Count the Domains in the Contractor’s Drawing (3 Marks)

## Domain counts

**Collision domains: 17**

- Each of the three hub segments (8 PCs and its hub uplink) is one shared collision domain: 3.
- The central switch-to-router G0/0 link is one collision domain: 1.
- On the second switch, each of the 12 IP-phone access ports is a separate collision domain, and the router uplink is another: 13.
- Total = 3 + 1 + 13 = **17 collision domains**.

**Broadcast domains: 2**

The router separates the G0/0 LAN from the G0/1 LAN. Routers do not forward Layer-2 broadcasts by default. Therefore there are **2 broadcast domains**.

## Marked logical drawing

```text
8 PCs -- HUB 1 --8 PCs -- HUB 2 ---[24-port Switch 1]---(Collision domain)--- Router
8 PCs -- HUB 3 --/                                      G0/0 | G0/1
                                                         |     |
                                              Broadcast domain 1  Broadcast domain 2
                                                               [Switch 2]
                                                            / / ... /                                                       12 separate IP-phone ports
```

## OSI layer and operation

| Device | OSI layer | Reason |
|---|---|---|
| Hub | Layer 1 – Physical | Repeats incoming electrical signals to all ports; does not examine MAC addresses. |
| Switch | Layer 2 – Data Link | Learns MAC addresses and forwards Ethernet frames through selected ports. |
| Router | Layer 3 – Network | Routes IP packets between different IP networks and separates broadcast domains. |
| IP phone | Mainly Layers 2–7 | Uses Ethernet at Layer 2 and IP/voice protocols at higher layers. |

A hub cannot stop collisions because all connected devices share the same half-duplex collision domain. It repeats signals rather than isolating each port.

## Connection types

- PC-to-hub and hub-to-switch: physically point-to-point cable links, but all devices on a hub share the same multipoint collision medium.
- Switch-to-router: point-to-point.
- IP phone-to-switch: point-to-point.
- The hub's shared segment is logically multipoint.

## Standards table

| Technology | Standard / specification | Defining body |
|---|---|---|
| Ethernet | IEEE 802.3 | IEEE |
| Wi-Fi | IEEE 802.11 | IEEE |
| IPv4 | RFC 791 | IETF |
| OSI reference model | ISO 7498 | ISO |
| 5G | 3GPP specifications; IMT-2020 requirements | 3GPP develops specifications; ITU defines IMT requirements |

Standard-compliant equipment improves interoperability, compatibility, safety, predictable performance and support from different vendors. It also reduces vendor lock-in.

## Alternative concept: replace the three hubs with switches

Each 8-PC edge switch creates one collision domain per PC port (8) plus one uplink domain to the central switch (1). Therefore each edge switch contributes 9 domains.

New collision domains = (3 × 9) + 1 central switch-to-router link + 13 second-switch domains = **41**.

Broadcast domains remain **2**, because unmanaged switches forward broadcasts and do not create Layer-3 boundaries.

This change gives each PC a dedicated collision domain, supports full-duplex links and removes collisions between PCs on the same edge switch. It does not stop broadcast traffic; VLANs or routers are needed to divide broadcast domains.
