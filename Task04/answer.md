# Task 4 – Choose a Medium for Each Run (4 Marks)

## Medium selection

| Run | Recommended medium | Reason |
|---|---|---|
| (a) Robot cell near arc welders, 85 m | Cat6A S/FTP shielded copper | Within the 100 m Ethernet channel limit; shielding reduces electromagnetic interference. Use proper grounding and industrial connectors. |
| (b) Office floor to shop-floor switch, 140 m | Multimode fibre | Exceeds the normal 100 m copper Ethernet channel limit; fibre is immune to electrical noise. |
| (c) Shop floor to data centre, 550 m | Multimode fibre | Copper Ethernet is beyond its 100 m limit; fibre supports high-speed backbone links. |
| (d) Plant to head office, 400 km | Single-mode fibre through a leased carrier service | Long-distance transmission with low attenuation; a managed leased service provides a predictable WAN connection. |

The 400 km run cannot use ordinary copper Ethernet because Ethernet twisted-pair channel length is normally limited to 100 m. A long-distance fibre or carrier WAN service is required.

## Shannon capacity

Formula: C = B log2(1 + SNR), where B is bandwidth in Hz and SNR is a linear ratio.

SNR in linear form = 10^(SNR(dB)/10).

For 250 MHz and 40 dB:

SNR = 10^4 = 10,000.

C = 250 × 10^6 × log2(1 + 10,000)
= **3,321.96 Mbps**.

For 250 MHz and 15 dB near welders:

SNR = 10^1.5 = 31.6228.

C = 250 × 10^6 × log2(1 + 31.6228)
= **1,256.95 Mbps**.

The theoretical capacity falls because electrical noise reduces SNR. The robot cell therefore needs shielded cable, careful grounding and error monitoring rather than relying on a noisy unshielded link.

A real link operates below the Shannon limit because of coding and modulation limits, interference, attenuation, protocol overhead, hardware capability, retransmissions and safety margins.

## Complete the Code – Python 3

Save as `capacity.py`.

```python
import math

def nyquist(bandwidth_hz, levels):
    return 2 * bandwidth_hz * math.log2(levels)

def shannon(bandwidth_hz, snr_db):
    snr = 10 ** (snr_db / 10)
    return bandwidth_hz * math.log2(1 + snr)

media = [
    ("Telephone line", 3100, 30),
    ("Cat6 pair", 250e6, 40),
    ("Wi-Fi 6 channel", 160e6, 25)
]

for name, bw, snr in media:
    print(f"{name:<16} Shannon limit {shannon(bw, snr) / 1e6:10.2f} Mbps")

print(f"Nyquist, 3,100 Hz with 4 levels: {nyquist(3100, 4) / 1e3:.1f} kbps")
```

Expected output:

```text
Telephone line   Shannon limit       0.03 Mbps
Cat6 pair        Shannon limit    3321.96 Mbps
Wi-Fi 6 channel  Shannon limit    1329.50 Mbps
Nyquist, 3,100 Hz with 4 levels: 12.4 kbps
```

## Telephone-line SNR for 56 kbps

Required SNR = 2^(C/B) − 1.

= 2^(56,000/3,100) − 1 = approximately 274,000.

SNR(dB) = 10 log10(274,000) = **54.4 dB**.

This is very high for a telephone line and is not realistic on a typical noisy line. In practice, advanced modulation and equalisation are used, but the Shannon calculation still shows the demanding signal quality required.

## Alternative concept: Wi-Fi 6 near welders

For a 160 MHz channel at 15 dB:

C = 160 × 10^6 × log2(1 + 31.6228)
= **804.45 Mbps** theoretical capacity.

Although the theoretical capacity is high, it does not prove that the wireless link can meet a 10 ms command deadline. Interference, contention, roaming, retries and variable latency make Wi-Fi less predictable. Shielded wired Ethernet is preferred for fixed robot controllers.
