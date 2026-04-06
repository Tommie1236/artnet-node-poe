# Art-Net Node PoE — v2

A compact, fully SMD Art-Net node with Power over Ethernet (PoE) support and two bidirectional DMX512 ports.

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [What is Art-Net?](#what-is-art-net)
- [What is DMX512?](#what-is-dmx512)
- [Hardware](#hardware)
  - [v1 vs v2](#v1-vs-v2)
  - [Board Design](#board-design)
  - [Power](#power)
  - [DMX Ports](#dmx-ports)
- [Getting Started](#getting-started)
  - [Powering the Node](#powering-the-node)
  - [Connecting DMX Devices](#connecting-dmx-devices)
  - [Network Configuration](#network-configuration)
- [Usage](#usage)
- [Planned Features](#planned-features)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

This project is the second generation of a DIY Art-Net node — a device that bridges an Ethernet network running the [Art-Net protocol](https://art-net.org.uk/) to standard DMX512 lighting fixtures and controllers.

The v1 node ([pico-art-net-node](https://github.com/Tommie1236/pico-art-net-node)) was built around a **Raspberry Pi Pico** with separate breakout modules: a WIZnet W5500 Ethernet module, an isolated RS-485 transceiver board, and an SSD1306 OLED display. While that approach worked well for prototyping, it produced a larger assembly with many inter-board connections.

The v2 design integrates everything onto a single PCB using **fully surface-mount components** — no separate Pico module, no separate W5500 module. The result is a much more compact and reliable board. The most significant new feature is **Power over Ethernet (PoE)**: a single RJ45 cable now carries both network data and power, eliminating the need for a separate power supply. v2 also expands from one DMX port to **two independent bidirectional DMX512 ports**.

---

## Features

| Feature | v1 ([pico-art-net-node](https://github.com/Tommie1236/pico-art-net-node)) | v2 (this project) |
|---|---|---|
| PCB design | THT breakout modules (Pico, W5500, RS-485 board) | Fully SMD, single PCB |
| Microcontroller | Raspberry Pi Pico (RP2040 module) | Integrated SMD MCU |
| Ethernet | WIZnet W5500 breakout module | Integrated SMD Ethernet |
| DMX ports | 1 (output) | 2 (Port A & Port B) |
| DMX direction | Output only | Input **and** Output per port |
| Display | SSD1306 OLED | — |
| Power source | External supply only | External supply **or** PoE |
| RDM support | No | Planned |

- **Fully SMD layout** — no bulky THT breakout modules; compact, clean, and reliable.
- **Power over Ethernet (PoE)** — power the node directly from a PoE-capable switch or injector via the RJ45 cable.
- **Two DMX512 ports** — both Port A and Port B support DMX **output** (Art-Net → DMX) and DMX **input** (DMX → Art-Net).
- **Standard Art-Net protocol** — compatible with any Art-Net controller or software (e.g., MA Lighting, QLC+, MadMapper, sACN bridges, etc.).
- **Future RDM support** — the hardware is designed with RDM in mind for a future firmware update.

---

## What is Art-Net?

[Art-Net](https://art-net.org.uk/) is a royalty-free protocol developed by Artistic Licence that allows DMX512 data to be transmitted over standard Ethernet (UDP/IP) networks. It is widely used in professional and hobbyist stage lighting installations to send lighting control data over long distances, through network switches, and to multiple nodes simultaneously.

An Art-Net **node** acts as the bridge between the Ethernet network and physical DMX cables, converting incoming Art-Net packets into DMX512 signals (output mode) and/or reading DMX512 signals and forwarding them as Art-Net packets to the network (input mode).

---

## What is DMX512?

DMX512 (Digital Multiplex 512) is a serial communication standard used to control stage lighting, dimmers, LEDs, fog machines, and many other effects devices. A single DMX universe carries up to 512 channels of 8-bit data at a refresh rate typically around 44 Hz. Devices on the DMX bus are daisy-chained through standard 5-pin (or 3-pin) XLR connectors.

---

## Hardware

### v1 vs v2

The original [v1 node (pico-art-net-node)](https://github.com/Tommie1236/pico-art-net-node) used a **Raspberry Pi Pico** (RP2040) as the microcontroller, paired with a WIZnet W5500 Ethernet breakout module, a separate isolated RS-485 transceiver board for DMX, and an SSD1306 OLED display for status info. All components were connected by wires and headers, which was great for iteration but produced a relatively bulky assembly.

v2 replaces this approach with a single, compact PCB:

- The RP2040 (or equivalent MCU) and Ethernet controller are placed as bare SMD components directly on the PCB, eliminating the stackable module approach.
- RS-485 transceivers and isolation are integrated on-board for both DMX ports.
- The PoE power extraction and regulation circuitry is integrated, replacing the need for an external supply.

### Board Design

- Single compact PCB with all components surface-mounted.
- Integrated Ethernet PHY and microcontroller section.
- Isolated RS-485 transceivers for each DMX port.
- PoE power extraction circuitry.
- Direction control logic for bidirectional DMX operation.

### Power

The node can be powered in two ways:

1. **Power over Ethernet (PoE)** — Connect the node to a PoE-capable Ethernet switch or use a PoE injector. The on-board PoE circuitry extracts power from the Ethernet cable and regulates it for the rest of the board. No additional cables required.
2. **External DC supply** — A conventional DC power input is provided as an alternative to PoE, for use with non-PoE network equipment.

### DMX Ports

The node provides **two independent DMX512 ports**, each with:

- A 5-pin XLR connector (standard professional wiring).
- An isolated RS-485 transceiver for electrical isolation from the network side.
- Software-configurable direction: **output** (receive Art-Net, drive DMX) or **input** (read DMX, send Art-Net).

Each port operates on its own Art-Net **universe**, configurable via the node's settings.

---

## Getting Started

### Powering the Node

**Option A — PoE:**

1. Connect the node to a PoE-capable switch port or a PoE injector using a standard Cat5e/Cat6 Ethernet cable.
2. The on-board PoE circuitry will power up the node automatically.

**Option B — External DC:**

1. Connect a suitable DC power supply to the barrel jack / power terminal.
2. Connect the Ethernet cable to a standard (non-PoE) switch or router.

### Connecting DMX Devices

- **Output mode (Art-Net → DMX):** Connect Port A or Port B to the DMX input of a lighting fixture, dimmer rack, or the first device in a DMX chain.
- **Input mode (DMX → Art-Net):** Connect a DMX controller's DMX output to Port A or Port B to forward its signal onto the Art-Net network.

Use a **5-pin XLR cable** to connect DMX devices. 3-pin XLR adapters can be used where needed (pins 1, 2, and 3 only).

Always **terminate** the end of a DMX chain with a 120 Ω terminator plug.

### Network Configuration

By default the node will attempt to obtain an IP address via DHCP. If no DHCP server is available it will fall back to a static IP (see firmware documentation for default address).

Configuration options (universe assignments, port directions, IP settings) are accessible via:

- A web-based configuration interface served by the node itself.
- Art-Net ArtAddress and ArtPoll packets from a compatible controller.

---

## Usage

1. Power the node (PoE or external supply).
2. Connect the node to your Ethernet network.
3. Open your Art-Net controller software and discover or manually add the node.
4. Assign Art-Net universes to Port A and Port B.
5. Connect your DMX fixtures or controllers to the XLR ports.
6. Start sending or receiving DMX data.

---

## Planned Features

- **RDM (Remote Device Management)** — The hardware already supports the bidirectional RS-485 communication required for RDM. A future firmware update will add full RDM support, allowing the node to discover and configure RDM-capable fixtures directly from Art-Net RDM controllers.
- Web UI improvements for easier configuration.
- sACN (E1.31) support alongside Art-Net.

---

## Contributing

Contributions are welcome! See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

---

## License

This project is licensed under the [MIT License](LICENSE).

Copyright © 2026 Timoo
