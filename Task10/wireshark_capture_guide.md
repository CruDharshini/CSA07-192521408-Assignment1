# Task 10 – Ping, traceroute and Wireshark evidence

Use a real destination that is reachable from your network. The commands below use 1.1.1.1 as an example; if ICMP is blocked, choose another permitted destination.

## Windows commands

Run in Command Prompt:

```bat
ping -n 10 1.1.1.1 > ping_output.txt
tracert -d 1.1.1.1 > traceroute_output.txt
```

Run the commands from the Task10 folder or move the resulting text files into this folder.

## Wireshark capture

1. Open Wireshark and select the active network interface.
2. Start capturing.
3. Run the ping and tracert commands above while the capture is active.
4. Stop capture.
5. Use `icmp or tcp or udp` as a display filter.
6. Select a real packet and expand Ethernet II, IPv4 and its transport/control protocol.
7. Save the capture as `connection_capture.pcapng`.
8. Save a screenshot as `wireshark_headers_labelled.png`.

Useful filters:
- `icmp` – ping and ICMP time-exceeded responses, when present.
- `icmp.type == 11` – ICMP time exceeded.
- `tcp` – TCP traffic.
- `udp` – UDP traffic.
- `ip` – IPv4 packets.

## Measurement table

Fill `measurement_table.csv` using your own traceroute results. Record hop number, responding address, each RTT shown, and a short note. Leave a hop as timeout/no response when the command shows stars; do not invent an RTT.

## Required files

- `ping_output.txt`
- `traceroute_output.txt`
- completed `measurement_table.csv`
- `connection_capture.pcapng`
- `wireshark_headers_labelled.png`

The assignment's supplied traceroute is a question dataset; it is not a substitute for your own measured output.