# Task 7 – Will the Receiver Notice the Error? (4 Marks)

## CRC calculation

Generator = 1011 (x³ + x + 1). Append three zeroes to the data:

Data = 1101011011

Dividend = 1101011011000

Modulo-2 division is performed using XOR (there is no carry or borrow). The remainder is **100**.

Transmitted frame = data + remainder = **1101011011100**.

The received frame in the question is 1101011011101. Its remainder is non-zero, so it is **rejected**.

The longest burst error guaranteed to be detected by a degree-3 generator is **3 bits**. A burst longer than 3 bits may also be detected, but detection is not guaranteed for every pattern.

An undetected error pattern is **1011** (or the same pattern shifted by adding trailing zeroes). It is a multiple of the generator polynomial, so the receiver obtains a zero remainder.

## Complete the Code – Python 3

Save as `crc.py`.

```python
def mod2_div(dividend, divisor):
    rem = list(dividend)
    for i in range(len(dividend) - len(divisor) + 1):
        if rem[i] == "1":
            for j in range(len(divisor)):
                rem[i + j] = str(int(rem[i + j]) ^ int(divisor[j]))
    return "".join(rem[-(len(divisor) - 1):])

def encode(data, gen):
    crc = mod2_div(data + "0" * (len(gen) - 1), gen)
    return data + crc

def check(frame, gen):
    return set(mod2_div(frame, gen)) == {"0"}

GEN = "1011"
frame = encode("1101011011", GEN)
print("transmitted:", frame)
print("no error   :", check(frame, GEN))
bad = frame[:4] + ("1" if frame[4] == "0" else "0") + frame[5:]
print("1-bit error:", check(bad, GEN))
burst = frame[:2] + "".join("1" if b == "0" else "0" for b in frame[2:5]) + frame[5:]
print("3-bit burst:", check(burst, GEN))
```

Expected output:

```text
transmitted: 1101011011100
no error   : True
1-bit error: False
3-bit burst: False
```

## Two-dimensional parity

Choose this 4 × 4 data block, using even parity:

```text
1 0 1 0 | 0
0 1 1 0 | 0
1 1 0 0 | 0
0 0 1 1 | 0
---------
0 0 1 1 | 0
```

The rightmost bits are row parity; the bottom row gives column parity. A four-bit rectangular error, for example flipping the four corner bits of a rectangle (positions (1,1), (1,2), (2,1), (2,2)), leaves each affected row and column with an even number of flipped bits. Therefore, two-dimensional parity misses this pattern.

## Internet checksum

Given 16-bit words (hex): 4500, 0073, 0000, 4000.

Sum = 0x4500 + 0x0073 + 0x0000 + 0x4000 = **0x8573**.

There is no carry beyond 16 bits.

One's complement = ~0x8573 = **0x7A8C**.

Internet checksum = **0x7A8C**.

## Alternative concept: CRC versus even parity

- Even parity detects any odd number of bit flips, but misses any even number of bit flips (for example, 2-bit and 4-bit errors).
- CRC-3 detects all single-bit errors and all burst errors of length up to 3 bits. It can miss some longer patterns, including an error polynomial divisible by 1011.
- Example: error pattern 1011 is missed by CRC-3 when aligned as a generator multiple. Even parity catches any two-bit error because it changes the parity. CRC-3 catches many two-bit errors, but this generator has period 7, so a two-bit error with bit positions separated by 7 can be an undetected pattern. The exact result depends on the frame length and bit positions.
- A four-bit error with an even number of flipped bits is missed by simple even parity. CRC may detect it, but a pattern that is a multiple of 1011 can pass CRC.

CRC is stronger for network frames because it checks a polynomial remainder over the entire bit sequence. Parity is simpler but provides weaker detection.
