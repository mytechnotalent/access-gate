# OPERATION IRON GATE - Nation-State Accuracy Review

**An adversarial, evidence-based audit of the entire project where every claim is
verified by a re-runnable command or explicitly labelled as a limitation.**

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

## 1. Scope and Method

This review treats the project as hostile-to-itself. Every module, constant, test
vector, document, and artifact is independently checked. The method is:

1. **Re-run every gate** (`audit_c_standard`, `audit_python_standard`, `run_tests`,
   `check_coverage`) and record the exact output and exit codes.
2. **Re-verify every constant** against the generated artifact header and the
   JSON source of truth, by regenerating the header and diffing it.
3. **Re-verify every cryptographic claim** against a published standard vector,
   and separate vectors that run in the native C suite from vectors that run only
   in the Python suite.
4. **Re-verify every artifact** by SHA-256 and by the in-repo build guardrail,
   because this project ships no CTF firmware artifact.
5. **Read the documents adversarially** for overclaims, stale numbers, and
   omissions, then correct them in this review.

## 2. Gate Results (all re-run for this review)

| gate | command | observed result |
|---|---|---|
| C standard | `python3 scripts/audit_c_standard.py` | exit 0, no output, **0 violations** |
| Python standard | `python3 scripts/audit_python_standard.py` | exit 0, no output, **0 violations** |
| Native tests | `python3 scripts/run_tests.py` | **357 checks, 0 failures**, 113 test cases |
| Coverage | `python3 scripts/check_coverage.py` | exit 0, **100.00% line coverage**, 1891 owned lines |
| Python field crypto | `python3 -m unittest test.test_field_crypto -v` | **7 tests, OK** |

The coverage gate passes on line coverage. It does not require 100% branch or
region coverage, and the raw report is not 100% there: regions 99.25% and
branches 93.63%. That gap is real and is stated in the module table below.

## 3. Module-by-Module Audit

Owned lines are the instrumented statement lines reported by `llvm-cov report`
through `check_coverage.py`. Raw `wc -l` over `src/*.c` is 5284 lines including
comments and blank lines; `main.c` is excluded from coverage by design. Every
owned module is at 100.00% line coverage.

| module | role | owned lines | line coverage | verification performed | honest limitation |
|---|---|---|---|---|---|
| `crc.c` | CRC-16/CCITT-FALSE diagnostic | 20 | 100.00% | `test_crc16_ccitt` check value `0x29B1` | not on the wire; a checksum is not authentication |
| `sensor.c` | DHT11 vault interlock classifier | 151 | 100.00% | waveform, all timeout shapes, CRC error, climate ok/reject/invalid, negative temp | mock GPIO replays a recorded waveform, not real silicon |
| `display.c` | HD44780 over PCF8574 | 73 | 100.00% | `test_display_format_lines`, `test_display_render_lines` via recorded I2C | mock I2C, not real HD44780 bus timing |
| `radio.c` | RYLR998 provisioning, `AT+SEND`, `+RCV` parser | 187 | 100.00% | build/parse/reject/pump, oversize guards, `test_radio_spoofed_sender_attribution` | mock UART; the RF band is not simulated |
| `status_led.c` | red/yellow/green DENIED/PENDING/GRANTED | 14 | 100.00% | `test_status_led_show` | none |
| `button.c` | request-to-exit input | 34 | 100.00% | pressed/consume/debounce/reset | deliberate unauthenticated egress path |
| `servo.c` | 50 Hz deadbolt PWM | 27 | 100.00% | `test_servo_map`, `test_servo_init`, `test_servo_actuate` | mock PWM; no real servo or inrush load |
| `ir_remote.c` | VS1838B NEC badge and keypad decode | 116 | 100.00% | decode valid/reject/bad leader/mark/ambiguous/address/command, `test_ir_poll_*` | optical path is unauthenticated; no anti-replay |
| `keypad.c` | NEC command to PIN accumulator | 35 | 100.00% | `test_keypad_accumulate`, overflow, clear, get guards | bounded count only; no lockout counter in this build |
| `auth.c` | authorization record, anti-replay window, state tag | 85 | 100.00% | `test_auth_state_tag`, apply window/advance/bad tag, null and tag guards, begin request | deterministic nonce from sequence; single key |
| `chacha20.c` | ChaCha20 and HChaCha20 | 99 | 100.00% | RFC 8439 block and stream vectors, HChaCha20 draft vector | none |
| `poly1305.c` | Poly1305 one-time authenticator | 169 | 100.00% | RFC 8439 tag vector, aligned path | none |
| `crypto_aead.c` | XChaCha20-Poly1305 seal/open | 38 | 100.00% | round-trip, tamper tag/ct/ad, constant-time `tag_equal` | built from the in-repo primitives, not an audited library |
| `blake2b.c` | BLAKE2b and Argon2 H' | 161 | 100.00% | `test_blake2b_abc`, multiblock, H' 32 and 256 vectors | none |
| `argon2.c` | Argon2id core (BLAMKA, hybrid addressing) | 336 | 100.00% | `test_argon2_lanes`, `test_argon2_type_i`, `test_argon2_clamp` branch coverage | the RFC 9106 KAT runs in Python, not in this C suite |
| `crypto_kdf.c` | Argon2id field key derivation | 28 | 100.00% | reject, empty password, determinism, salt sensitivity | classroom profile `t=3 p=1 m=64`; committed passphrase and salt |
| `envelope.c` | hex nonce/ciphertext/tag codec | 91 | 100.00% | nonce, round-trip, seal/open rejects, uppercase, known vector | none |
| `monitor.c` | gate state machine | 227 | 100.00% | init, idle, render log, state text, interlock, digit, grant success/replay/bad tag/interlock fail, state-tag tamper, REX unlock and climate block, hold and pending expiry, reply guards, IR request | mocks are not the real silicon |
| `main.c` | entry point | n/a | excluded | build only | excluded from coverage by design |

