# Task 12 – Revised design build guide

The Day-4 change adds a private 5G network on the shop floor and 50 moving AGVs. The assignment's example design direction is 5G URLLC for AGVs and robots, Wi-Fi 6 for office users, and fibre for the backbone.

## Suggested logical topology to draw

- Core Layer-3 switch at the plant data centre.
- Fibre uplinks from the core to shop-floor and office distribution switches.
- Robot controller / robot network attached to the shop-floor switch.
- Private 5G network block on the shop floor, with a 5G core/base-station symbol if available in your Packet Tracer version; otherwise use a clearly labelled generic wireless segment as a conceptual placeholder.
- 50 AGV client icons or a small representative group labelled “50 AGVs – private 5G URLLC”.
- Wi-Fi 6 access point and office PCs/laptops on the office network.
- CCTV and sensor devices on their planned networks.
- Router/WAN cloud representing the fibre or leased-line connection to head office.

## Important Packet Tracer limitation

Packet Tracer versions may not provide a real private 5G/URLLC radio, mobility or handover simulation. If your version lacks those features, label the wireless segment as a conceptual 5G design representation. Do not claim Packet Tracer simulated real 5G latency or URLLC reliability.

## Addressing example

Use separate subnets to keep traffic groups distinct. These are placeholders; coordinate with your original Task 11 addressing plan.

| Network | Example subnet |
|---|---|
| Robot control | 10.10.1.0/24 |
| AGV / private 5G concept | 10.10.2.0/24 |
| CCTV | 10.10.3.0/24 |
| Sensors | 10.10.4.0/24 |
| Office Wi-Fi | 10.10.5.0/24 |
| Network management | 10.10.6.0/24 |

## Suggested labels on the drawing

- “5G URLLC – robots and 50 AGVs”
- “Wi-Fi 6 – office users”
- “Fibre backbone – shop floor to data centre”
- “Separate CCTV and sensor traffic”
- “QoS priority – robot/AGV command traffic”

## Save and test

1. Build the logical topology in Packet Tracer using devices available in your installed version.
2. Configure the IP addressing and routing that Packet Tracer supports.
3. Test connectivity between representative endpoints.
4. Save the genuine project as `revised.pkt`.
5. Take a screenshot named `revised_topology.png`.
6. If you use a conceptual 5G block, state clearly that 5G handover/URLLC performance is justified by calculations, not measured by Packet Tracer.

This guide is not a `.pkt` file. Create and save the actual project in Packet Tracer.