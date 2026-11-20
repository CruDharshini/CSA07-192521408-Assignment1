# Task 3 – Wireshark capture guide

The assignment requires your own capture and a screenshot with Ethernet, IPv4 and TCP headers identified. Capture traffic on your active Ethernet/Wi-Fi adapter; do not submit a made-up capture.

## Capture

1. Open Wireshark and select the interface that is carrying your network traffic.
2. Start capture.
3. Generate a real connection, for example open a website in your browser or run `ping -n 4 1.1.1.1` in Windows Command Prompt.
4. Stop the capture after traffic appears.
5. Apply display filter: `eth && ip && tcp`.
6. Select a TCP packet and expand **Ethernet II**, **Internet Protocol Version 4**, and **Transmission Control Protocol**.
7. Save as `frame_capture.pcapng`.
8. Take a screenshot showing the selected packet and expanded headers; save as `frame_headers_labelled.png`.

## Useful display filters

- Ethernet + IPv4 + TCP: `eth && ip && tcp`
- TCP only: `tcp`
- IPv4 only: `ip`
- Modbus TCP (if present): `tcp.port == 502`
- ICMP ping packets: `icmp`

## Header identification checklist

- Ethernet II: source MAC, destination MAC, EtherType.
- IPv4: source IP, destination IP, TTL, protocol.
- TCP: source port, destination port, sequence number, acknowledgement number, payload length.

A normal web capture may not contain Modbus TCP on port 502. If it does not, identify the headers in a real TCP packet and keep the assignment's supplied Modbus example separate from your own capture. Do not claim that an unrelated packet is a Modbus command.

## Files to commit

- `frame_capture.pcapng` – your own capture.
- `frame_headers_labelled.png` – your own labelled Wireshark screenshot.