**Total owned lines at 100.00% line coverage: 1891.**

Branch coverage below 100% in the same report: `monitor.c` 84.62%, `display.c`
85.71%, `radio.c` 89.87%, `envelope.c` 92.86%, `keypad.c` 94.44%, `sensor.c`
94.74%, `ir_remote.c` 96.00%, `auth.c` 96.67%, `argon2.c` 97.56%.

## 4. Cryptographic Claim Verification

The native suite asserts the following published vectors. Each name below appears
as a passing case in the `run_tests.py` output for this review.

| claim | standard | vector | observed |
|---|---|---|---|
| ChaCha20 block function | RFC 8439 section 2.3.2 | key 00..1f, nonce 000000090000004a00000000 | `test_chacha20_block` PASS |
| ChaCha20 stream cipher | RFC 8439 section 2.4.2 | "Ladies and Gentlemen..." 114-byte ciphertext | `test_chacha20_stream` PASS |
| HChaCha20 subkey | XChaCha20 draft (irtf-cfrg-xchacha) | published subkey vector | `test_hchacha20` PASS |
| Poly1305 tag | RFC 8439 section 2.5.2 | "Cryptographic Forum Research Group" tag `a8061dc1305136c6c22b8baf0c0127a9` | `test_poly1305`, `test_poly1305_aligned` PASS |
| BLAKE2b-512 | BLAKE2 reference | digest of "abc", multiblock, long-input | `test_blake2b_abc`, `test_blake2b_multiblock` PASS |
| Argon2 variable-length hash H' | RFC 9106 section 3.3 | H' of {1,2,3,4} at 32 and 256 bytes | `test_blake2b_long_short`, `test_blake2b_long` PASS |
| Argon2id known-answer | RFC 9106 section 5.3 | `0d640df58d78766c08c037a34a8b53c9d01ef0452d75b65eb52520e96b01e659` | `test.test_field_crypto.TestFieldCrypto.test_rfc9106_argon2id_vector` PASS (Python suite) |
| Envelope layout | project vector | nonce `00..17`, node id 7, fixed body | `test_envelope_known_vector` PASS |

The RFC 9106 Argon2id known-answer test is a Python `unittest` in
`test/test_field_crypto.py`; it is not part of the 357 native checks. Running it
directly confirms all 7 field-crypto tests pass, including the KAT and the
firmware-interop envelope vector.

