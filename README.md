![access-gate](https://raw.githubusercontent.com/mytechnotalent/access-gate/main/access-gate.png)

<br>

## FREE Reverse Engineering Self-Study Course [HERE](https://github.com/mytechnotalent/reverse-engineering)
## FREE Embedded Hacking Course [HERE](https://github.com/mytechnotalent/Embedded-Hacking)

<br>

# OPERATION IRON GATE

### Access Gate & Vault Control
#### Act II of OPERATION COLD IRON

<br>

***
**LEGAL DISCLAIMER:**
The information, tools, and code provided in this repository and course are strictly for educational, research, and defensive purposes only. 

You are explicitly prohibited from using any materials contained herein to access, test, modify, or exploit any device, network, or system that you do not own 100% or for which you do not have explicit, documented, and legally binding authorization to interact with.

By using this repository and course, you acknowledge and agree that:

1. Any illegal, unauthorized, or malicious use of this information is solely your responsibility.
2. The author(s) and contributor(s) of this repository and course shall not be held liable for any damages, legal repercussions, criminal charges, or unauthorized actions resulting from the use, misuse, or abuse of the contents herein.
3. You will comply with all applicable local, state, national, and international laws regarding cybersecurity and computer fraud.

**IF YOU DO NOT AGREE WITH THESE TERMS, DO NOT USE THIS REPOSITORY AND COURSE.**
***

<br>
<br>

> Hello, friend.
>
> In Act I you learned the first lesson: a sensor can lie. It reported minus
> eighteen while the store warmed, and the lie was the product.
>
> This is the second lesson. The door.
>
> The frame you pulled out of the monitor led somewhere. A NorthPharma logistics
> annex with one entrance and no windows. NIGHTINGALE sent her last ping from
> inside it, then went quiet. FROSTLINE is already wiping the site. WHITEOUT has
> until dawn.
>
> The only way in is the access gate. A keypad nobody watches, a deadbolt on a
> servo, and an audit uplink that is supposed to tell the truth about who came
> through.
>
> Last time the AI wrote bad code for a sensor. This time it wrote bad code for a
> door. Worse: the gate keeps its final verdict in plain SRAM, and the
> cryptography around it is perfect. So the door trusts a boolean.
>
> You are going to walk through it, friend. Then you are going to seal it.

<br>

## THE SYSTEM

NorthPharma owns the cold chain. Not all of it. Enough of it. The monitors, the
gateways, the insurance, the audits. When a shipment spoils, they pay a claim,
raise the premium, and sell a newer monitor. Failure is not a bug in their
business. Failure is the business.

The door was a vendor upcharge. A lobby turnstile grew into a "secure access
program," and the program was signed off by the same committee that approved the
monitors. Same procurement, same deadline, same AI-assisted code generator, same
negligent vendor. Act II is not a new enemy. It is the same disease with a
deadbolt bolted to it.

Their security arm is still called FROSTLINE. You will not find it on an org
chart. It exists so that when a site is scrubbed, no one can point at a person.
It does not break into buildings. It makes the buildings believe they are safe.

<br>

## THE STAKES

Act I was a lie about temperature. Act II is a lie about people. Every badge, every
PIN entry, every request to exit lands in an access log that the site trusts. If
the log can be forged, nobody knows who was inside when the wipe began. If the
verdict can be flipped, the bolt opens for anyone with a debugger.

NIGHTINGALE is behind that door. The evidence that ends FROSTLINE is on the other
side of it. The gate is the last thing standing between a person and a
bulldozer, and the gate was built to trust the wrong thing.

That is not access control. That is a locked room with a vent, and the vent is a
firmware flag.

<br>

## NIGHTINGALE

She was a firmware engineer. Thirteen years. She wrote the bootloader they all
trust. In Act I she copied the shipping image to a dead drop and sent one message
to a crew that does not exist on paper. Us. **WHITEOUT**.

Then the messages stopped. The last packet was a frame from the monitor, and the
frame carried a location. The location is the annex. The annex has a door. The
door is this project.

She is still in there. The clock is running.

<br>

## THE MACHINE

The firmware in this repository is the door's firmware. On a breadboard it is a
toy: a Pico 2, an infrared badge reader that doubles as a PIN keypad, a 1602 LCD
access log, three LEDs, a request-to-exit button, an SG90 deadbolt servo, a
DHT11 vault interlock, and a radio.

Two things are open. **The radio** takes an order from anyone on the band, and it
carries the sealed authorization and audit uplink. **The infrared eye** takes a
badge and a PIN from anyone with a universal remote, and it is the primary input,
not a maintenance afterthought. Neither surface asks a hard question by itself.
And the servo will throw the deadbolt because a machine that keeps its verdict in
plain SRAM will obey whatever writes the verdict.

Then there is the face of the thing. The LEDs and the LCD report DENIED, PENDING,
and GRANTED with total confidence. The gate does not know the difference between
a real desk grant and a boolean a debugger set. It will show you green while it
lets you walk into a site that is being erased.

<br>

## THE JOB

You do not have to be a hero. You have to be honest. The door is lying about who
it let in. Make it tell the truth.

1. **Read the dead.** Take the image apart. Map the keypad, the display, the
   radio, the LEDs, the request-to-exit button, the servo, and the interlock.
2. **Find the doors.** Replay a captured grant. Forge an unlock. Flip the SRAM
   verdict with a debugger. Walk through request-to-exit.
3. **Lock them.** Seal every frame with **XChaCha20-Poly1305**, keyed through
   **Argon2id**, add a monotonic anti-replay window so a captured grant dies on
   second use, and tag the authorization record so a flipped boolean fails before
   the bolt moves.
4. **Fix the policy.** Make the gate fail secure, rate-limit the keypad, lock the
   debug port, and leave the life-safety egress exactly where it is, named and
   defended on purpose instead of by accident.

This document is the manual for the job. Work it on a breadboard. When the green
light lies to you, remember what it is: not a welcome. A witness.

Goodbye, friend.

<br>

## A NOTE ON THE ROADMAP

This capstone is Act II of **OPERATION COLD IRON**. Act I was the sensor
([cold-chain-monitor](https://github.com/mytechnotalent/cold-chain-monitor)). Act
2 is the door. Both are defended devices; the companion CTF repository ships the
compromised one. The investigation lives here:

- [OPERATION IRON GATE CTF](https://github.com/mytechnotalent/CTF_access-gate)

The CTF is the red half, weaponized: four deep defects, each with a static
analysis, a dynamic proof under GDB, a hardware demonstration, and an in-place,
same-size patch. This repository is the defended device. The CTF repository is
the breached one. The full story lives at
[github.com/mytechnotalent/access-gate](https://github.com/mytechnotalent/access-gate).

---


<br>

## WHERE THIS FITS: OPERATION COLD IRON

This repository is **Act II (IRON GATE)** of the ten-act OPERATION COLD IRON saga.
Acts I and II are the vulnerability-only foundation; the malware track begins at
Act III. The full spine is in [SAGA.md](SAGA.md).

- Previous act: Act I, COLD IRON, [cold-chain-monitor](https://github.com/mytechnotalent/cold-chain-monitor)
- This act: Act II, IRON GATE, the access gate
- Next act: Act III, IRON VEIN (pipeline-valve-controller, forthcoming)
- Companion CTF: [CTF_access-gate](https://github.com/mytechnotalent/CTF_access-gate)


<br>

## THE MINISTRY

The Ministry runs the state: the surveillance, the cold chain, the gates, the
pipelines. NorthPharma is one of its deniable industrial fronts, and FROSTLINE is
the contractor that does the work no Ministry letterhead will admit to. Against
them is WHITEOUT, and the engineer who copied this image, NIGHTINGALE. This act is
one node of the Ministry's industrial edge. TELESCREEN, the surveillance backbone
that watches it, comes after the ten.

An adversarial, evidence-based audit of this act, including its honest limitations, is in [NATION-STATE-REVIEW.md](NATION-STATE-REVIEW.md).


<br>

## How This Project Fits the Embedded Hacking Course

This repository is the Act II capstone integration for the
[Embedded Hacking](https://github.com/mytechnotalent/Embedded-Hacking) course. It
reuses the entire Act I peripheral set so one breadboard serves both chapters, and
it adds the stateful security concepts the course builds toward: authorization
versus authentication, anti-replay, state integrity, debug-port trust, and
fail-secure policy.

Each earlier module teaches one peripheral or language concept in isolation; this
project wires several of them into a single, tested product and adds the wireless
and stateful security layers on top.

| Embedded Hacking module | Concept you learn | Where it lives here |
| ----------------------- | ----------------- | ------------------- |
| Week 1: Introduction, Ethics, Scoping | Authorized lab work | Every lab is self-contained and authorized by design |
| Week 3: RP2350 Architecture and Firmware Analysis | Bare-metal targets, ELF/UF2, SWD | Pico SDK build, `build/*.uf2`, Debug Probe flash via OpenOCD |
| Weeks 4-6: Variables, Integers/Floats, Static | Data types, GPIO | `src/monitor.c` state machine, LED on GP25 |
| Week 7: Constants with 1602 LCD I2C | I2C bus, HD44780 commands | `src/display.c` |
| Week 9: Operators with DHT11 | Bit operations, edge timing | `src/sensor.c` vault interlock |
| Week 11: Structures and Functions | Modular design | `include/*.h` and `src/*.c` module boundaries |
| This project adds | UART AT driver, LoRa desk link, stateful auth, anti-replay, authenticated state, debug lockdown, fail-secure policy, strict testing | `src/radio.c`, `src/auth.c`, `src/keypad.c`, `scripts/gateway.py`, `scripts/spoof.py`, `scripts/gen_packet.py`, `test/` |

If you have not worked through Weeks 7 and 9 yet, do those first: this project
assumes you are comfortable with I2C wiring and one-wire edge timing.

<br>

## Learning Objectives

By the end of this chapter and its labs you will be able to:

- Explain why physical access control is an authorization problem, not just an
  authentication problem, and why proving who someone is does not prove what they
  are allowed to do.
- Wire and drive a 1602 LCD through a PCF8574 I2C backpack and render an access
  log with the last badge, sequence, and verdict.
- Decode a VS1838B infrared receiver as both a badge reader and a NEC PIN
  keypad, including the digit, ENTER, and CLEAR command codes and the
  ready-before-consume guard on the accumulator.
- Drive an SG90 deadbolt with 50 Hz PWM and explain why a 1000uF bulk capacitor
  is not optional.
- Read a DHT11 vault interlock and enforce a safe environmental band before the
  bolt is allowed to move.
- Design a sealed request and grant exchange over a sub-GHz LoRa link using a
  fixed-size envelope and the declared-length slicing invariant.
- Distinguish authentication from authorization, and demonstrate both: a forged
  grant that fails the tag, and a captured grant that passes the tag but fails
  the anti-replay window.
- Explain state integrity and TOCTOU, and demonstrate with GDB that flipping the
  SRAM verdict boolean without recomputing the state tag is rejected before the
  deadbolt moves.
- Apply blue-half controls: a monotonic sequence window, an authenticated state
  tag, debug-port lockdown, keypad rate limiting and lockout, and a fail-secure
  policy.
- Reason about designed egress: why request-to-exit is deliberately
  unauthenticated for life safety, and why the correct response is documentation
  and defense in depth rather than "fixing" it closed.
- Derive a key with Argon2id, seal every frame with XChaCha20-Poly1305, and read
  and run a native host test suite with hardware mocks and line coverage.

<br>

## Prerequisites

- The [Embedded Hacking](https://github.com/mytechnotalent/Embedded-Hacking)
  breadboard (`EHP2_bb.png`) and parts list.
- Act I is helpful but not required. See
  [cold-chain-monitor](https://github.com/mytechnotalent/cold-chain-monitor) for
  the sensor and telemetry chapter. The pin map is identical, so one breadboard
  serves both.
- Comfort with C, the Linux/macOS shell, and basic electronics.
- A Pico 2, a Debug Probe (recommended), a 1602 LCD with PCF8574 backpack, a
  DHT11, the full Embedded Hacking kit (3 LEDs, 3 resistors, a push button, an
  SG90 servo, a 1000uF capacitor, and a VS1838B infrared receiver plus NEC
  remote), two RYLR998 modules, and one USB-to-TTL serial adapter.
- Toolchain: Pico SDK 2.2.0+, `arm-none-eabi-gcc`, CMake, Ninja, Python 3, GDB
  (`arm-none-eabi-gdb`) for Lab 3, and (optionally) `typst` to rebuild the paper.

<br>

## Table of Contents

1. [Background](#background)
2. [System Architecture](#system-architecture)
3. [The Wire Protocol](#the-wire-protocol)
4. [The Cryptographic Envelope](#the-cryptographic-envelope)
5. [Hardware You Need](#hardware-you-need)
6. [Wiring the Node](#wiring-the-node)
7. [Build and Flash](#build-and-flash)
8. [Lab 1: Bring-Up and Verify](#lab-1-bring-up-and-verify)
9. [Lab 2: Inspect the Wire Protocol](#lab-2-inspect-the-wire-protocol)
10. [Lab 3: The Red Half](#lab-3-the-red-half)
11. [Lab 4: The Blue Half](#lab-4-the-blue-half)
12. [Troubleshooting](#troubleshooting)
13. [Testing Philosophy and Coverage](#testing-philosophy-and-coverage)
14. [Generating Packet Artifacts](#generating-packet-artifacts)
15. [Code Standards](#code-standards)
16. [Project Layout](#project-layout)
17. [Glossary](#glossary)
18. [Further Reading](#further-reading)
19. [License](#license)

<br>

## Background

### Why physical access control

A door is a decision. Someone presents a badge or a PIN, the door decides whether
that person may pass, and a bolt moves. Everything interesting in access control
happens in that decision and in the record it leaves behind. Two separate
questions hide inside it:

- **Authentication.** Who is this? A badge number, a PIN, a cryptographic key.
- **Authorization.** Are they allowed, right now, in this state, to pass?

The classic failure of naive systems is to collapse the two. If a PIN equals a
stored value, open. But a PIN is a shared secret that leaks, that gets written on
the back of a badge, and that says nothing about whether the holder should be
inside the vault at three in the morning. Act II splits the questions on purpose: a
sealed request goes to a security desk, and the desk decides. The desk is the
authorization authority. The gate is the enforcement point.

### Why replay and state integrity matter

Even after every frame is authenticated with strong cryptography, two attacks
remain, and they are the heart of Act II:

- **Replay.** A perfectly valid, authentically sealed GRANTED frame is captured
  off the air and sent again later. Cryptography is happy: the tag is correct.
  Only state, a monotonic counter, can tell the gate that this grant was already
  used.
- **State tampering.** The gate decides with a boolean in SRAM, call it
  `granted`. An attacker with a debug probe and a GDB session does not break the
  cipher; they set `granted = 1` and let the bolt move. The wire is sealed; the
  verdict is not.

This is the TOCTOU lesson in embedded form: the cryptographically strong decision
happens, then a plain, mutable piece of memory is trusted afterward. The fix is
not more cipher. It is to authenticate the state itself, so a modified verdict no
longer matches the tag computed over the record.

### Why ChaCha20 over AES on the RP2350

The RP2350 has no hardware AES engine; its accelerated crypto block covers
SHA-256, not AES. A software AES implementation on this part is therefore both
slower and riskier, because table-driven AES performs data-dependent memory
accesses that create a cache-timing side channel. ChaCha20 is built only from
addition, rotation, and XOR, with no data-dependent table lookups, so it is fast
in portable C and has no comparable cache-timing surface. XChaCha20-Poly1305 is
thus both the modern choice and the pragmatic one for this silicon. The full
rationale, including the extended-nonce benefit, appears in
[The Cryptographic Envelope](#the-cryptographic-envelope).

### The two on-wire problems this project solves

1. **Payloads that contain commas.** The sealed body is carried as lowercase hex,
   but the `+RCV` framing still separates fields with commas. A naive receiver
   that splits the line on the first comma corrupts the frame. The correct
   discipline is the **declared-length** rule: slice exactly `L` characters after
   the second comma and require the next character to be a comma.
2. **Telling a real grant from a forged or replayed one.** The gate records the
   sender address exactly as the radio reports it, and it trusts the bytes that
   arrive. The sealed envelope plus the stateful window are what close that gap.

### Inter-Integrated Circuit (I2C)

I2C is a two-wire bus: **SDA** (data) and **SCL** (clock), each pulled up to the
supply rail. A controller (the Pico) addresses a target by its 7-bit address and
writes or reads bytes. The 1602 LCD backpack carries a **PCF8574** I/O expander
at address `0x27`; the firmware bit-bangs the HD44780 nibble protocol over that
expander. Pull-ups are mandatory: the firmware enables the internal ones and the
backpack usually adds its own.

### The DHT11 one-wire protocol

The DHT11 is a low-cost digital temperature and humidity sensor. In Act II it is
the **vault environmental interlock**: the deadbolt will not release unless the
vault is inside a safe temperature band. It speaks a custom single-wire protocol:

1. The host pulls the line low for at least 18 ms (the **start pulse**), then
   releases it and enables its pull-up.
2. The sensor answers with an 80 us low, then an 80 us high handshake.
3. The sensor sends **40 bits**. Each bit begins with a 50 us low, then a high
   pulse whose width encodes the value: about 26-28 us for a `0`, about 70 us
   for a `1`.
4. Five bytes follow: humidity integer, humidity decimal, temperature integer,
   temperature decimal, and a checksum equal to the low byte of their sum.

Reading it means timing edges on the order of tens of microseconds, so the
firmware uses an 18 ms host pulse, a 50 us bit-classification threshold, and a
240 us per-edge timeout so a dead or unplugged sensor fails fast instead of
hanging the loop. A reading that fails its checksum is never "safe", and a valid
reading outside **-5.0 C to 10.0 C** (the tenths band `-50` to `100`) is out of
band. Either way, the interlock denies the release.

### Universal Asynchronous Receiver/Transmitter (UART) and AT commands

The RYLR998 is driven over a UART at 115200 baud using CRLF-terminated ASCII
commands. The firmware writes `AT+SEND=...` and drains inbound `+RCV=...` lines.
Because the radio is a separate processor, its configuration (address, network
identifier, band) persists until changed; the gate and the security desk each
provision their own radio at start-up so they agree before any authorization
traffic flows.

### Cyclic Redundancy Check (CRC)

`src/crc.c` implements CRC-16/CCITT-FALSE (`poly = 0x1021`, `init = 0xFFFF`,
check value `0x29B1` for `"123456789"`). It is provided as a reusable integrity
diagnostic and exercised by the test suite. It is **not** part of the LoRa frame
in this project; the lesson is the *absence* of authentication, not the absence
of a checksum.

<br>

## System Architecture

There are four roles:

| Role | Runs on | Job |
| ---- | ------- | --- |
| **Gate node** | Pico 2 firmware | Decodes the IR badge and PIN keypad, seals an unlock request to the desk, announces PENDING, verifies the sealed grant, drives the deadbolt, enforces the interlock, and logs state |
| **Security desk** | laptop + USB-TTL radio | Authenticates every request, logs it to `access_log.csv`, decides authorization, and answers with a sealed grant carrying a sequence and a state tag (`scripts/gateway.py`) |
| **Edge simulator** | laptop + USB-TTL radio | Pretends to be a second gate node and sends sealed requests (`scripts/sim_edge.py`) |
| **Attacker** | laptop + USB-TTL radio | Impersonates the desk, forges a grant, or replays a captured grant (`scripts/spoof.py`) |

### Data flow

```text
+----------------------+                              +----------------------+
|   Pico 2 gate node   |        LoRa (sub-GHz)        |   Security desk      |
|   IR keypad -> GP5   |  AT+SEND=0001,<len>,<hex>    |  USB-TTL radio       |
|   DHT11     -> GP4   |----------------------------->|  scripts/gateway.py  |
|   Servo     -> GP14  |<-----------------------------|  access_log.csv      |
|   LCD    -> GP2/GP3  |  AT+SEND=<gate>,<len>,<hex>  |  sealed grant reply  |
+----------------------+                              +----------------------+

+----------------------+                              +----------------------+
|   Attacker laptop    |  forged or replayed grant    |   (same gate)        |
|   scripts/spoof.py   |----------------------------->|   rejects at the tag |
|  claims the desk     |                              |   tag or seq window  |
+----------------------+                              +----------------------+
```

### Firmware module map

| File | Responsibility |
| ---- | -------------- |
| `src/main.c` | Entry point: `stdio_init_all`, `monitor_init`, tick loop |
| `src/monitor.c` | State machine: I2C bus scan, keypad, desk request, grant verification, deadbolt, interlock, access log |
| `src/keypad.c` | NEC command to PIN accumulator with a bounded count and ready-before-consume guard |
| `src/auth.c` | Authorization record, monotonic anti-replay window, authenticated state tag |
| `src/sensor.c` | DHT11 one-wire sampling and vault interlock classifier |
| `src/display.c` | HD44780 driver over the PCF8574 backpack and access-log rendering |
| `src/radio.c` | RYLR998 provisioning, `AT+SEND` builder, `+RCV` parser, line pump |
| `src/status_led.c` | Red/yellow/green DENIED/PENDING/GRANTED annunciator |
| `src/button.c` | Debounced request-to-exit button around the internal pull-up |
| `src/servo.c` | 50 Hz PWM deadbolt actuator |
| `src/ir_remote.c` | VS1838B edge timing and NEC remote decode |
| `src/chacha20.c` | ChaCha20 stream cipher and HChaCha20 subkey derivation |
| `src/poly1305.c` | Poly1305 one-time message authenticator |
| `src/crypto_aead.c` | XChaCha20-Poly1305 seal/open envelope |
| `src/blake2b.c` | BLAKE2b and the Argon2 variable-length hash H' |
| `src/argon2.c` | Argon2id core (BLAMKA, hybrid addressing) |
| `src/crypto_kdf.c` | Argon2id passphrase key derivation |
| `src/envelope.c` | Hex nonce/ciphertext/tag envelope codec |
| `src/crc.c` | CRC-16/CCITT-FALSE diagnostic |
| `include/access_gate.h` | Pin map, bus, and provisioning constants |
| `include/auth.h` | Authorization record, window, and state tag |
| `include/keypad.h` | PIN accumulator interface |

<br>

## The Wire Protocol

### Request frame

The gate seals the entered PIN into an authenticated envelope and sends it to the
security desk:

```text
AT+SEND=0001,<len>,<hex envelope>
```

The plaintext of a request is the PIN text itself, for example `482190` for a
six-digit PIN. Six digits is the bounded accumulator length
(`ACCESS_GATE_PIN_LENGTH`, `PACKET_PIN_LENGTH`); the keypad refuses a seventh and
refuses any value above nine.

### Grant frame

The desk answers an authenticated request with a sealed grant. The grant
plaintext is a 20-byte body:

```text
seq[4] (little-endian) || state_tag[16]
```

- `seq` is the monotonic desk sequence number.
- `state_tag` is an XChaCha20-Poly1305 tag over the authorization record the
  grant would produce, so the gate can verify that the verdict it is about to
  store is the one the desk authorized.

The desk sends it back to the claimed sender address:

```text
AT+SEND=<gate>,<len>,<hex envelope>
```

### Sealed envelope layout

Every payload on the wire is the lowercase hexadecimal encoding of:

```text
nonce[24] || ciphertext[L] || tag[16]
```

For a six-digit request body this is 24 + 6 + 16 = 46 bytes, or 92 hex
characters. For a 20-byte grant body this is 24 + 20 + 16 = 60 bytes, or 120 hex
characters. The declared length `L` in the `AT+SEND` and `+RCV` framing is the
length of the hex string, not of the underlying plaintext.

The maximum accepted plaintext is 48 bytes (`ENVELOPE_MAX_PLAINTEXT`), and the
maximum hex envelope buffer is `(24 + 48 + 16) * 2 + 1 = 177` bytes
(`ENVELOPE_MAX_HEX_LEN`), which fits the 256-byte radio command and receive
buffers with framing headroom.

### Declared-length slicing invariant

Given the substring `T` after the second comma:

```text
C = T[0 : L]   and   T[L] == ","
```

The receiver checks `T[L] == ","`, so a mismatch between the declared length and
the actual payload is a parse error rather than silent corruption. This is what
makes hex-bearing payloads safe to carry and is the same invariant Act I uses.

### LCD access log

```text
PIN:482190 S:0007
STATE:PENDING
```

Line 1 is the last badge PIN and the most recent accepted sequence number. Line 2
is the current gate state: `OFF`, `DENIED`, `PENDING`, or `GRANTED`. Exactly one
status LED is lit at a time to match.

### Radio provisioning

For the link to work, both radios must share the same **network identifier** and
each must have the address the other targets:

- Firmware sets its own radio: `AT+ADDRESS=7`, `AT+NETWORKID=18`.
- `gateway.py` sets the desk radio: `AT+ADDRESS=1`, `AT+NETWORKID=18`.

Both radios must also be the **same band variant** (for example 915 MHz or
868 MHz); band and RF parameters are left at factory defaults, so use matching
modules.

### Timing

| Quantity | Value |
| -------- | ----- |
| Authorization wait (`AUTH_WAIT_MS`) | 5000 ms |
| Deadbolt release hold (`MONITOR_HOLD_MS`) | 5000 ms |
| Request-to-exit debounce | 30000 us |
| DHT11 host start pulse | 18000 us |
| DHT11 bit threshold | 50 us |
| DHT11 per-edge timeout | 240 us |
| LCD I2C clock | 100000 Hz |
| Radio UART baud | 115200 |
| PIN length | 6 digits |
| Interlock safe band | -50 to 100 tenths (-5.0 C to 10.0 C) |
| Servo locked pulse | 500 us |
| Servo open pulse | 1500 us |
| Servo PWM period | 20000 us (50 Hz) |

<br>

## The Cryptographic Envelope

The radio is the first open door, and it is the one a key can close. The fix is
authenticated encryption: every request and every grant is sealed so a forged
frame dies at the authentication tag instead of moving the deadbolt. The full
implementation lives in `src/chacha20.c`, `src/poly1305.c`, and
`src/crypto_aead.c`, and every primitive is checked against its published test
vectors in the native suite.

### Why XChaCha20-Poly1305

- **256-bit key, 192-bit nonce.** The extended nonce means nonces can be drawn at
  random forever, so the gate never needs a shared counter that a reboot could
  reuse.
- **AEAD in one pass.** Confidentiality and integrity come from one operation;
  the associated data (the gate node id, byte `0x07`) is authenticated even
  though it is not encrypted.
- **Constant-time software.** ChaCha20 has no data-dependent table lookups, so it
  has no cache-timing surface. The RP2350 has no hardware AES engine (it
  accelerates SHA-256 only), which makes software AES both slower and riskier on
  this silicon.
- **128-bit Poly1305 tag.** Guessing a valid tag succeeds with probability
  2^-128.

### Why Argon2id

A passphrase is not a key. Argon2id (RFC 9106) is the memory-hard password hash:
it mixes the passphrase with a salt across memory and time so an attacker cannot
cheaply recover the field passphrase from a captured image. The classroom profile
is `t=3`, `p=1`, `m=64` blocks (`CRYPTO_KDF_TIME_COST`,
`CRYPTO_KDF_PARALLELISM`, `CRYPTO_KDF_MEMORY_BLOCKS`) to fit the RP2350 SRAM
budget. Raise it on the security desk. The lab salt is the 16 ASCII bytes
`coldiron-salt-01`.

### Key model: one field key plus a desk key

Act II introduces a two-role key model, and the lifecycle is the lesson:

- **Field key.** Derived with Argon2id and used to seal every frame on the wire,
  both the gate request and the desk grant.
- **Desk key.** The authorization authority. It is the key under which the desk
  computes the state tag over the authorization record, so the gate can tell a
  real grant from a boolean a debugger wrote into SRAM.

In the classroom build the field key and the desk role are both derived from the
same committed lab passphrase and salt, so the firmware and the security desk
interoperate with no provisioning step. That is a lab convenience, not a
deployment. The design names the two roles separately so students can reason
about the real lifecycle: derive, provision per device, use, rotate on a
schedule, and retire. A production build provisions key material from
one-time-programmable (OTP) memory, keeps the desk key off the field device, and
rotates the field key without reflashing every gate.

### Envelope layout

The sealed frame is carried as hex inside the `AT+SEND` payload:

```text
nonce[24] || ciphertext[L] || tag[16]
```

The receiver recomputes the Poly1305 tag over the associated data and ciphertext,
compares it in constant time, and only then decrypts. This envelope is wired end
to end: `src/monitor.c` seals the request with `src/envelope.c`, the desk
authenticates before it parses or acts, and the gate opens the reply only after
the tag verifies. Authenticated frames carry the gate node id as associated data,
so a frame sealed for one node cannot be relabeled for another.

### Act II additions: anti-replay and authenticated state

Strong AEAD is necessary and not sufficient. Two stateful controls sit on top:

- **Anti-replay sequence window.** `src/auth.c` keeps `last_seq`, the highest
  sequence number ever accepted. `auth_apply_grant` accepts a grant only when its
  sequence is strictly greater than `last_seq`. A captured GRANTED frame, even a
  perfectly valid one, is rejected on second use.
- **Authenticated state tag.** The authorization record is ten bytes: `granted`,
  `pending`, `seq[4]`, `last_seq[4]`. The tag is an XChaCha20-Poly1305 tag over
  that record, computed under the field key with a deterministic nonce built from
  the sequence number and the domain byte `0xA6`. `auth_state_ok` recomputes the
  tag and compares it in constant time before the bolt is allowed to move. A
  debugger that sets `granted = 1` without recomputing the tag fails here first.

The sequence window and the state tag are independent. The window stops a valid
grant from working twice; the tag stops an unauthorized verdict from existing at
all.

<br>

## Hardware You Need

Full parts list with links: [PARTS.md](PARTS.md).

| Qty | Part | Notes |
| --- | ---- | ----- |
| 1 | Raspberry Pi Pico 2 (RP2350) with headers | The gate node |
| 1 | Raspberry Pi Debug Probe | SWD flashing, UART0 console, and the Lab 3 SRAM attack (recommended) |
| 1 | Full-size breadboard | |
| 1 | Assorted jumper wires | |
| 1 | 1602 LCD with PCF8574 I2C backpack | Access log, address `0x27` |
| 1 | DHT11 temperature/humidity sensor | Vault environmental interlock |
| 1 | 10K resistor | Only if your DHT11 has no onboard pull-up |
| 3 | 5mm LEDs (red, yellow, green) | DENIED, PENDING, GRANTED annunciator |
| 3 | 100, 220, or 330 Ohm resistors | One per LED |
| 1 | Push button (tactile switch) | Request-to-exit, active low |
| 1 | SG90 servo motor | Deadbolt actuator |
| 1 | 1000uF 25V capacitor | Bulk decoupling on the servo 5V rail |
| 1 | VS1838B infrared receiver | Badge reader and PIN keypad input |
| 1 | NEC-compatible infrared remote | Badge and PIN keypad trigger |
| 3 | RYLR998 LoRa modules with antennas | 2 for the authorization loop, 3 for the live attack lab |
| 2 | USB-to-TTL serial adapters (FTDI FT232, CP2102, or CH340), 3.3V logic | 1 for the desk, 1 for the attacker in the live lab |
| 4 | USB cables | Pico 2, Debug Probe, and serial adapter(s) |

### How many radios do you actually need?

| Goal | Radios | What is connected |
| ---- | ------ | ----------------- |
| Legitimate authorization loop (Labs 1-2) | **2** | 1x RYLR998 on the Pico (UART1) + 1x RYLR998 on a USB-to-TTL adapter (the desk) |
| Live attack lab (Labs 3-4, watch a grant land and fail) | **3** | the 2 above + 1x RYLR998 on a second USB-to-TTL adapter (the attacker) |
| Attack concept with no extra hardware | 2 or 0 | read-and-run the offline parser demo, or the unit tests |

A radio never receives its own transmission, and the desk radio is busy listening
as `gateway.py`, so the live attack needs a separate attacker radio. The 2-radio
kit runs the whole legitimate system; only the live attack observation needs the
third.

> Serial adapter warning: the RYLR998 is **not** 5V tolerant. Use a
> **3.3V-logic** USB-to-TTL adapter (or set its jumper to 3.3V).

<br>

### How each part works

Every part in the bill of materials does one physical job and one job in this act:

| Part | How it works | Role in this act |
| ---- | ------------ | ---------------- |
| 1x Full-size breadboard (long) | The two columns of spring-clip tie points sit on a 0.1 inch grid, so every hole in a row is bridged by a metal clip, while the two outer power rails run the full length and distribute power and ground to the whole build. | It is the substrate that holds the Pico 2, LCD, sensor, servo, and radio headers and distributes 3.3V and 5V across the gate. |
| 1x Assorted jumper wires (male-to-male, male-to-female, female-to-female) | Male pins push into breadboard tie points or female headers, female sockets slide over the Pico 2, LCD, and servo header pins, and male-to-female leads bridge a breadboard row to a module header. | They carry power and the I2C, one-wire, UART, PWM, IR, and GPIO signals between the boards. |
| 1x Raspberry Pi Pico 2 with header | The RP2350 pairs two Arm Cortex-M33 cores with 3.3V logic and a GPIO block that exposes ADC, I2C, UART, and PWM, plus the onboard GP25 LED. | It is the gate controller that accumulates the IR PIN, checks the vault interlock, drives the deadbolt servo and annunciator, and runs the sealed request and grant protocol. |
| 1x Raspberry Pi Pico Debug Probe | It drives the two-wire SWD port (SWCLK and SWDIO) to flash and single-step the target, and it also presents a USB UART bridge for the serial console. | It flashes and debugs the gate and is the instrument for the Lab 3 SRAM verdict attack and the fix. |
| 2x USB A-male to USB micro-B cables | USB carries 5V power and a data channel on the same cable, so one cable powers the Pico 2 and presents its USB CDC console while the other powers the Debug Probe and carries its SWD and UART traffic. | They power the two boards and carry the console and debug links. |
| 3x 5mm LEDs (1 red, 1 green, 1 yellow) | An LED is a diode with a forward voltage drop of roughly 2V, so current flows only from the anode to the cathode, and a GPIO pin set high sources that current and lights the lamp. | They are the red DENIED, yellow PENDING, and green GRANTED access annunciator, and exactly one is lit per state. |
| 3x 100, 220, or 330 Ohm resistors | A resistor in series with each LED sets the current by Ohm's law, I equals (supply minus forward voltage) divided by resistance, which protects the LED and keeps the GPIO within its current limit. | One resistor per lamp limits the LED current on each of the three annunciator pins. |
| 1x Push button (tactile switch) | The switch shorts its input pin to ground when pressed, and the RP2350 internal pull-up holds that pin high at rest so the press reads as active low. | It is the request-to-exit button, the deliberate life-safety egress path that opens the deadbolt without a badge or PIN. |
| 1x 1602 LCD with PCF8574 I2C backpack | The HD44780 controller takes a 4-bit nibble protocol with register-select and enable strobes, and the PCF8574 I2C expander latches those eight control lines so the whole display is driven over two I2C wires at address 0x27. | It renders the access log readout. |
| 1x DHT11 temperature and humidity sensor | The host pulls the single data line low for a start pulse, then the sensor answers with 40 bits of humidity, temperature, and checksum timed by pulse widths, and the checksum must match. | It is the vault environmental interlock that denies a release when the reading fails or leaves the -5.0 C to 10.0 C band. |
| 1x SG90 servo motor | The servo expects a 50 Hz PWM signal whose high pulse of 1 to 2 ms selects the shaft angle, and a Pico PWM slice generates that pulse train. | It is the deadbolt actuator, locked at 0 degrees and open at 90 degrees. |
| 1x 1000uF 25V capacitor | Wired across the servo 5V rail and ground, the capacitor is a bulk reservoir that supplies the motor inrush current and smooths the rail while the servo starts. | It keeps the deadbolt move from browning out the RP2350 and resetting the gate. |
| 1x Infrared (IR) receiver (VS1838B) | The receiver pairs a photodiode with a 38 kHz bandpass demodulator that ignores ambient light and outputs an active-low logic pulse for each IR burst. | It is the primary PIN input that feeds digits and ENTER and CLEAR into the accumulator on GP5. |
| 1x Infrared (IR) remote controller (NEC-compatible) | The remote emits its bursts modulated at 38 kHz in the NEC frame, a 9 ms leader followed by 32 bits where the address and command are each sent with their bitwise complements for validation. | It is the badge and PIN keypad that sends digits 0x16 to 0x4A, ENTER 0x46, and CLEAR 0x45. |
| 1x RYLR998 LoRa radio module | The module is configured and driven over UART with AT commands and carries sub-GHz LoRa packets, and its logic pins are 3.3V only so the radio must never see 5V. | It is the UART1 request and grant link that carries the sealed frames between the gate and the desk, and it is not 5V tolerant. |

<br>

## Wiring the Node

### Pin map

This is the authoritative map; it is identical to Act I and is defined in
`include/access_gate.h` and enforced by the test suite.

| Peripheral | Signal | Pico 2 GPIO |
| ---------- | ------ | ----------- |
| DHT11 vault interlock | DATA (one-wire) | **GP4** |
| 1602 LCD (PCF8574) | SDA (I2C1) | **GP2** |
| 1602 LCD (PCF8574) | SCL (I2C1) | **GP3** |
| RYLR998 | RX <- Pico TX (UART1) | **GP8** |
| RYLR998 | TX -> Pico RX (UART1) | **GP9** |
| Infrared receiver | OUT (VS1838B) | **GP5** |
| Deadbolt servo | PWM signal | **GP14** |
| Red DENIED LED | anode | **GP16** |
| Yellow PENDING LED | anode | **GP17** |
| Green GRANTED LED | anode | **GP18** |
| Request-to-exit button | to ground | **GP15** |
| Onboard LED | heartbeat | GP25 |
| Debug Probe / UART0 console | TX | GP0 |
| Debug Probe / UART0 console | RX | GP1 |

> Note: GPIO 2/3 are the classic I2C1 pins used throughout the Embedded Hacking
> breadboard; this project's map matches that board because it is the same board.

### 1602 LCD with I2C backpack

| LCD backpack | Pico 2 |
| ------------ | ------ |
| VCC | 3.3V |
| GND | GND |
| SDA | GP2 |
| SCL | GP3 |

### DHT11 vault interlock

| DHT11 | Pico 2 |
| ----- | ------ |
| VCC | 3.3V |
| DATA | GP4 |
| GND | GND |

If your DHT11 has no onboard pull-up, add a **10K resistor between DATA and
3.3V**. The firmware also enables the internal pull-up, but the external resistor
makes reads far more reliable over jumper wires. The interlock denies the release
when the sensor fails or reads outside `-5.0 C` to `10.0 C`.

### Status LEDs

| LED | Pico 2 | Series resistor |
| --- | ------ | --------------- |
| Red (DENIED) | GP16 (anode) | 220-330 Ohm to GND |
| Yellow (PENDING) | GP17 (anode) | 220-330 Ohm to GND |
| Green (GRANTED) | GP18 (anode) | 220-330 Ohm to GND |

Exactly one lamp is lit at a time. Red is a denied or failed authorization,
yellow is a request awaiting the desk, and green is an open deadbolt.

**LED behavior**

| State | Lamp | Indication | Meaning |
| ----- | ---- | ---------- | ------- |
| `STATUS_LED_OFF` | none | All dark | Deadbolt is locked and no authorization is in flight. |
| `STATUS_LED_DENIED` | Red | Solid | Authorization was denied or failed, the interlock tripped, or the pending wait expired. |
| `STATUS_LED_PENDING` | Yellow | Solid | A sealed unlock request is awaiting the desk grant. |
| `STATUS_LED_GRANTED` | Green | Solid | The deadbolt is released for the bounded hold. |

The three annunciator lamps are always solid; `status_led.c` never blinks them.
Exactly one lamp is lit at a time, and `STATUS_LED_OFF` leaves all three dark. A
pending request that times out falls back to `STATUS_LED_DENIED`, and a grant that
expires falls back to `STATUS_LED_OFF`. The onboard GP25 LED is initialized as an
output and driven as a heartbeat: `monitor.c` toggles it every four monitor ticks
in `monitor_heartbeat`, so a running gate is visible even while idle.

### Request-to-exit button

| Button | Pico 2 |
| ------ | ------ |
| Leg 1 | GP15 |
| Leg 2 | GND |

The firmware enables the internal pull-up, so **do not** connect 3.3V to the
button. This is the deliberate, unauthenticated life-safety egress path: a press
consumes one debounced edge, checks the interlock, and opens the deadbolt without
a badge, a PIN, or a desk grant. Lab 4 explains why that is correct and how to
contain it rather than close it.

### SG90 deadbolt servo

| Servo | Pico 2 |
| ----- | ------ |
| Signal (orange) | GP14 |
| VCC (red) | 5V (VBUS) |
| GND (brown) | GND |

Solder the **1000uF capacitor** across the servo 5V and GND rails to absorb the
inrush current; without it the RP2350 can brown out when the deadbolt moves.
Locked is 0 degrees and open is 90 degrees.

### Infrared receiver

| VS1838B | Pico 2 |
| ------ | ------ |
| OUT | GP5 |
| VCC | 3.3V |
| GND | GND |

Point any NEC-compatible remote at the receiver. In Act II this is the primary
input, not a maintenance extra: the firmware decodes digit commands `0x16`
through `0x4A`, an ENTER command `0x46`, and a CLEAR command `0x45`, and feeds
them into the PIN accumulator. There is no challenge and no secret on the
optical surface, which is exactly the point of Lab 3.

**Using the remote**

Point the NEC remote at the VS1838B receiver on **GP5** and press a mapped button;
the receiver idles high and pulls low on a mark. Every valid frame prints
`IR <NAME> (0xNN)` on the console and is fed straight into the PIN accumulator.

| NEC command | Name | Action |
| ----------- | ---- | ------ |
| `0x16` | `MONITOR_IR_DIGIT_0` | Pushes digit `0` onto the PIN accumulator. |
| `0x0C` | digit | Pushes digit `1` onto the PIN accumulator. |
| `0x18` | digit | Pushes digit `2` onto the PIN accumulator. |
| `0x5E` | digit | Pushes digit `3` onto the PIN accumulator. |
| `0x08` | digit | Pushes digit `4` onto the PIN accumulator. |
| `0x1C` | digit | Pushes digit `5` onto the PIN accumulator. |
| `0x5A` | digit | Pushes digit `6` onto the PIN accumulator. |
| `0x42` | digit | Pushes digit `7` onto the PIN accumulator. |
| `0x52` | digit | Pushes digit `8` onto the PIN accumulator. |
| `0x4A` | `MONITOR_IR_DIGIT_9` | Pushes digit `9` onto the PIN accumulator. |
| `0x46` | `MONITOR_IR_ENTER_COMMAND` | Latches a full-length PIN and sends a sealed unlock request to the desk; the annunciator shows PENDING. |
| `0x45` | `MONITOR_IR_CLEAR_COMMAND` | Clears the PIN accumulator without sending a request. |

A digit command beyond `0x4A` is discarded, and ENTER is ignored until exactly
`ACCESS_GATE_PIN_LENGTH` digits are held.

### RYLR998 LoRa radio

> The RYLR998 must be powered. Forgetting **VDD** is the single most common
> reason the link appears dead: the firmware prints while the radio sits silent.

| RYLR998 | Pico 2 |
| ------- | ------ |
| VDD | 3.3V |
| GND | GND |
| RXD | GP8 (Pico UART1 TX) |
| TXD | GP9 (Pico UART1 RX) |

Attach the antenna before transmitting. TX and RX are **crossed**: the radio's
RXD is the Pico's TX and vice versa.

### Debug Probe (recommended)

| Debug Probe | Pico 2 |
| ----------- | ------ |
| SWCLK | SWCLK (3-pin debug header) |
| SWDIO | SWDIO |
| GND | GND |
| UART TX | GP1 (Pico RX) |
| UART RX | GP0 (Pico TX) |
| GND | GND |

The firmware enables stdio on **both** UART0 (`115200`) and USB, so you can
watch boot output on the probe's console or on the Pico's own USB serial port.
The Debug Probe is also the instrument for the Lab 3 SRAM verdict attack: the
probe that fixes the firmware is the probe that breaks it if the verdict is not
authenticated.

### Peripherals used

Every part in the Act II bill of materials is exercised by the firmware:

| Peripheral | Role | Where it is used |
| ---------- | ---- | ---------------- |
| Red, yellow, green LEDs | Tri-color access-gate annunciator | `status_led.c` drives exactly one lamp per state |
| Request-to-exit button (GP15) | Life-safety egress | `button.c` consumes one debounced press in `monitor_handle_rex` |
| 1602 I2C LCD | Access log readout | `display.c` renders the formatted lines over I2C1 |
| DHT11 (GP4) | Vault environmental interlock | `sensor.c` reads the one-wire frame and gates the release |
| SG90 servo (GP14) | Deadbolt actuator | `servo.c` locks and unlocks the deadbolt |
| 1000uF capacitor | Bulk decoupling on the servo 5V rail | Required to keep the RP2350 from browning out on servo moves |
| VS1838B IR receiver (GP5) | Primary PIN input | `ir_remote.c` captures and decodes the frame |
| NEC IR remote | PIN keypad and ENTER/CLEAR | Sends digits `0x16` to `0x4A`, ENTER `0x46`, and CLEAR `0x45` |
| RYLR998 (UART1) | LoRa request and grant link | `radio.c` sends the sealed request and pumps inbound grants |
| Onboard GP25 LED | Onboard heartbeat | `monitor.c` toggles it every four monitor ticks in `monitor_heartbeat` |
| Debug Probe | SWD flashing and UART0 console | `stdio` is enabled on both UART0 `115200` and USB |

Every Act II peripheral in the bill of materials is exercised by the firmware.

### How the functionality works

Every input feeds the state machine in `monitor_step`, every output is driven
once per tick, and the interactive console mirrors both over UART0 and USB. This
is what each surface does at run time.

**Inputs**

| Input | What it does when you use it |
| ----- | ---------------------------- |
| Infrared remote (GP5) | A decoded NEC frame prints `IR <NAME> (0xNN)` and is applied: digits `0x16` to `0x4A` push onto the PIN accumulator, `0x46` latches a full PIN and sends a sealed request, and `0x45` clears it. |
| Push button (GP15) | A debounced press prints `BUTTON request-to-exit -> release check` and opens the deadbolt for the bounded hold, unless the vault interlock is out of band. |
| DHT11 (GP4) | The vault interlock is sampled when a release is attempted; a good read prints a live status line, and a failed read prints `SENSOR read failed -> WARNING` and denies. |
| RYLR998 (UART1) | Each inbound `+RCV` frame prints `RX from 0xNNNN, N bytes` and is offered to the sealed reply path. |

**Outputs**

| Output | What it shows |
| ------ | ------------- |
| Red, yellow, green LEDs | Exactly one solid lamp per state, red DENIED, yellow PENDING, green GRANTED, and all three dark at rest; `status_led.c` never blinks them. |
| 1602 I2C LCD | `PIN:<pin> S:<seq>` on the first line and `STATE:<state>` on the second, refreshed every tick. |
| SG90 servo | The deadbolt, locked at 0 degrees and open at 90 degrees for the bounded hold. |
| Onboard GP25 LED | The heartbeat, toggled every four monitor ticks in `monitor_heartbeat`. |

**Watching the console**

Open the UART0 console (Debug Probe) or the Pico's own USB serial port at
`115200`. The boot banner and control hint name the remote buttons and the push
button:

```text
=== OPERATION IRON GATE // ACT II ACCESS CONTROL ===
REMOTE: CH+ 0x47 spare | CH- 0x45 clear | CH 0x46 enter
KEYPAD: 0x16-0x4A digits | BUTTON: GP15 request-to-exit
```

While the gate runs, an interlock read prints one live status line and each event
prints a named line:

```text
INTERLOCK t=23 h=610 valid=1 LED=GRANTED seq=4
IR ENTER (0x46)
RX from 0x0001, 120 bytes
BUTTON request-to-exit -> release check
SENSOR read failed -> WARNING
```

The status line carries the sensor value (`t` and `h`), the annunciator state
(`LED`), and the authorization sequence (`seq`). The event lines cover a decoded
remote command, a button press, an inbound radio frame, and a failed sensor read.

<br>

## Build and Flash

### 1. Install toolchain prerequisites

- Pico SDK 2.2.0+
- ARM GNU toolchain (`arm-none-eabi`)
- CMake and Ninja
- Python 3.x
- GDB (`arm-none-eabi-gdb`) for the Lab 3 SRAM attack

**Linux:**

```bash
export PICO_SDK_PATH="$HOME/.pico-sdk/sdk/2.2.0"
```

**macOS:**

```bash
brew install cmake ninja arm-none-eabi-gcc python
export PICO_SDK_PATH="$HOME/.pico-sdk/sdk/2.2.0"
```

**Windows:** install PowerShell, Visual Studio Build Tools, CMake, Ninja,
Python 3, and the ARM embedded toolchain.

### 2. Build the firmware

```bash
mkdir -p build && cmake -S . -B build -G Ninja -DPICO_BOARD=pico2 -DPICO_PLATFORM=rp2350-arm-s && cmake --build build
```

Build-time artifact guardrail:

- The build regenerates `packet_artifact.h` from
  `scripts/packet_artifact.json` before compiling.
- The build fails if the committed `include/packet_artifact.h` is stale relative
  to the JSON artifact.

Generated outputs:

- `build/access_gate.elf` (primary firmware binary)
- `build/access_gate.uf2` (UF2 for BOOTSEL/picotool)
- `build/access_gate_app.elf` / `.uf2` (backward-compatible copies)

### 3. Flash the RP2350

**BOOTSEL (drag-and-drop):** hold BOOTSEL while plugging in USB, then:

```bash
cp build/access_gate.uf2 /Volumes/RP2350/
```

**picotool:**

```bash
picotool load build/access_gate.uf2 -fx
```

*(If `picotool` is not on your PATH, invoke it from
`$HOME/.pico-sdk/picotool/*/picotool/picotool`.)*

**Debug Probe (SWD):** with `openocd` installed you can flash and reset without
touching BOOTSEL:

```bash
openocd -f interface/cmsis-dap.cfg -f target/rp2350.cfg \
  -c "program build/access_gate.elf verify reset exit"
```

### 4. Watch the console

Open the UART0 console (Debug Probe) or the Pico's USB serial port at `115200`.
On reset you should see:

```text
BOOT
I2C scan:
  found 0x27
=== OPERATION IRON GATE // ACT II ACCESS CONTROL ===
REMOTE: CH+ 0x47 spare | CH- 0x45 clear | CH 0x46 enter
KEYPAD: 0x16-0x4A digits | BUTTON: GP15 request-to-exit
```

`found 0x27` confirms the LCD backpack answered on the I2C bus, and the banner
confirms the state machine reached its ready policy. If a peripheral fails, the
firmware prints `INIT FAIL` and stops.

<br>

## Lab 1: Bring-Up and Verify

**Goal:** prove the gate reads the interlock, drives the LCD, takes a PIN, reaches
the desk, and moves the deadbolt.

1. Wire the node per the pin map and attach the antenna.
2. Build and flash the firmware.
3. Connect the desk radio to the laptop and find its port (`/dev/cu.usbserial-*`
   on macOS, `/dev/ttyUSB*` on Linux).
4. Start the security desk:

   ```bash
   python3 scripts/gateway.py --port /dev/cu.usbserial-XXXX --baud 115200
   ```

5. Type a six-digit PIN on the IR remote and press ENTER. The gate seals a
   request and the desk prints it, then answers with a sealed grant:

   ```text
   +OK
   +OK
   +RCV=7,92,<92 hex characters>,-11,10
   GRANT seq=1 pin=482190
   ```

6. The gate turns yellow (PENDING), receives the grant, verifies the state tag
   and the anti-replay window, checks the interlock, then turns green (GRANTED)
   and opens the deadbolt for five seconds before sealing again.

**Checkpoint:** the LCD shows `PIN:482190 S:0001` and `STATE:GRANTED`, the green
LED is lit during the release, and `access_log.csv` gains one row per request:

```text
utc,sender,auth,pin,rssi_snr
2026-09-20T09:30:05+00:00,7,OK,482190,"-11,10"
```

**Theory check:** why does a successful grant prove the LCD initialized? Because
`monitor_init()` only returns true when every peripheral, including the LCD, is
ready; otherwise `main` prints `INIT FAIL` and never enters the loop.

<br>

## Lab 2: Inspect the Wire Protocol

**Goal:** see the sealed envelope and the declared-length rule in action.

1. Capture a full `+RCV` line from the console or the desk log.
2. Confirm the declared length equals the number of hex characters between the
   second comma and the RSSI field.
3. Split the hex into three parts: the first 48 hex characters are the 24-byte
   nonce, the last 32 are the 16-byte tag, and everything between is the
   ciphertext of the request or grant body.
4. Locate the payload, its declared length, and the two tail fields in
   `scripts/gateway.py` (`_rcv_parts` and `_split_payload`), and explain why
   finding the *first* comma would be a bug.
5. Challenge: for the grant body, identify the four bytes of the sequence number
   and the sixteen bytes of the state tag.

**Checkpoint:** you can explain why a frame must be sliced by the number in the
declared length field, not by delimiter counting, and why a hex envelope is even
safer than comma-bearing JSON.

<br>

## Lab 3: The Red Half

**Goal:** get in using the gate's own flaws, with no key. Four attacks, each one
demonstrating a defect that Lab 4 then seals.

### Attack A: replay a captured grant

1. Run the legitimate loop and capture one sealed grant hex string from the desk
   console.
2. Replay it with the spoof tool in `replay` mode:

   ```bash
   python3 scripts/spoof.py --port /dev/cu.usbserial-ATTACK --victim 7 --mode replay --capture <captured hex>
   ```

3. On the naive gate the replayed grant opens the deadbolt again, because the
   tag is genuine and nothing tracks freshness.

**The lesson:** authentication is not freshness. A captured valid frame is still
a valid frame until the gate keeps state.

### Attack B: forge a grant

1. Inject a structurally plausible grant with a random nonce and a random tag:

   ```bash
   python3 scripts/spoof.py --port /dev/cu.usbserial-ATTACK --victim 7 --mode bad-tag --seq 99
   ```

2. The naive gate accepts it because it only checks the grant flag. The hardened
   gate rejects it because the spoof tool holds no field key and cannot produce a
   tag that verifies.

**The lesson:** a grant is data, and data without a key is a suggestion.

### Attack C: flip the SRAM verdict with GDB

This is the centerpiece. The cryptography is perfect, and the verdict lives
unprotected in SRAM.

1. Start the gate and halt it under the Debug Probe:

   ```bash
   arm-none-eabi-gdb build/access_gate.elf
   (gdb) target extended-remote /dev/cu.usbmodemXXXX
   (gdb) monitor reset halt
   ```

2. Break in `monitor_grant_release` and inspect the authorization record:

   ```text
   (gdb) p g_auth
   (gdb) set g_auth.granted = 1
   (gdb) continue
   ```

3. On the naive gate the deadbolt opens. On the hardened gate,
   `auth_state_ok` recomputes the state tag over the modified record, finds that
   `granted` no longer matches the tag, and denies before the bolt moves.

**The lesson:** the wire is authenticated, the verdict is not, unless you tag the
state. This is the TOCTOU failure in one GDB session.

### Attack D: walk through request-to-exit

1. Press the request-to-exit button with no badge and no desk grant.
2. The deadbolt opens. This is not a bug. It is a deliberate, defensible
   life-safety egress path: a person inside the vault must always be able to
   leave, even in a fire, even if the desk is down, even if every key is lost.
3. Note what that means for the audit trail: the access log shows a release with
   no badge, because the design accepts local egress over remote authority.

**The lesson:** security decisions and safety decisions can conflict, and the
right answer is to name the trade-off and contain it, not to "fix" it closed.

### Hardening checklist for the red half

- Reject any grant whose sequence is not strictly newer than the last accepted
  one.
- Verify a keyed tag before trusting the grant flag.
- Authenticate the authorization record itself before acting on the verdict.
- Rate-limit and lock out the keypad so a four-to-six digit PIN is not a brute
  force target.
- Lock the debug port in production and treat the probe as an attacker.
- Log unauthenticated attempts distinctly instead of discarding them.

<br>

## Lab 4: The Blue Half

**Goal:** seal the gate so the red half cannot be done to you. Each control maps
to an attack in Lab 3.

### 1. Anti-replay window

`src/auth.c` holds `last_seq`, the highest sequence ever accepted.
`auth_apply_grant` requires `seq > last_seq` before it will do anything else. A
captured GRANTED frame fails on second use even though its tag is valid. Re-run
Attack A: the replayed grant now lands on the DENIED lamp and the bolt does not
move.

### 2. Authenticated state tag

`auth_state_tag` seals a ten-byte record (`granted`, `pending`, `seq[4]`,
`last_seq[4]`) under the field key with a deterministic nonce built from the
sequence and the `0xA6` domain byte. `auth_state_ok` recomputes and compares in
constant time. Re-run Attack C: the debugger write is detected and denied.

### 3. Debug-port lockdown

The probe that fixes the firmware is the probe that breaks it. In production,
burn the RP2350 secure-boot and debug-disable settings in OTP so SWD cannot read
or write SRAM on a deployed gate. In the lab, this is a discussion control: the
authenticated state tag is what makes a compromised debug port survivable.

### 4. Rate limiting and lockout

A six-digit PIN is one million combinations, and the keypad is a physical,
unauthenticated input. Add a failed-attempt counter and a lockout window in the
monitor policy, and log lockouts distinctly. Brute force is a real attack on a
door in a way it is not on a signed protocol.

### 5. Fail-secure policy

The deadbolt is sealed at initialization and on every failure path. Loss of power,
a failed sensor read, a malformed grant, a write to the verdict, or a lost radio
link all leave the bolt locked, because the release is an affirmative action that
must be authorized. Compare this to the life-safety egress, which is fail-safe by
design. The single most important blue-half lesson is that fail mode is a
deliberate policy choice, and the two halves of a door (security ingress,
life-safety egress) can require opposite defaults.

### The blue-half checklist

- Anti-replay: strictly monotonic sequence window.
- Authenticated state: keyed tag over the authorization record.
- Debug-port lockdown: OTP debug disable on the deployed part.
- Lockout: failed-attempt counter and cooldown on the optical pad.
- Fail-secure: sealed on power loss and every fault.
- Designed egress: request-to-exit stays unauthenticated, documented, and logged.
- Key lifecycle: separate the field key and the desk key, provision from OTP, and
  rotate on a schedule.

<br>

## Troubleshooting

| Symptom | Likely cause | Fix |
| ------- | ------------ | --- |
| No `BOOT` on the console | Wrong console pins / not reset | Check UART0 GP0/GP1 or USB; press RESET |
| `INIT FAIL` with no `0x27` in the scan | LCD not answering | Check LCD VCC=3.3V, SDA=GP2, SCL=GP3, contrast pot |
| LCD shows blocks / nothing | Contrast or address | Turn the backpack contrast pot; confirm address `0x27` vs `0x3F` |
| Interlock always denies | DHT11 not reading | Check DATA=GP4; add 10K pull-up to 3.3V; wait 1-2 s after power-up |
| IR remote does nothing | Receiver wiring or remote protocol | Check OUT=GP5, VCC=3.3V; confirm the remote is NEC-compatible |
| PIN will not latch | Fewer than six digits, or a consumed PIN | Enter exactly six digits, then ENTER; a consumed PIN must be entered again |
| `AT+SEND` sent but desk sees nothing | Radio unpowered / wrong band | **Power VDD**, attach antenna, use matching band modules |
| Desk sees nothing but `+OK` | Address/network mismatch | Confirm desk radio provisioned to `AT+ADDRESS=1`, `AT+NETWORKID=18` |
| `access_log.csv` stays empty while `+RCV` prints | Desk parser regression | Ensure `_split_payload` checks the comma at the declared length |
| Gate shows PENDING forever | No grant arrived before the wait | Confirm the desk replied and the gate radio address matches the reply target |
| Grant rejected on the gate | Tag, window, or interlock | Check the field key matches, the sequence is newer, and the vault is in band |

<br>

## Testing Philosophy and Coverage

Hardware bugs are expensive to find on the bench, so the firmware is written so
that almost all of it can be tested on the host. The suite compiles the real
`src/*.c` files against mock Pico SDK headers (`test/mock/`), replacing GPIO,
I2C, UART, and time with deterministic fakes.

- The mock GPIO can replay a recorded DHT11 waveform as an absolute time/level
  timeline, so the exact edge-timing decoder is exercised without a sensor.
- The mock I2C records every LCD byte, so rendered text can be decoded and
  asserted.
- The mock UART records outbound `AT+SEND` bytes and injects inbound `+RCV` lines,
  so the keypad-to-desk-to-deadbolt path runs end to end with no radio.

Run the native test suite:

```bash
python3 scripts/run_tests.py
```

Or configure via CMake and CTest:

```bash
cmake -S test -B build-test -G Ninja && cmake --build build-test && ctest --test-dir build-test --output-on-failure
```

The suite has **113 cases** and **357 checks** covering the full DHT11 waveform
and every timeout shape, the keypad accumulator and its ready-before-consume
guard, the NEC decoder and its malformed, ambiguous, and timeout shapes, the
authorization window and state tag, the declared-length parser with hex-bearing
payloads, and monitor ticks for request, grant, replay, tamper, interlock-fail,
and deadline paths.

Verify **100% line coverage** of owned firmware modules:

```bash
python3 scripts/check_coverage.py
```

The harness itself is a small in-repo framework (`test/harness/`) so the repo
vendors no third-party code and every owned file obeys the coding standard.

<br>

## Generating Packet Artifacts

`scripts/gen_packet.py` writes the build-time generated header from the JSON
artifact:

- `scripts/packet_artifact.json` is the source of truth.
- `include/packet_artifact.h` is the generated header, committed for the build
  guardrail.

Why these constants are compiled into firmware:

- The RP2350 firmware has no runtime JSON parser or filesystem on this path.
- `include/packet_artifact.h` is generated from the JSON so the frame size, node
  id, desk address, PIN length, authorization wait, servo pulses, DHT timeout,
  and provisioning constants are embedded in flash.
- This is provisioned data; regenerate whenever you rotate node identity, desk
  addressing, or key material.

To sync the committed header from the JSON artifact:

```bash
python3 scripts/gen_packet.py --from-json scripts/packet_artifact.json --header-out include/packet_artifact.h
```

The `check_packet_artifact_header` CMake target fails the build when the
committed header is stale.

<br>

## Code Standards

This repository enforces unusually strict standards because the point is to
teach disciplined embedded and tooling practice, not just working code.

### C standard

- Every function body has **no blank lines**.
- Every function body is **at most eight lines** (Doxygen comment blocks and
  lone braces excluded).
- Every file, function, macro, type, and struct member carries Doxygen
  `@brief` documentation.
- Naming: `snake_case` files/functions, `UPPER_SNAKE` macros, `snake_case_t`
  types.

Run the C audit:

```bash
python3 scripts/audit_c_standard.py
```

### Python standard

- Strict PEP8, four-space indents, `snake_case`, 79-character lines.
- Every function has a NumPy-style docstring.
- Every function executable body is **at most eight lines**, with no exceptions.
- No blank lines inside function bodies.

Run the Python audit:

```bash
python3 scripts/audit_python_standard.py
```

Both audits must report nothing.

<br>

## Project Layout

- `src/main.c`: firmware entry point
- `src/monitor.c`: state machine tying keypad, auth, deadbolt, interlock, and radio together
- `src/keypad.c`: NEC command to PIN accumulator with a ready-before-consume guard
- `src/auth.c`: authorization record, monotonic anti-replay window, authenticated state tag
- `src/sensor.c`: DHT11 one-wire sampling and vault interlock classifier
- `src/display.c`: 1602 LCD rendering over the PCF8574 I2C backpack
- `src/radio.c`: RYLR998 provisioning, AT-command interface, and `+RCV` parser
- `src/status_led.c`: red/yellow/green DENIED/PENDING/GRANTED annunciator
- `src/button.c`: debounced request-to-exit input
- `src/servo.c`: 50 Hz PWM deadbolt actuator
- `src/ir_remote.c`: VS1838B edge timing and NEC badge/keypad decoder
- `src/crc.c`: CRC-16/CCITT-FALSE helper
- `src/chacha20.c`, `src/poly1305.c`, `src/crypto_aead.c`, `src/blake2b.c`, `src/argon2.c`, `src/crypto_kdf.c`, `src/envelope.c`: the in-repo cryptographic stack
- `include/access_gate.h`: board-level pin and provisioning configuration
- `include/auth.h`, `include/keypad.h`: authorization and keypad interfaces
- `include/field_secrets.h`: lab-only committed key material
- `include/packet_artifact.h`: generated packet artifact header
- `test/test_access_gate_and_security.c`, `test/test_peripheral_and_crypto.c`: comprehensive test suites
- `test/mock/`: Pico SDK hardware mocks (GPIO, I2C, UART, timer)
- `test/harness/`: minimal in-repo test harness (strictly C-standard compliant)
- `scripts/gateway.py`: security desk with radio provisioning, authentication, CSV logging, and sealed grant replies
- `scripts/spoof.py`: forged and replayed grant injection client
- `scripts/sim_edge.py`: laptop gate-node simulator
- `scripts/field_crypto.py`: pure-Python interoperable crypto
- `scripts/gen_packet.py` / `scripts/packet_artifact.json`: packet artifact generator and source
- `scripts/run_tests.py`, `scripts/check_coverage.py`: test runner and coverage report
- `scripts/audit_c_standard.py`, `scripts/audit_python_standard.py`: code-standard auditors
- `scripts/gen_banner.py`: banner generator
- `paper.typ` / `paper.pdf`: classroom paper describing the protocol and exercise
- `.github/workflows/release.yml`: tag-driven UF2 release workflow

<br>

## Glossary

- **AEAD**: authenticated encryption with associated data; one operation for
  secrecy and integrity.
- **Anti-replay window**: a monotonic sequence rule that rejects a valid frame
  that has already been used.
- **Argon2id**: the memory-hard password hash (RFC 9106) used to derive the field
  key.
- **AT command**: a short ASCII command (`AT+...`) understood by the radio.
- **Authorization**: whether an authenticated party is allowed to act now.
- **Authentication**: proving who a party is.
- **CRC**: cyclic redundancy check, a checksum for detecting corruption.
- **Declared length**: the byte count the sender claims for a payload; the
  receiver slices exactly that many characters.
- **Desk key**: the authorization authority under which the state tag is
  computed.
- **DHT11**: a low-cost temperature/humidity sensor using a custom one-wire
  protocol, used here as the vault interlock.
- **Fail-secure**: a fault leaves the door locked. **Fail-safe**: a fault leaves
  the egress passable.
- **Field key**: the key that seals frames on the wire.
- **HD44780**: the character-LCD controller inside a 1602 module.
- **I2C**: a two-wire bus (SDA/SCL) used here for the LCD backpack.
- **LoRa**: a long-range, low-power sub-GHz radio modulation.
- **NEC**: the infrared remote encoding the VS1838B decodes.
- **PCF8574**: an I2C I/O expander that drives the LCD's parallel interface.
- **Request-to-exit (REX)**: the deliberately unauthenticated life-safety egress
  button.
- **RSSI / SNR**: received signal strength and signal-to-noise ratio reported
  with each `+RCV` frame.
- **State tag**: a keyed tag over the authorization record that detects a
  tampered verdict.
- **TOCTOU**: time-of-check to time-of-use; a decision checked in one state and
  trusted in another.
- **UART**: a serial port used to talk to the radio.
- **XChaCha20-Poly1305**: the AEAD used for every sealed frame, with a 192-bit
  nonce and a 128-bit tag.

<br>

## Further Reading

- Act I, the sensor and telemetry chapter:
  https://github.com/mytechnotalent/cold-chain-monitor
- The companion CTF for this act:
  https://github.com/mytechnotalent/CTF_access-gate
- Embedded Hacking course and breadboard:
  https://github.com/mytechnotalent/Embedded-Hacking
- Reverse Engineering self-study course:
  https://github.com/mytechnotalent/Reverse-Engineering
- `paper.typ` / `paper.pdf`: the classroom paper for this project.
- DHT11 datasheet, RYLR998 AT command reference, SG90 datasheet, and VS1838B
  datasheet (module vendors).

<br>

# Next
[OPERATION IRON GATE CTF](https://github.com/mytechnotalent/CTF_access-gate)

<br>

# License
[MIT License](https://github.com/mytechnotalent/access-gate/blob/main/LICENSE)
