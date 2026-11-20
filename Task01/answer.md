# Task 1 – Size the Traffic Before Choosing Anything (4 Marks)

## Traffic calculation

**1. Robot controllers**

Each controller sends 200 bytes every 2 ms.

Traffic = 40 × 200 × 8 / 0.002 = 32,000,000 bits/s = **32 Mbps**.

**2. Vibration sensors**

Traffic = 600 × 50 × 8 = 240,000 bits/s = **0.24 Mbps**.

**3. CCTV cameras**

Traffic = 24 × 4 Mbps = **96 Mbps**.

**4. Office users**

Active users = 150 × 10% = 15.

Traffic = 15 × 2 Mbps = **30 Mbps**.

**Total traffic per production line**

32 + 0.24 + 96 + 30 = **158.24 Mbps**.

For 8 lines: 158.24 × 8 = **1,265.92 Mbps (approximately 1.266 Gbps)**.

## Evaluation

- CCTV generates the highest volume at 96 Mbps.
- Robot commands have the strictest delay requirement: 10 ms. Therefore, the largest traffic source and the most time-critical source are not the same.
- A 1 Gbps uplink is enough for one line (158.24 Mbps), but it is not enough for all eight lines (1,265.92 Mbps), even before protocol overhead and growth. A 10 Gbps plant uplink is a safer choice.
- If CCTV and robot commands share one FIFO queue, a large burst of video packets can make robot commands wait. This can violate the 10 ms deadline even when average bandwidth is available. Separate VLANs/queues and strict priority or time-aware scheduling should be used for control traffic.

## Harm caused by a 100 ms delay

1. **Robot commands:** Most harmful. A 100 ms delay is ten times the 10 ms deadline and may cause unsafe or inaccurate movement.
2. **AGV/control traffic (if present):** A delayed command can interrupt movement coordination and create safety risks.
3. **CCTV:** A 100 ms delay affects live monitoring but recorded video can usually tolerate buffering.
4. **ERP/e-mail:** Users may notice slow responses, but the data is not normally time-critical.
5. **Vibration/temperature reports:** A 100 ms delay is usually acceptable for routine monitoring, but alarms should be prioritised.

## Alternative concept: motion-triggered CCTV

If cameras transmit clips only 5% of the time:

CCTV traffic = 96 × 0.05 = **4.8 Mbps**.

New line traffic = 32 + 0.24 + 4.8 + 30 = **67.04 Mbps**.

For 8 lines: 67.04 × 8 = **536.32 Mbps**.

The 1 Gbps uplink would then be sufficient for the calculated average traffic of all eight lines, subject to burst capacity, overhead and future growth. Local recording also reduces continuous network load.

## Conclusion

CCTV is the largest traffic source, while robot commands need the lowest and most predictable delay. The plant should use traffic prioritisation and a 10 Gbps backbone for the original continuous-video design. Motion-triggered recording reduces the calculated eight-line load to 536.32 Mbps, making a 1 Gbps uplink sufficient on average.
