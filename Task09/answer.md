# Task 9 – How Big Must the Window Be? (4 Marks)

## Link timing

Distance = 400 km = 400,000 m.

Propagation speed = 2 × 10^8 m/s.

One-way propagation time = 400,000 / (2 × 10^8) = **0.002 s = 2 ms**.

Frame size = 1,500 bytes = 12,000 bits.

Transmission time = 12,000 / (1 × 10^9) = **0.000012 s = 12 μs**.

a = propagation time / transmission time = 0.002 / 0.000012 = **166.67**.

## Stop-and-wait utilisation

U = 1 / (1 + 2a) = 1 / (1 + 333.33) = **0.299%**.

Throughput = 1 Gbps × 0.002994 = approximately **2.99 Mbps**.

This means most of the link remains idle while the sender waits for acknowledgement.

## Minimum sliding window

For full utilisation, W ≥ 1 + 2a.

W ≥ 1 + 333.33 = 334.33.

Smallest integer window = **335 frames**.

**Go-Back-N:** W ≤ 2^k − 1. For W = 335, k = 9 bits (2^9 − 1 = 511).

**Selective Repeat:** W ≤ 2^(k−1). For W = 335, k = 10 bits (2^9 = 512).

## Go-Back-N versus Selective Repeat

- Go-Back-N retransmits the lost frame and all later unacknowledged frames in the window. It requires a simpler receiver that normally accepts only the next in-order frame.
- Selective Repeat retransmits only lost or damaged frames. It needs more receiver buffering and logic to store out-of-order frames.

Selective Repeat resends fewer frames after one loss, but costs more receiver memory and complexity.

## HDLC and PPP framing

HDLC uses the flag sequence **01111110** at the start and end of a frame. Bit stuffing prevents the flag pattern from appearing accidentally in the data.

PPP also uses the flag byte **0x7E** to mark frame boundaries. It uses byte stuffing or escaping on asynchronous links.

## Complete the Code – Python 3

Save as `window.py`.

```python
def utilisation(frame_bits, rate_bps, distance_m, window, speed=2e8):
    tx = frame_bits / rate_bps
    prop = distance_m / speed
    a = prop / tx
    u = min(1, window / (1 + 2 * a))
    return u, a

links = [
    ("Campus LAN, 200 m, 1 Gbps", 1e9, 200),
    ("City fibre, 50 km, 1 Gbps", 1e9, 50_000),
    ("Satellite, 36,000 km, 10 Mbps", 10e6, 36_000_000)
]

for name, rate, dist in links:
    u1, a = utilisation(12_000, rate, dist, 1)
    u7, _ = utilisation(12_000, rate, dist, 7)
    u127, _ = utilisation(12_000, rate, dist, 127)
    print(f"{name:<31} a = {a:7.2f} stop-and-wait {u1:6.1%} "
          f"W=7 {u7:6.1%} W=127 {u127:6.1%}")
```

Expected output (rounded):

```text
Campus LAN, 200 m, 1 Gbps      a =    0.08 stop-and-wait  85.7% W=7 100.0% W=127 100.0%
City fibre, 50 km, 1 Gbps      a =   20.83 stop-and-wait   2.3% W=7  16.4% W=127 100.0%
Satellite, 36,000 km, 10 Mbps a =  150.00 stop-and-wait   0.3% W=7   2.3% W=127  42.2%
```

## Satellite minimum window

For the satellite link, a = 150.

W ≥ 1 + 2(150) = **301 frames**.

## Alternative concept: Selective Repeat window of 64

For the 400 km head-office link, a = 166.67.

U = 64 / (1 + 2 × 166.67) = 64 / 334.33 = **19.14%**.

Throughput = 1 Gbps × 0.1914 = **191.4 Mbps**.

Receiver buffer = 64 × 1,500 bytes = **96,000 bytes (96 kB)**, excluding implementation overhead.
