# Big Iron CDE

> Open-source tools for running, testing, and teaching **real 6502 hardware** on a breadboard. A Raspberry Pi Pico acts as the ROM, clock, and reset source, so you do not need vintage EPROMs or oscillators.

[![Website](https://img.shields.io/badge/website-big--iron.dev-blue)](https://big-iron.dev)
[![Organization](https://img.shields.io/badge/GitHub-big--iron--cde-181717?logo=github)](https://github.com/big-iron-cde)

**Big Iron CDE** (Computer Design & Engineering) is an open-source organization for people who want to build, learn, and experiment with real 6502 systems. This GitHub org holds the firmware, host software, starter templates, and CI tooling that make that possible with a modern workflow.

---

## Table of Contents

- [What is Big Iron?](#what-is-big-iron)
- [Why does it exist?](#why-does-it-exist)
- [Who is it for?](#who-is-it-for)
- [How the pieces fit together](#how-the-pieces-fit-together)
- [Repositories](#repositories)
- [Getting started](#getting-started)
- [Documentation](#documentation)
- [Contributing](#contributing)
- [License](#license)

---

## What is Big Iron?

Big Iron is a full toolkit for building and testing a 6502 computer by hand. Here is what each part does:

| Layer | What it does |
|-------|--------------|
| **Hardware** | A real W65C02S CPU, SRAM, and a Raspberry Pi Pico wired on a breadboard |
| **Firmware** | [Piclone](https://github.com/big-iron-cde/piclone) makes the Pico act as a 32 KB ROM, clock, and reset controller |
| **Host client** | [Romulan](https://github.com/big-iron-cde/romulan) builds ROM images from annotated assembly and talks to the Pico over USB |
| **Automation** | [Runner](https://github.com/big-iron-cde/runner) is a GitHub Actions runner in Docker that can reach a real board over serial |
| **Design assets** | [Fritzing parts](https://github.com/big-iron-cde/fritzing-parts) and the [Starter](https://github.com/big-iron-cde/starter) template help you document circuits and start new assembly projects |

You write a program on your laptop, upload it to the Pico, and the CPU runs it on real hardware. Romulan can also record every bus cycle as JSON, so you can script tests instead of watching LEDs by hand.

---

## Why does it exist?

Most classic 6502 setups need extra gear that is hard to find and slow to work with:

- EPROM programmers and UV erasers
- Crystal oscillators and clock cans
- 5 V to 3.3 V level shifters
- Logic analyzers or LEDs for manual debugging

Big Iron replaces that with a simpler path. One 3.3 V Pico handles the ROM, clock, and reset. You write 6502 assembly, build a ROM image on your laptop, upload it over USB, and watch the CPU run on a real chip. No EPROM burning. No separate programmer.

The goal is real computer architecture on real hardware: a real address bus, real instruction fetches, and real reset vectors, with a workflow that feels like normal embedded development.

---

## Who is it for?

**Learners** who want to go beyond emulators and see a 6502 actually fetch instructions from a breadboard they wired.

**Teachers and course builders** who need repeatable lab workflows: build, upload, capture bus cycles, and check results with scripts instead of guessing from LED patterns.

**Hobbyists and tinkerers** who enjoy retro computing but want open tools, clear wiring docs, and a USB-based upload path.

**Developers** who want to automate hardware tests. Runner passes serial access into GitHub Actions so CI jobs can run against a physical board.

Everything here is MIT-licensed. Fork it, extend it, use it in your own projects.

---

## How the pieces fit together

```
  Developer
    |                           ^
    |  annotated hex to ROM     |  pass/fail + logs (GitHub)
    |  push / PR (CI)           |
    v                           |
  Romulan <-------- Runner (GitHub Actions)
    ^  |              runs upload + capture on real hardware
    |  |
    |  |  bus capture + status (local, in terminal)
    |  |
    |  USB serial (Hardware API v1)
    v
  Piclone firmware (Pico)
    |
    |  address / data bus
    v
  W65C02S + SRAM (breadboard)
```

A typical workflow looks like this:

1. Write 6502 assembly. Start from [starter](https://github.com/big-iron-cde/starter) or use your own files.
2. Turn it into an annotated hex dump and build a 32 KB ROM with Romulan.
3. Flash [Piclone](https://github.com/big-iron-cde/piclone) onto a Pico and wire the breadboard.
4. Upload the ROM and capture bus cycles until the program hits `STP`. Romulan prints the results in your terminal.
5. For CI, push your code or open a PR. [Runner](https://github.com/big-iron-cde/runner) runs the same upload and capture steps on real hardware, then reports pass/fail and logs back to you on GitHub.

---

## Repositories

| Repository | What it is | Docs |
|------------|------------|------|
| [**piclone**](https://github.com/big-iron-cde/piclone) | Pico firmware that acts as a ROM emulator, clock, and reset controller for a 65C02 on a breadboard | [piclone.big-iron.dev](https://piclone.big-iron.dev) |
| [**romulan**](https://github.com/big-iron-cde/romulan) | Python host client that parses annotated hex, builds ROM images, and controls the Pico over USB | [romulan.big-iron.dev](https://romulan.big-iron.dev) |
| [**starter**](https://github.com/big-iron-cde/starter) | Template repo for new 6502 assembly projects using the Big Iron toolchain | n/a |
| [**runner**](https://github.com/big-iron-cde/runner) | Docker-based GitHub Actions runner with serial access for hardware-in-the-loop testing | n/a |
| [**fritzing-parts**](https://github.com/big-iron-cde/fritzing-parts) | Fritzing part files for the 6502, HM62256 RAM, and SST39SF010A ROM | n/a |

---

## Getting started

If you are new here, start in this order:

1. **Read the wiring guide:** [Piclone hardware reference](https://big-iron-cde.github.io/piclone/hardware/pinout.html)
2. **Flash the firmware:** build `piclone.uf2` and load it onto a Pico 2
3. **Install the host client:** clone [romulan](https://github.com/big-iron-cde/romulan) and run `uv sync`
4. **Build and upload a demo ROM:** `uv run romulan demo.txt --build --upload`
5. **Capture bus cycles:** `uv run romulan hardware capture --max-cycles 500`

Want a structured first project? Fork the [starter](https://github.com/big-iron-cde/starter) template.

---

## Documentation

| Site | What you will find there |
|------|--------------------------|
| [big-iron.dev](https://big-iron.dev) | Organization home and project index |
| [piclone.big-iron.dev](https://piclone.big-iron.dev) | Wiring diagrams, firmware internals, Hardware API protocol |
| [romulan.big-iron.dev](https://romulan.big-iron.dev) | Getting started, CLI reference, Python API |
| [GitHub Pages (piclone)](https://big-iron-cde.github.io/piclone/) | Mirror of Piclone docs |
| [GitHub Pages (romulan)](https://big-iron-cde.github.io/romulan/) | Mirror of Romulan docs |

---

## Contributing

Issues and pull requests are welcome in any repository. If you plan a big change, open an issue first. That is especially important for firmware pin maps, the Hardware API protocol, and how Romulan builds ROM images.

---

## License

Project code is released under the **MIT License** unless a repository's `LICENSE` file says otherwise.
