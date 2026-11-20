# Task 8 – Correct the Error or Ask Again? (3 Marks)

## Hamming check bits

For m = 16 data bits, choose r such that:

2^r ≥ m + r + 1.

For r = 4: 16 is less than 16 + 4 + 1 = 21, so it is insufficient.

For r = 5: 32 ≥ 16 + 5 + 1 = 22.

Therefore, **5 check bits** are required. Total codeword length = 16 + 5 = 21 bits.

Check-bit overhead as a percentage of transmitted bits = (5 / 21) × 100 = **23.81%**. Relative to the original data size, the added overhead is 5/16 × 100 = 31.25%.

## Minimum Hamming distance

Code = {00000, 01011, 10101, 11110}.

The minimum pairwise Hamming distance is **2** (for example, 01011 and 11110 differ in two positions).

A code with minimum distance d = 2 can detect up to d − 1 = **1 bit error**, but can correct floor((d − 1)/2) = **0 bit errors**.

## Correct the received Hamming(7,4) codeword

Original codeword = 0110011.

Received codeword = 0110111.

The changed bit is position 5. The syndrome is:

s1 = bit1 ⊕ bit3 ⊕ bit5 ⊕ bit7 = 0 ⊕ 1 ⊕ 1 ⊕ 1 = 1.

s2 = bit2 ⊕ bit3 ⊕ bit6 ⊕ bit7 = 1 ⊕ 1 ⊕ 1 ⊕ 1 = 0.

s3 = bit4 ⊕ bit5 ⊕ bit6 ⊕ bit7 = 0 ⊕ 1 ⊕ 1 ⊕ 1 = 1.

Position = s1 + 2s2 + 4s3 = 1 + 0 + 4 = **5**.

Flip position 5. Corrected codeword = 0110011. Data bits at positions 3, 5, 6 and 7 = **1011**.

## Complete the Code – Python 3

Save as `hamming.py`.

```python
def encode(d):
    p1 = d[0] ^ d[1] ^ d[3]
    p2 = d[0] ^ d[2] ^ d[3]
    p3 = d[1] ^ d[2] ^ d[3]
    return [p1, p2, d[0], p3, d[1], d[2], d[3]]

def decode(c):
    s1 = c[0] ^ c[2] ^ c[4] ^ c[6]
    s2 = c[1] ^ c[2] ^ c[5] ^ c[6]
    s3 = c[3] ^ c[4] ^ c[5] ^ c[6]
    pos = s1 + 2 * s2 + 4 * s3
    if pos != 0:
        c[pos - 1] ^= 1
    data = [c[2], c[4], c[5], c[6]]
    return pos, data

word = encode([1, 0, 1, 1])
print("codeword   :", "".join(map(str, word)))
for flip in (None, 5):
    rx = word[:]
    if flip:
        rx[flip - 1] ^= 1
    pos, data = decode(rx)
    print(f"error at {str(flip):<4}: syndrome {pos}, data {''.join(map(str, data))}")
```

Expected output:

```text
codeword   : 0110011
error at None: syndrome 0, data 1011
error at 5   : syndrome 5, data 1011
```

## Two-bit error limitation

Starting codeword: 0110011. Flip positions 1 and 2:

Received = 1010011.

The syndrome becomes s1=1, s2=1, s3=0, so the decoder calculates position 3 and flips that bit. It changes the wrong bit and produces incorrect data **0011**. Basic Hamming(7,4) corrects only one-bit errors; it cannot reliably correct two-bit errors.

## Alternative concept: FEC versus ARQ

Assumption: bit-error probability is 0.001 per bit, independent, and a 16-bit data reading is retransmitted if any bit is wrong.

Probability of at least one error in 16 bits:

p = 1 − (0.999)^16 = **0.01588 (1.588%)**.

**Hamming FEC:** 16 data bits + 5 check bits = **21 bits per reading**. It corrects a single-bit error without waiting for retransmission. A two-bit error may be miscorrected by basic Hamming(7,4); stronger codes are needed for robust multi-bit correction.

**ARQ:** Expected data bits sent with unlimited retries = 16 / (1 − p) = 16 / 0.98412 = **16.26 bits per successful reading**, on average. Expected retransmission waiting time = [p / (1 − p)] × 40 ms = **0.645 ms** per successful reading, excluding initial transmission and processing time.

For this radio link, FEC is useful when retransmission takes 40 ms and timely readings matter. ARQ can use fewer bits on average when errors are rare, but it adds variable delay. Video streaming and 5G use FEC because retransmission may arrive too late for real-time playback or low-latency service.