### 4.1 Anti-replay sequence window

`auth_apply_grant` in `src/auth.c` accepts a grant only when
`seq > auth->last_seq`, then verifies the keyed grant tag, then advances the
floor. The behavior is asserted by `test_auth_apply_window` (accept once, reject
the same sequence, reject an older sequence), `test_auth_apply_advance` (a newer
sequence advances `last_seq`), `test_auth_apply_bad_tag`, and the end-to-end
`test_monitor_grant_replay`. All pass in this review.

Honest limitation: `last_seq` is plain SRAM and resets to zero on every boot, so a
grant captured before a reboot can be replayed after one. The paper's Threat
Model states this; the README does not. A production gate would persist the floor
in non-volatile memory.

### 4.2 Authenticated-state tag

`auth_state_tag` seals a ten-byte record (`granted`, `pending`, `seq[4]`,
`last_seq[4]`) under the field key with a nonce built from the sequence and the
domain byte `0xA6`; `auth_state_ok` recomputes and compares in constant time
(`crypto_aead_tag_equal` is a branchless XOR accumulator). `test_auth_state_tag`
proves a modified record fails, and `test_monitor_state_tag_tamper` proves the
end-to-end denial before the bolt moves.

Honest limitations: the lab derives the field key and the desk role from one
committed secret, so a compromised device can compute tags the desk accepts; the
tag protects against casual tamper and a debugger that flips `granted`, not a
physical attacker who can read the key out of SRAM and recompute the tag; there is
no per-badge key or rotation; and the nonce is deterministic in the sequence, so
two distinct records that ever share a sequence would violate AEAD nonce
uniqueness.

## 5. Artifact Verification

This project ships no CTF firmware artifact (`build/` holds only untracked local
test binaries). The companion CTF is external:
`https://github.com/mytechnotalent/CTF_access-gate`, which ships the four-defect
compromised image and its verifier. What is verified in this repository is the
source tree and the provisioning artifact.

Source-tree aggregate SHA-256 over all 40 `.c` and `.h` files under `src/` and
`include/`, computed as `find src include ... | sort | xargs shasum -a 256 |
shasum -a 256`:

```
ac9f0ab980f6402b63f747263273749a73e428914b49db1c0c239b70f3ffc727
```

Key artifacts by SHA-256:

```
paper.pdf                   92218f7669afb9936d746137734cea50cd0866b3460edaf782ca79006322b97e
access-gate.png             b51066a4c61a924e8f441e813d8bf93c0a3e4bce9ffd8bfd2f9d7b15c63ec685
scripts/packet_artifact.json b212f65df3dae7f21477c3cf23eea1b058f1c7f08bffce4aa66bc41b4fa5f8c5
include/packet_artifact.h   64894d1d1b219610a715f6867271fa09c8bf9aca3db493f54a4b8fe544d49de5
include/field_secrets.h     63cffbbeb9e4740c4031865d6bcf5bb8302ede090579eb4fbf0ef41764135d07
```

The build guardrail `check_packet_artifact_header` regenerates
`include/packet_artifact.h` from `scripts/packet_artifact.json` and fails if the
committed header is stale. Re-run for this review:

```
$ python3 scripts/gen_packet.py --from-json scripts/packet_artifact.json \
      --check-header-path include/packet_artifact.h
Wrote generated firmware header: include/packet_artifact.h
Verified header matches: include/packet_artifact.h
exit=0
```

Constants re-read from `include/packet_artifact.h` and matched to the README and
the pin map: `PACKET_NODE_ID` 7, `PACKET_HUB_ADDRESS` 0x0001, `PACKET_FRAME_SIZE`
48, `PACKET_PIN_LENGTH` 6, `PACKET_AUTH_WAIT_MS` 5000,
`PACKET_SERVO_LOCK_PULSE_US` 500, `PACKET_SERVO_OPEN_PULSE_US` 1500,
`PACKET_DHT_TIMEOUT_US` 240, `PACKET_LCD_I2C_ADDRESS` 0x27,
`PACKET_MAX_RCV_LEN` 256, plus the shared pin map.

