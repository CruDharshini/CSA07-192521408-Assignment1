# CSA07 – Computer Networks Assignment 1

This repository contains practical files, executable programs, verified sample outputs and evidence templates only. Written assignment answers are not stored here.

## Environment

- Student: Dharshini
- Register number: 192521408
- Department: Information Technology
- OS used by student: record the actual OS/version used before submission.
- Python: Python 3. Record the local version with `python --version`.
- Cisco Packet Tracer: record the installed version when creating the .pkt files.
- Wireshark: record the installed version when creating captures.

## Task folders

| Folder | Practical files / evidence |
|---|---|
| Task01 | FILES_TO_ADD.md – supporting traffic calculation evidence |
| Task02 | Packet Tracer build guide, router/switch CLI templates, FILES_TO_ADD.md – save genuine office.pkt and marked screenshot in Packet Tracer |
| Task03 | encapsulation.py, output, Wireshark capture guide and filters – save your own .pcapng and labelled screenshot |
| Task04 | capacity.py, capacity_output.txt, FILES_TO_ADD.md |
| Task05 | FILES_TO_ADD.md – wireless planning evidence if created |
| Task06 | FILES_TO_ADD.md – ethtool before/after output and switch config |
| Task07 | crc.py, crc_output.txt, FILES_TO_ADD.md |
| Task08 | hamming.py, hamming_output.txt, FILES_TO_ADD.md |
| Task09 | window.py, window_output.txt, FILES_TO_ADD.md |
| Task10 | measurement_table.csv, Wireshark capture guide and filters – add your own ping/tracert output, .pcapng and labelled screenshot |
| Task11 | FILES_TO_ADD.md – optional design diagram/calculation evidence |
| Task12 | revised design Packet Tracer guide and example addressing plan – save genuine revised.pkt and topology screenshot |

## Run the Python programs

Run from the repository root:

```bash
python Task03/encapsulation.py
python Task04/capacity.py
python Task07/crc.py
python Task08/hamming.py
python Task09/window.py
```

The corresponding `*_output.txt` files contain outputs verified by executing the committed Python logic in the assistant's execution environment. Re-run the programs locally and capture your own terminal screenshots for submission.

## Evidence that must be created on the student's computer

The following files are not fabricated or included because they must come from the actual tools and network:
- `Task02/office.pkt` and marked topology screenshot.
- `Task03/frame_capture.pcapng` and labelled Wireshark screenshot.
- `Task06/ethtool_before.txt`, `ethtool_after.txt` and any corresponding switch-port configuration evidence.
- `Task10/ping_output.txt`, `traceroute_output.txt`, completed `measurement_table.csv`, `connection_capture.pcapng` and labelled Wireshark screenshot.
- `Task12/revised.pkt` and revised topology screenshot.

See each task's `FILES_TO_ADD.md` for the specific filenames and purpose. Do not replace actual measurements with sample data.

## Submission reminder

The assignment report containing calculations, diagrams, interpretations and written answers is separate from this repository. This repository is intended for code, project files, configurations, actual outputs and evidence.
