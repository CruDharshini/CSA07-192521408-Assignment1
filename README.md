# CSA07 – Computer Networks Assignment 1 (CO1)

**Student:** Dharshini  
**Register Number:** 192521408  
**Department:** Information Technology  
**Academic Year:** 2026–2027

## Repository structure

- `Task01/answer.md` – Task 1 answer and calculations
- `Task02/answer.md` – Task 2 answer and calculations
- `Task03/answer.md` – Task 3 answer and calculations
- `Task04/answer.md` – Task 4 answer and calculations
- `Task05/answer.md` – Task 5 answer and calculations
- `Task06/answer.md` – Task 6 answer and calculations
- `Task07/answer.md` – Task 7 answer and calculations
- `Task08/answer.md` – Task 8 answer and calculations
- `Task09/answer.md` – Task 9 answer and calculations
- `Task10/answer.md` – Task 10 answer and calculations
- `Task11/answer.md` – Task 11 answer and calculations
- `Task12/answer.md` – Task 12 answer and calculations
- `Task07/crc.py` – Executable CRC program

## Environment
- Operating system: Windows; exact build not recorded.
- Python: Python 3 required. Record the installed version using `python --version`.
- Packet Tracer and Wireshark versions: record the versions actually used.

## Running code
Open a terminal at the repository root. The CRC script can be run using:

```bash
python Task07/crc.py
```

Completed code for Tasks 3, 4, 8 and 9 is included in the corresponding answer.md files. Copy each code block into a `.py` file in its task folder before execution.

## Evidence to add from your own computer
- Task 2: Packet Tracer topology and `.pkt` file, if Packet Tracer is used.
- Task 3: Own Wireshark capture with Ethernet, IP and transport headers labelled.
- Task 6: `ethtool` output before and after the duplex correction, or equivalent Windows adapter evidence.
- Task 10: Own ping and traceroute output, measurement table and Wireshark `.pcapng` capture.
- Task 12: Revised Packet Tracer `.pkt` file and screenshots if using Packet Tracer.

Do not describe example calculations as measurements from a real plant.

## Assumptions
- Task 1 treats listed per-device rates as simultaneous peak rates and uses decimal Mbps.
- Task 2 assumes each phone uses a separate switch port.
- Task 3 states which headers are included; its teaching program uses text tags, not binary protocol headers.
- Task 5 assumes 39 Wi-Fi and 3 5G handovers over the route.
- Task 6 ring recovery depends on the selected industrial ring protocol.
- Task 8 assumes independent bit errors at BER 0.001 and unlimited ARQ retries.
- Task 9 uses the assignment's 12,000-bit frame for the supplied link comparison.
- Task 12 assumes each AGV sends 200 bytes every 2 ms because the assignment does not specify AGV traffic.

## Repository
https://github.com/CruDharshini/CSA07-192521408-Assignment1

Written answers and code currently added. Actual captures, screenshots, Packet Tracer files and measured outputs must be added after generating them locally.