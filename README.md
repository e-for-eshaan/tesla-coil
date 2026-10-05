# Tesla Coil

A small Tesla coil that does two things at once: it lights a fluorescent tube from across the table with no wires, and it carries a message through the air on the same field. The coil is a Slayer exciter, the message is keyed onto it by a Raspberry Pi, and a Hamming (7,4) code lets the receiver fix bit errors on the way through.

The work was published as a peer-reviewed conference paper: [**Encoding Data Using Slayer Exciter Circuits with Simultaneous Power and Information Transmission**](https://link.springer.com/chapter/10.1007/978-981-96-0047-2_7), in the proceedings of *Recent Developments in Control, Automation and Power Engineering* (RDCAPE 2023), Springer, 2025.

## What it does

- **Wireless power.** The secondary coil throws a high-frequency electromagnetic field that lights a fluorescent lamp held near it. No contact, no wires.
- **Wireless data.** The Pi switches the coil on and off in a pattern, turning the field into a carrier for binary data. A loop of wire on the far side picks it up.
- **Error correction.** Every four bits of the message are padded to seven with parity bits. The receiver can detect and correct any single flipped bit, which matters when the channel is a spark coil.
- **One circuit for both.** The same coil, the same drive transistor and the same field do the power transfer and the data link.

## How it works

```
message  →  Raspberry Pi  →  Hamming (7,4) encode  →  GPIO keys MOSFET / relay
                                                              ↓
          fluorescent lamp lights up  ←  Slayer exciter field  ←  coil on/off
                                                              ↓
                   wire loop  →  oscilloscope  →  demodulate  →  decode  →  message
```

### 1. Wireless power

A Slayer exciter is the simplest Tesla coil there is: an air-cored transformer with a few turns on the primary, hundreds on the secondary, and a single transistor acting as a switch. Feedback from the secondary drives the transistor at the coil's resonant frequency, so a low DC input becomes a high-frequency, high-voltage AC output. The field around the secondary is strong enough to excite the mercury vapour in a fluorescent tube, which lights up with nothing connected to it.

### 2. Encoding and modulation

The Raspberry Pi takes a message, splits it into 4-bit nibbles and runs each through a Hamming (7,4) encoder, which adds three parity bits. The resulting 7-bit codewords go out one bit at a time on a GPIO pin that drives a MOSFET (or a relay) in series with the exciter's supply.

This is amplitude-shift keying at its most basic: coil on is a `1`, coil off is a `0`. Each bit holds the coil in its state for a fixed interval, so the receiver only has to sample at the same rate.

### 3. Reception

A loop of wire near the coil picks up the field. On an oscilloscope the carrier is clearly visible as bursts, one burst per `1` bit, so the bitstream can be read straight off the trace. Grouping the bits back into sevens and running the Hamming decoder recovers the nibbles, and with them the original message, even if a bit was lost to noise along the way.

## Hamming (7,4) in brief

Four data bits `d1 d2 d3 d4` become seven bits with three parity bits placed at positions 1, 2 and 4:

```
position:  1   2   3   4   5   6   7
bit:       p1  p2  d1  p3  d2  d3  d4

p1 = d1 ⊕ d2 ⊕ d4
p2 = d1 ⊕ d3 ⊕ d4
p3 = d2 ⊕ d3 ⊕ d4
```

On receipt, recomputing the three parities gives a 3-bit syndrome. Zero means the codeword is clean; any other value is the position of the single bit that flipped, so the decoder just inverts it. The cost is 75% more bits on the wire, which is cheap for a channel this noisy.

A reference encoder for the Pi fits in a few lines of Python:

```python
def hamming74(nibble):            # nibble: 4 data bits, MSB first
    d1, d2, d3, d4 = nibble
    p1 = d1 ^ d2 ^ d4
    p2 = d1 ^ d3 ^ d4
    p3 = d2 ^ d3 ^ d4
    return [p1, p2, d1, p3, d2, d3, d4]
```

Each returned bit is written to the GPIO pin, held for one bit period, and the pin is cleared between codewords.

## Parts

- Slayer exciter: a hand-wound secondary, a few-turn primary, an NPN power transistor, a base resistor and a diode, on a 9 V to 12 V supply
- Raspberry Pi (any model with GPIO)
- Logic-level MOSFET or a relay module to key the exciter's supply
- A fluorescent tube or CFL as the wireless load
- A loop of wire and an oscilloscope for the receiver

## Build it

1. Wind and wire the Slayer exciter. Check that a fluorescent tube lights near the secondary before adding anything else.
2. Put the MOSFET or relay in series with the exciter's supply and drive its gate from a Pi GPIO pin.
3. Encode the message with Hamming (7,4) and bit-bang the codewords onto the GPIO pin at a fixed bit rate.
4. Place the pickup loop near the coil and watch the bursts on the oscilloscope.
5. Sample the trace at the bit rate, group into sevens, decode, and read the message back.

## Where this goes

The same idea scales to wireless charging, short-range sensors, and Li-Fi-style links where light or a field stands in for a cable. The interesting part was never the coil on its own. It was that a single noisy field can deliver energy and a reliable message at the same time.

## Publication

- **Encoding Data Using Slayer Exciter Circuits with Simultaneous Power and Information Transmission.** In *Recent Developments in Control, Automation and Power Engineering (RDCAPE 2023)*, Springer, 2025. DOI: [10.1007/978-981-96-0047-2_7](https://doi.org/10.1007/978-981-96-0047-2_7)
