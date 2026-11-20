# Task 5 – Wi-Fi 6 or 5G for Moving Equipment? (4 Marks)

## Handover calculation

Route length = 1.2 km = 1,200 m. AGV speed = 2 m/s.

Travel time = 1,200 / 2 = **600 seconds**.

**Wi-Fi 6:** AP spacing = 30 m. Number of AP intervals = 1,200 / 30 = 40. Assuming the AGV starts in the first AP cell and finishes in the last, handovers = 40 − 1 = **39**.

Unreachable time = 39 × 150 ms = **5,850 ms = 5.85 s**.

**Private 5G:** Cell coverage = 300 m. Number of cells = 1,200 / 300 = 4. Handovers = 4 − 1 = **3**.

Unreachable time = 3 × 20 ms = **60 ms**.

Both totals exceed the 10 ms command deadline if commands are required during each interruption. 5G has far fewer and shorter interruptions, but handover interruption still needs mitigation through overlapping coverage, make-before-break mobility and application-level safety handling.

## 5G service types

| Equipment | Suitable 5G service | Reason |
|---|---|---|
| AGVs and robot control | URLLC | Requires very low latency and high reliability. |
| CCTV cameras | eMBB | Requires high data rates for video. |
| Large number of sensors | mMTC | Supports many low-data-rate connected devices. |

## Where short-range / low-power wireless fits

- Bluetooth: wearable maintenance devices, configuration and nearby peripherals.
- Zigbee: low-power sensor networks and building automation.
- LoRa/LoRaWAN: small, infrequent telemetry over long distances where low data rate is acceptable.
- Wi-Fi 6: office devices, tablets and non-critical mobile applications.
- Private 5G: mobile industrial equipment and applications needing managed mobility and QoS.

## 802.11r evaluation

802.11r fast transition can reduce Wi-Fi roaming time compared with a full authentication process. However, the assignment gives a 150 ms Wi-Fi handover and does not provide a measured 802.11r delay. Therefore, its exact delay cannot be calculated from the given data. Even if roaming is faster, interference, congestion, retries and coverage gaps remain risks. A measured test is required to prove a 10 ms deadline.

## Alternative concept: LoRaWAN for 600 sensors

Traffic per sensor = 50 bytes/s × 8 = 400 bits/s.

For 600 sensors = 600 × 400 = **240,000 bits/s = 240 kbps**.

One gateway capacity is approximately 5 kbps.

240 / 5 = **48 times** the stated gateway capacity. Therefore, all 600 sensors cannot fit through one 5 kbps gateway at the stated reporting rate. LoRaWAN may be used for infrequent, small telemetry after reducing reporting frequency or distributing devices across gateways. High-rate vibration monitoring and time-critical alarms should stay on wired Ethernet or a suitable Wi-Fi/5G network.

## Conclusion

Private 5G is a better mobility platform than ordinary Wi-Fi for AGVs, but the stated 20 ms handover still exceeds the 10 ms command deadline. The design must use overlapping cells, redundant connectivity and safe local control. Wi-Fi 6 remains suitable for office traffic, while LoRa is limited to low-rate sensor telemetry.
