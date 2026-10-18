# TI Voyage 200 explorations

Reverse engineering of the TI Voyage 200's link port with an ESP32 (Arduino
framework): what the calculator says on the wire, both to a partner that
answers it and to another calculator.

| Part | What it does | Read |
|---|---|---|
| **Hardware** | Parts, wiring, a wiring check sketch, building and flashing. | [docs/hardware.md](docs/hardware.md) |
| **ESP32 as a link partner** | The ESP32 speaks TI's packet protocol, posing as a CBL data logger or another calculator, so TI-BASIC's `Send`, `Get` and `SendCalc` talk to it. | [docs/link-partner.md](docs/link-partner.md) |
| **Calculator-to-calculator protocol** | The ESP32 listens passively between two Voyage 200s; twenty tests document what they say to each other. | [PROTOCOL.md](calc-to-calc-protocol-reversed/PROTOCOL.md), [its README](calc-to-calc-protocol-reversed/README.md) |

## Layout

```
esp32/                               Arduino sketches (one folder per sketch)
  esp32_tilink_cbl/                  the ESP32 as a link partner
    esp32_tilink_cbl.ino               packet layer, decoders, Serial commands
    TiLink.h, TiLink.cpp               bit-level link driver with error diagnostics
  esp32_tilink_c2c/                  passive sniffer between two calculators
    esp32_tilink_c2c.ino               sampler, packet printer, line history, Serial commands
    BitDecoder.h                       passive bit decoder (plain C++, testable on a PC)
  esp32_tilink_linetest/             wiring check: prints tip/ring levels without driving them
calc-to-calc-protocol-reversed/      the calculator-to-calculator protocol: documentation and data
  README.md                            sniffer wiring, flashing, running a capture session
  TESTS.md                             the test sequence (T00-T20)
  PROTOCOL.md                          the reversed calculator-to-calculator protocol
  captures/                            raw logs of every session, named by date and time
docs/
  hardware.md                        parts, wiring, wiring check, building and flashing
  link-partner.md                    the link-partner sketch and its protocol findings
```

## Status

A record of experiments, not a finished product. Treat the protocol notes as
findings from these calculators, not as a specification.

## Credits

- [ArTICL](https://github.com/KermMartian/ArTICL) by Christopher Mitchell, the
  Arduino TI linking library, for the pin conventions and approach.
- The [TIGCC documentation](http://tigcc.ticalc.org/doc/link.html) for the
  calculator's link functions and variable types.
- [TI calculator character sets](https://en.wikipedia.org/wiki/TI_calculator_character_sets)
  on Wikipedia for the TI-89/92/Voyage 200 character table.
