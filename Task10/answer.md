# Task 10 – Find Where the Delay Comes From (4 Marks)

## Propagation delay for 400 km fibre

Distance = 400 km = 400,000 m. Propagation speed = 2 × 10^8 m/s.

One-way propagation delay = 400,000 / (2 × 10^8) = **2 ms**.

Traceroute probe values are normally round-trip times (RTT). Assuming a similar 400 km return path, propagation RTT = 2 × 2 = **4 ms**. Compared with the final hop RTT of about 12.6 ms, fibre propagation accounts for approximately (4 / 12.6) × 100 = **31.7%**. The remaining time includes router processing, queuing and other path delays. This is an estimate because the return route may not be identical.

## Interpreting the traceroute

- Hop 3 has two values around 4 ms and one value of 38.2 ms. This isolated spike may be caused by temporary congestion or slow ICMP response processing/rate limiting at that router. Hop 4 and hop 6 return to about 12 ms, so the 38.2 ms value does not prove that forwarded traffic experiences the same delay.
- Hop 5 shows asterisks because no response arrived before the timeout. The router may filter or rate-limit traceroute probes. It is not necessarily a forwarding fault, especially because hop 6 responds.
- Hop 3 has the largest individual probe delay (38.2 ms), but it is an isolated response. The typical path RTT rises from about 0.6 ms at hop 1 to about 12.6 ms at hop 6. The table does not provide enough information to assign all cumulative delay to one router.

## Own measurement evidence

The assignment requires your own ping, traceroute and Wireshark capture. Run these commands on your computer and save the actual outputs. Replace the example target with an approved head-office or test server.

**Windows PowerShell / Command Prompt**

```powershell
ping -n 10 1.1.1.1
tracert -d 1.1.1.1
```

**Linux**

```bash
ping -c 10 1.1.1.1
traceroute -n 1.1.1.1
```

Open Wireshark, select the active network interface, start capture, visit a website or run ping, and stop the capture. Save it as `Task10/connection_capture.pcapng`.

In Wireshark, expand:
- Ethernet II: source and destination MAC addresses (Layer 2).
- Internet Protocol Version 4/6: source and destination IP addresses, TTL/hop limit (Layer 3).
- TCP or UDP: source and destination ports (Layer 4).
- Application protocol such as DNS, HTTP or TLS, when present (upper layers).

Do not claim a real measurement until you have run the commands and saved your own capture.

## Alternative concept: continuous monitoring

| Method | What it detects | Detection speed |
|---|---|---|
| Occasional ping and traceroute | End-to-end reachability, RTT changes, route changes and visible hop response delays | Only when the test is run; a short fault between tests can be missed. |
| SNMP interface counters | Interface errors, discards, utilisation, link status and traffic levels | Continuous polling; speed depends on polling interval and alert configuration. |
| NetFlow/IPFIX flow records | Which hosts and applications use bandwidth, traffic volume, flows and congestion patterns | Near-real-time or periodic export; it does not directly measure every packet's latency. |

SNMP counters can reveal rising CRC errors or interface drops. Flow records can show whether CCTV or another application is consuming bandwidth. Ping and traceroute are useful for quick path checks, but continuous monitoring gives better visibility between manual tests.
