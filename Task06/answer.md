# Task 6 – Interpret an Ethernet Port Report (4 Marks)

## Meaning of the errors

- **Late collision:** A collision detected after the normal Ethernet collision window. It can occur because of a duplex mismatch, excessive cable length or a faulty link configuration.
- **CRC error:** The received frame failed its Frame Check Sequence calculation, indicating that bits may have been corrupted during transmission.

The combination of 412 late collisions and 1,907 CRC errors strongly suggests a physical/link problem. The reported half-duplex 100 Mbps setting at the PC while the switch port is set to auto-negotiation can produce a duplex mismatch. The switch may operate full duplex while the PC operates half duplex.

## Linux correction using ethtool

First inspect the interface:

```bash
sudo ethtool eth0
```

If the switch port is configured for auto-negotiation, enable auto-negotiation on the PC too:

```bash
sudo ethtool -s eth0 autoneg on
sudo ethtool eth0
```

Alternatively, if the network administrator chooses fixed settings, both ends must be configured identically, for example 100 Mbps full duplex:

```bash
sudo ethtool -s eth0 speed 100 duplex full autoneg off
```

Do not force only one end. The switch port and PC must use matching speed and duplex. Recheck counters after the change. These commands may reset after reboot unless configured persistently by the operating system's network manager.

## MAC address interpretation

- **01:00:5e:0a:01:02** is an IPv4 multicast MAC address. It identifies a group, not one individual device.
- The first three bytes of **3c:52:82:4e:11:07** are **3C:52:82**, the OUI portion assigned to a manufacturer. The remaining bytes identify the interface within that manufacturer's address allocation. The manufacturer name should be verified using an IEEE OUI registry rather than guessed.

## Jumbo frames

Jumbo frames of about 9,000 bytes can reduce per-packet processing and header overhead for large CCTV transfers to a storage server. They do not increase the physical link speed and may not help small packets.

For jumbo frames to work, the server NIC, switch ports and every intermediate Layer-2 device on the path must support and be configured for the same compatible MTU. Otherwise, large frames may be dropped or fragmented at higher layers.

## Alternative concept: ring versus star

| Topology | Cable-cut behaviour | Devices affected |
|---|---|---|
| Industrial ring with redundancy | Ring recovery protocol detects the break and redirects traffic through the other direction. Recovery time depends on the protocol and switch configuration; no exact value is supplied in the question. | Usually devices remain reachable after recovery; traffic may be interrupted briefly. |
| Star | A cut in one device's access cable isolates that device only. A cut in the central switch uplink or switch failure can disconnect all devices downstream of that point. | Depends on which star link fails. |

A ring can provide an alternate path after one cable cut, while a simple star is easier to manage but has a central point of failure. The actual recovery time must be measured from the selected industrial ring protocol and equipment.
