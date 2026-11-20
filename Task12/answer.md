# Task 12 – Re-evaluate and Adapt the Design (8 Marks)

## Decision: MODIFY the original design

The original hybrid network is retained, but private 5G, mobility protection and additional capacity are added for 50 AGVs. A fully wireless redesign is not recommended because fixed robots, CCTV aggregation and backbone traffic need predictable wired capacity.

## A. Impact analysis (2 Marks)

The original eight-line plant traffic from Task 1 is:

8 × 158.24 Mbps = **1,265.92 Mbps**.

The assignment does not specify the AGV command size or reporting interval. For a transparent estimate, assume each of the 50 AGVs sends a 200-byte command/status message every 2 ms, the same rate specified for each robot controller in Task 1.

Traffic per AGV = 200 × 8 / 0.002 = 800,000 bits/s = 0.8 Mbps.

Traffic for 50 AGVs = 50 × 0.8 = **40 Mbps**.

New plant traffic = 1,265.92 + 40 = **1,305.92 Mbps (1.306 Gbps)**.

If AGV messages are carried over private 5G, this traffic is radio access traffic but still contributes to the plant's backhaul load. A 1 Gbps plant uplink is insufficient. Use a 10 Gbps fibre backbone with capacity monitoring and QoS.

Affected choices:
- Fixed robot cell: retain shielded Cat6A S/FTP and fibre uplinks.
- AGVs: add private 5G URLLC with overlapping coverage and redundant connectivity.
- CCTV: retain wired Ethernet/fibre and a separate traffic class.
- Error control: use FEC where supported on wireless links and CRC/FCS on Ethernet; avoid depending on long ARQ retries for 10 ms control.
- Flow control: isolate traffic with VLANs, QoS queues and admission control.

## B. Test the original design (2 Marks)

For the 1.2 km route at 2 m/s, the AGV travel time is 600 seconds.

**Wi-Fi 6:** 30 m AP spacing gives 40 AP intervals and approximately 39 handovers. At 150 ms each:

39 × 150 ms = **5,850 ms = 5.85 seconds** of total interruption per trip.

**Private 5G:** 300 m cell coverage gives 4 cells and approximately 3 handovers. At 20 ms each:

3 × 20 ms = **60 ms** of total interruption per trip.

The private 5G result is much better than Wi-Fi, but one 20 ms handover is already twice the 10 ms command deadline. Therefore, ordinary break-before-make handover cannot by itself satisfy the requirement that commands are never missed. Overlapping coverage, make-before-break mobility, packet duplication/redundant paths and local AGV safety control are required. The exact interruption after these improvements must be measured in a site test; it cannot be calculated from the supplied data.

## C. Adaptation / revised design (2 Marks)

1. Use private 5G URLLC for AGV command and control traffic, with planned cell overlap along the complete route.
2. Enable redundant connectivity or packet duplication where supported so a handover does not create a command gap.
3. Keep emergency stop, obstacle avoidance and safe stopping logic local to the AGV; do not depend only on a remote network command.
4. Keep fixed robot controllers on shielded industrial Ethernet and use fibre for switch-to-switch and data-centre connections.
5. Keep CCTV on a separate wired VLAN; office devices use Wi-Fi 6.
6. Apply QoS, admission control, monitoring and alarms to prevent lower-priority video traffic from delaying control traffic.

## D. Evidence-based defence (2 Marks)

- Original plant load is 1,265.92 Mbps. With the assumed AGV traffic it becomes 1,305.92 Mbps, so a 1 Gbps uplink is not enough.
- A 10 Gbps fibre backbone provides adequate headroom for the calculated average traffic, subject to measured peaks and protocol overhead.
- Wi-Fi handover interruption is approximately 5.85 seconds per trip; private 5G reduces it to 60 ms. This supports choosing private 5G for mobile AGVs.
- However, the 20 ms 5G handover is still greater than the 10 ms deadline. Redundant coverage and local safety control are necessary; 5G alone cannot guarantee that no command is missed.
- The AGV traffic estimate depends on the stated assumption of a 200-byte message every 2 ms. Actual AGV application traffic must be measured before final capacity approval.

## Final decision

**MODIFY the original hybrid design.** Keep fibre and shielded Ethernet for fixed critical equipment, add private 5G URLLC for AGVs, use Wi-Fi 6 for office traffic and separate CCTV traffic. Add overlapping 5G coverage, redundant communication and local fail-safe control. This design meets the new mobility requirement more effectively while preserving predictable wired performance for fixed industrial equipment.