## 6. Adversarial Document Review

| document claim | audit verdict |
|---|---|
| README does not imply the device is unhackable | **accurate**; it states the wire is sealed and the verdict is not |
| README "The cryptography is perfect, and the verdict lives unprotected in SRAM" | **rhetorical overclaim**; the AEAD is correctly implemented and vector-checked, but no cryptography is "perfect" and the key is committed in flash |
| README frames request-to-exit as a deliberate unauthenticated path | **accurate**; it appears in the story, the wiring notes, Lab 3 Attack D, and Lab 4 "Designed egress" |
| README states the debug-port and SRAM limitations | **accurate**; Lab 3 Attack C and Lab 4 section 3 describe the probe and the OTP debug-disable control |
| README "The suite has **113 cases** and **357 checks**" | **accurate**; the native runner reports exactly 113 cases and 357 checks |
| README key model says the lab shares one committed secret | **accurate**; it names field and desk roles separately and calls the shared lab key a convenience, not a deployment |
| README does not mention per-badge key rotation | **omission**; the paper's Threat Model is the only place that says the two-role separation is not implemented in the lab |
| README anti-replay section does not mention the reboot reset | **omission**; the paper states it, the README does not |

Corrections: the README should drop the word "perfect," and its anti-replay and
key-model sections should carry the same reboot-reset and no-per-badge-rotation
caveats that the paper already carries. No claim of unhackability was found.

## 7. Honest Limitations

- **Physical access wins.** A Debug Probe over SWD can read the field key from
  SRAM. The authenticated state tag detects a flipped verdict, but a probe that
  can read the key and recompute the tag defeats the design. Only OTP debug
  disable closes this.
- **Key extraction from flash.** `include/field_secrets.h` commits the passphrase
  and salt. Anyone holding the image holds the key. This is a lab convenience,
  not a deployment.
- **Single shared field and desk key.** Both roles derive from one committed
  secret, so a compromised field device can compute state tags the desk accepts.
  There is no per-badge key and no rotation in this build.
- **Replay after reboot.** `last_seq` resets to zero, so a grant captured before
  a power cycle can be replayed after it. The floor is not persisted.
- **Classroom crypto profile.** Argon2id runs at `t=3 p=1 m=64` to fit SRAM; the
  state-tag nonce is deterministic in the sequence; both are teaching parameters,
  not hardening parameters.
- **Designed egress.** The request-to-exit button opens the deadbolt with no
  badge, PIN, or desk grant. This is a deliberate life-safety path, documented and
  logged, not a bug to close.
- **Unauthenticated optical input.** Any NEC remote can enter a PIN. Rate limiting
  and lockout are described as policy but are not implemented in this build.
- **Supply chain and sensor trust are out of scope.** The DHT11 is checksummed,
  not authenticated, and the firmware is only as trustworthy as the toolchain and
  the parts.
- **Availability is not protected.** An attacker on the band can jam or flood the
  receiver.
- **Coverage is line coverage.** Branch coverage is not 100%, and the harness
  mocks are not the real silicon.

## 8. Conclusion

The project is internally consistent and candid: 18 owned modules, 1891
instrumented lines, 100.00% line coverage, 357 native checks passing with 0
failures, 113 native cases, and 7 Python field-crypto tests passing, every
cryptographic primitive anchored to a published vector. The four gates all pass
with exit 0. Act II adds real stateful controls over Act I: a strictly monotonic
anti-replay window and a keyed tag over the authorization record, both exercised
end to end. The documentation is unusually honest about the shared key, the open
debug port, and the designed egress, with two minor omissions (reboot reset and
no per-badge rotation) and one rhetorical overclaim ("perfect") that should be
corrected. The core lesson holds and is stated: the wire is sealed, the verdict
is tagged, and the remaining risk is the key and the probe.

---

*This review is reproducible: run the five commands in section 2, the header
check in section 5, and the Python field-crypto suite in section 4.*

This is Act II of the ten-act OPERATION COLD IRON saga. See SAGA.md.
