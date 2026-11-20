# Task 11 – Defend the Network Design (4 Marks)

## Recommended plant network

| Area / traffic | Medium and technology | Ethernet / network settings |
|---|---|---|
| Fixed robot controllers | Cat6A S/FTP industrial Ethernet; fibre uplinks | Full duplex, auto-negotiation consistently configured, dedicated VLAN, strict priority / TSN where supported |
| Welding-area sensors | Shielded industrial Ethernet for critical/high-rate sensors | Proper grounding, CRC monitoring, separate control VLAN |
| General low-rate sensors | Zigbee or LoRaWAN where reporting is infrequent and delay is not critical | Separate gateway and monitoring network |
| CCTV | Fibre backbone from camera access switches; Ethernet to cameras | Separate CCTV VLAN and bandwidth policy; jumbo MTU only if every device on path supports it |
| Office users | Wi-Fi 6 and wired Ethernet | Separate office VLAN; normal QoS priority |
| AGVs | Private 5G URLLC with overlapping coverage and local safety control | Dedicated QoS slice / traffic class, redundant coverage and monitored handover |
| Plant to head office | Single-mode fibre leased service | Routed WAN, QoS for production data, encryption and redundant WAN path |

## Error control and flow control

- **Robot wired Ethernet:** Ethernet FCS/CRC detects corrupted frames. Industrial real-time protocols and TSN scheduling should be used to bound delay. Avoid relying on repeated retransmissions for commands with a 10 ms deadline.
- **Wireless AGVs and sensors:** Use link-layer FEC where supported to correct errors without waiting for a long retransmission. Use controlled retries only where the deadline permits. Private 5G provides managed QoS but does not remove the need for local safety logic.
- **Office and ERP traffic:** TCP provides checksum-based error detection, acknowledgements and retransmission. TCP flow control uses the receiver-advertised window; congestion control prevents persistent overload.
- **CCTV:** Use Ethernet FCS and application buffering; prioritise live streams below robot-control traffic.
- **Head-office WAN:** Use TCP/ARQ for reliable business data and a sliding window rather than stop-and-wait. Task 9 shows a 400 km, 1 Gbps path has a = 166.67; stop-and-wait throughput is only about 2.99 Mbps. A window of 335 frames is required for full theoretical utilisation under the stated assumptions.

## Numerical justification

- Task 1 gives 158.24 Mbps per line and 1,265.92 Mbps for eight lines. A 1 Gbps uplink is therefore insufficient for the original all-eight-line continuous-video design. Use at least a 10 Gbps plant backbone/uplink, with capacity planning for peaks.
- CCTV is 96 Mbps per line, while robot command traffic is 32 Mbps per line. Robot commands need the strict 10 ms deadline, so queue priority must be based on latency criticality, not volume.
- Task 4 gives a 250 MHz channel Shannon limit of about 3,321.96 Mbps at 40 dB SNR and 1,256.95 Mbps at 15 dB. This supports choosing shielded cable near welders rather than trusting a noisy wireless link.
- Task 5 gives 5.85 s total Wi-Fi handover interruption versus 60 ms for 5G across the route. 5G is better for mobility, but even 60 ms is above the 10 ms command deadline; local control and handover resilience are essential.
- Task 9 shows the 400 km link has a 2 ms one-way propagation time and needs a large sliding window. A 10 Gbps or suitably provisioned carrier fibre service should be considered as production data grows.

## Two biggest remaining risks

1. **Deterministic latency and mobility:** Wireless interference, 5G handover, congestion or coverage gaps can still delay AGV/robot commands. Use overlapping cells, redundant paths, local fail-safe control and measured acceptance tests.
2. **Capacity and physical faults:** Continuous CCTV and plant growth can overload uplinks; welding interference and duplex/CRC faults can corrupt traffic. Use fibre backbone, shielded copper at the cell, monitoring, redundant links and QoS.

## Five-plant scalability

Assuming each plant has eight production lines:

Traffic per plant = 1,265.92 Mbps.

For five plants = 5 × 1,265.92 = **6,329.6 Mbps = 6.33 Gbps**.

A 10 Gbps head-office link can carry this calculated average in theory, but it leaves limited room for peak traffic, protocol overhead and growth. A 10 Gbps service is a minimum planning baseline; a redundant 10 Gbps or higher-capacity WAN should be evaluated using measured peak utilisation and service-level requirements.

## Alternative concept: entire plant on private 5G

Private 5G should carry mobile AGVs, mobile maintenance terminals and selected sensor traffic. eMBB can support high-rate video where radio capacity and coverage are proven; mMTC can support large numbers of low-rate sensors; URLLC is intended for critical control. Fixed robot control, core switching, CCTV aggregation and data-centre connections should remain on wired Ethernet/fibre because they offer predictable capacity and avoid radio contention. A fully wireless plant would increase dependence on radio coverage, spectrum and handover performance.

## Conclusion

A hybrid design is the most suitable choice: shielded Ethernet for fixed critical machines, fibre for the backbone and WAN, private 5G for AGVs, Wi-Fi 6 for offices and low-power wireless for suitable sensors. QoS, redundancy, monitoring and local fail-safe control are required to protect the 10 ms control deadline.
