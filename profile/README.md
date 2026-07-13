<p align="center">
  <img src="https://raw.githubusercontent.com/big-iron-cde/.github/main/images/cover.png" alt="Big Iron cover" width="100%">
</p>

# Big Iron CDE

> Build and run a **real 6502 computer** on a breadboard, with open-source tools and a modern upload workflow.

[![Website](https://img.shields.io/badge/website-big--iron.dev-blue)](https://big-iron.dev)
[![License: MIT](https://img.shields.io/badge/license-MIT-green)](https://github.com/big-iron-cde/romulan/blob/main/LICENSE)

**Big Iron CDE** (Cloud Development Environment) is an open-source organization for people who want to build, learn, and experiment with real 6502 systems. This GitHub org holds the firmware, host software, starter templates, and CI tooling that make that possible with a modern workflow.

## Table of Contents

- [Big Iron CDE](#big-iron-cde)
  - [Table of Contents](#table-of-contents)
  - [What is this?](#what-is-this)
  - [Why does it exist?](#why-does-it-exist)
  - [Projects](#projects)
  - [Quick start](#quick-start)
  - [Contributing](#contributing)

---

## What is this?

Big Iron lets you wire up a **real 6502 CPU** on a breadboard and run programs you wrote yourself. You see actual instruction fetches, memory access, and bus activity on physical hardware, not just in a simulator.

A **Raspberry Pi Pico** sits on the board and does the heavy lifting:

- stores your program and feeds it to the CPU
- provides the clock signal that steps the CPU forward
- lets you upload new programs from your laptop over USB

Your laptop runs **Romulan**, the host tool that turns assembly into a program image and talks to the Pico.

---

## Why does it exist?

Learning how a CPU works is much more tangible when you can build the machine yourself. But classic 6502 kits often need extra parts that are hard to find and awkward to use in a lab.

Big Iron keeps the real hardware experience and removes that hassle. Write code, upload over USB, watch the CPU run. Open docs, wiring guides, and automation tools are included so others can reproduce and build on the same setup.

---

## Projects

| Repo | What it does |
|------|--------------|
| [**piclone**](https://github.com/big-iron-cde/piclone) | Firmware for the Pico: program storage, clock, and reset |
| [**romulan**](https://github.com/big-iron-cde/romulan) | Laptop tool: build programs, upload to the board, record what the CPU does |
| [**starter**](https://github.com/big-iron-cde/starter) | Starting point for new 6502 assembly projects |
| [**runner**](https://github.com/big-iron-cde/runner) | Runs automated tests against real hardware in GitHub Actions |
| [**fritzing-parts**](https://github.com/big-iron-cde/fritzing-parts) | Breadboard diagrams and part files for Fritzing |

**Docs:** [piclone.big-iron.dev](https://piclone.big-iron.dev) · [romulan.big-iron.dev](https://romulan.big-iron.dev)

---

## Quick start

1. Follow the [Piclone wiring guide](https://big-iron-cde.github.io/piclone/hardware/pinout.html) to build the breadboard
2. Flash [piclone](https://github.com/big-iron-cde/piclone) onto a Raspberry Pi Pico 2
3. Install [romulan](https://github.com/big-iron-cde/romulan), then run `uv run romulan demo.txt --build --upload`

---

## Contributing

Issues and pull requests are welcome. All projects are MIT-licensed.
