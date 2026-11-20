# Task 3 – Read a Captured Frame Layer by Layer (4 Marks)

## Layer-by-layer interpretation

| Captured line | OSI layer | PDU |
|---|---|---|
| Ethernet II (source/destination MAC, EtherType 0x0800) | Layer 2 – Data Link | Frame |
| IPv4 (source/destination IP, TTL, protocol) | Layer 3 – Network | Packet |
| TCP (ports, sequence, acknowledgement) | Layer 4 – Transport | Segment |
| Modbus write-register command | Application layer | Data |

The Ethernet destination is 00:1d:9c:c7:b0:41 and source is 3c:52:82:4e:11:07. EtherType 0x0800 indicates IPv4. TCP destination port 502 is commonly used by Modbus TCP.

## Number of routers crossed

Initial TTL = 64; received TTL = 61.

Routers crossed = 64 − 61 = **3 routers** (assuming no device altered the TTL).

## Frame size and overhead

For this calculation, use the standard Ethernet header (14 bytes), IPv4 header (20 bytes), TCP header (20 bytes), payload (12 bytes) and FCS (4 bytes). Preamble, start-frame delimiter and inter-frame gap are excluded.

Total = 14 + 20 + 20 + 12 + 4 = **70 bytes**.

Overhead = 14 + 20 + 20 + 4 = 58 bytes.

Overhead percentage = (58 / 70) × 100 = **82.86%**.

For a 1,400-byte camera payload:

Total = 14 + 20 + 20 + 1,400 + 4 = **1,458 bytes**.

Overhead percentage = (58 / 1,458) × 100 = **3.98%**.

Small robot commands have a high percentage of header overhead. This is why small packets consume proportionally more bandwidth than large camera packets.

## Fault-to-layer mapping

- Wrong destination MAC: Layer 2 (Data Link); check switching, VLAN and MAC configuration.
- TTL reaches zero: Layer 3 (Network); the router discards the packet and normally sends ICMP Time Exceeded.
- Destination port closed: Layer 4 (Transport); TCP may return a reset (RST), or a firewall may reject/drop the connection.

## TCP/IP mapping

| TCP/IP model | Corresponding OSI layers |
|---|---|
| Application | 7 Application, 6 Presentation, 5 Session |
| Transport | 4 Transport |
| Internet | 3 Network |
| Network Access / Link | 2 Data Link, 1 Physical |

## Complete the Code – Python 3

Save as `encapsulation.py`.

```python
LAYERS = [
    ("Application", "HTTP"),
    ("Transport", "TCP"),
    ("Network", "IP"),
    ("Data link", "ETH")
]

def send(message):
    pdu = message
    for layer, tag in LAYERS:
        if tag == "ETH":
            pdu = f"[{tag}|{pdu}|FCS]"
        else:
            pdu = f"[{tag}|{pdu}]"
        print(f"{layer} adds {tag} -> {len(pdu)} bytes")
    return pdu

def receive(frame):
    pdu = frame
    for layer, tag in reversed(LAYERS):
        head = "[" + tag + "|"
        tail = "|FCS]" if tag == "ETH" else "]"
        assert pdu.startswith(head) and pdu.endswith(tail), f"{layer}: bad header"
        pdu = pdu[len(head):-len(tail)]
    return pdu

frame = send("GET /results")
print("on the wire:", frame)
print("delivered :", receive(frame))
print(f"overhead   : {len(frame) - len('GET /results')} bytes for 12 bytes of data")
```

Expected output:

```text
Application adds HTTP -> 19 bytes
Transport adds TCP -> 25 bytes
Network adds IP -> 30 bytes
Data link adds ETH -> 40 bytes
on the wire: [ETH|[IP|[TCP|[HTTP|GET /results]]]|FCS]
delivered : GET /results
overhead   : 28 bytes for 12 bytes of data
```

For a 1,400-byte message, the program's wrapper adds 28 bytes. Overhead percentage = 28 / 1,428 × 100 = **1.96%**. For the 12-byte message it is 28 / 40 × 100 = **70%**. (The capture calculation above uses real Ethernet/IP/TCP header sizes; this teaching program uses short text tags.)

## Alternative concept: direct industrial Ethernet

If the robot command is sent directly in an Ethernet frame using EtherType 0x8892, without IP and TCP, it saves 20 + 20 = **40 bytes per command**.

Commands per second per controller = 1 / 0.002 = 500.

For 40 controllers, saved data = 40 × 40 × 500 = 800,000 bytes/s = 6.4 Mbps.

On a 100 Mbps link, this is equivalent to 6.4 Mbps of link capacity. The plant loses IP addressing and normal IP routing, making communication less flexible across routed networks. The industrial protocol must provide the required addressing, scheduling and reliability features itself.
