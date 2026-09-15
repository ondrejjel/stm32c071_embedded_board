# STM32C071 Development Board

A compact, portable embedded development board built around the **STM32C071K8T6** microcontroller.

The board was designed as a personal platform for embedded firmware development, hardware experimentation, and prototyping. The focus is on providing a reasonably complete development environment in a small form factor while keeping important interfaces accessible for debugging and experimentation.

> **Status:** Rev. 1 PCB manufactured.

## Features

### MCU

* **STM32C071K8T6**
* ARM Cortex-M0+ core
* LQFP-32 package
* SWD programming and debugging
* Dedicated RESET and BOOT0 buttons

### Memory

* **16 MB SPI NOR flash**
* Winbond **W25Q128JV**
* Dedicated SPI interface
* Accessible through exposed test points

### Security

* **ATECC608A** secure element
* I²C interface
* Intended for experimentation with hardware-backed cryptography, secure key storage, and device identity

### USB

* USB-C receptacle
* USB 2.0 interface
* **USBLC6-4SC6** ESD protection

### Power

* 5 V input
* **MCP1825S-330** 3.3 V LDO
* Exposed 5 V and 3.3 V test points
* Multiple ground test points

### User Interface

* 5 user buttons
* Dedicated RESET button
* Dedicated BOOT0 button
* Dedicated red power indicator LED
* 5 user LEDs:

  * 1× red
  * 1× green
  * 1× blue
  * 1× orange
  * 1× yellow

### Debugging & Test

The board includes numerous test points to make hardware bring-up and debugging easier.

Exposed signals include:

* SWDIO
* SWCLK
* NRST
* BOOT0
* UART TX/RX
* SPI CS
* SPI CLK
* SPI DI
* SPI DO
* I²C SDA/SCL
* 5 V
* 3.3 V
* GND

An **STDC14** debug connector is also provided.

## Hardware Architecture

The main hardware blocks are:

```text
                         ┌──────────────────┐
                    -----│    USB-C Input   │
                    │    └────────┬─────────┘
                    │             │
                   USB           5 V
                    │             │
                    │    ┌────────▼─────────┐
                    │    │   Power / 3.3 V  │
                    │    │     MCP1825S     │
                    │    └────────┬─────────┘
                    │             │ 
                 ┌──▼─────────────▼────────────────┐
                 │          STM32C071K8T6           │
                 │                                 │
                 │  GPIO   SPI   I²C   USB   SWD   │
                 └───┬─────┬─────┬─────┬────┬─────┘
                     │     │     │     │    │
                     │     │     │     │    └── Debug
                     │     │     │     │
                     │     │     │     └──── USB
                     │     │     │
                     │     │     └──────── ATECC608A
                     │     │
                     │     └────────────── W25Q128JV
                     │
                     └────────────────── Buttons / LEDs
```

The exact peripheral assignments are defined in the schematic and firmware configuration.

## PCB Design

The schematic and PCB were designed entirely in **KiCad**.

The board was designed around several goals:

* compact and portable form factor
* practical access to debugging interfaces
* dedicated test points for important signals
* integrated external memory
* hardware security experimentation
* simple power architecture
* useful user I/O
* easy hardware bring-up

The PCB was manufactured by **JLCPCB**.

## Bring-up Plan

Once the assembled board is available, hardware bring-up will be performed incrementally.

### 1. Power

* Inspect the assembled PCB
* Check for shorts before applying power
* Verify 5 V input
* Verify 3.3 V regulator output
* Measure idle current consumption

### 2. MCU

* Connect through SWD
* Verify MCU identification
* Test RESET
* Verify BOOT0 operation
* Flash a minimal test program

### 3. GPIO

* Test user buttons
* Test LEDs
* Verify GPIO levels and mappings

### 4. SPI Flash

* Verify SPI signals with a logic analyzer
* Read the JEDEC ID
* Test read/write operations
* Verify the full memory interface

### 5. I²C / Secure Element

* Verify I²C electrical levels
* Detect the ATECC608A
* Establish communication
* Test basic device functionality

### 6. USB

* Verify USB power and signal integrity
* Test USB enumeration
* Develop and validate the intended USB functionality

Any hardware problems discovered during bring-up will be documented in this repository.

## Test Points

The board intentionally exposes important signals for external measurement.

| Signal     | Purpose              |
| ---------- | -------------------- |
| GND        | Ground reference     |
| 5V         | Main 5 V rail        |
| 3V3        | Regulated 3.3 V rail |
| NRST       | MCU reset            |
| BOOT0      | Boot configuration   |
| SWDIO      | SWD data             |
| SWCLK      | SWD clock            |
| TX         | UART transmit        |
| RX         | UART receive         |
| CS         | SPI chip select      |
| CLK        | SPI clock            |
| DI         | SPI data input       |
| DO         | SPI data output      |
| SDA        | I²C data             |
| SCL        | I²C clock            |
| USERBTN1–5 | User button signals  |

## Repository Structure

```text
.
├── hardware/
│   ├── schematic/
│   ├── pcb/
│   └── fabrication/
├── firmware/
├── docs/
├── LICENSE
└── README.md
```

The structure may evolve as firmware and documentation are added.

## Design Files

The KiCad project contains the complete schematic and PCB design.

Manufacturing files are included so that future revisions can be reproduced or modified.

## Bill of Materials

The main active components are:

| Reference | Component       | Purpose                     |
| --------- | --------------- | --------------------------- |
| U1        | STM32C071K8T6   | Main MCU                    |
| U2        | ATECC608A-SSHDA | Secure element              |
| U3        | W25Q128JV       | 16 MB SPI flash             |
| U4        | USBLC6-4SC6     | USB ESD protection          |
| U5        | MCP1825S-330E   | 3.3 V LDO                   |
| J1        | STDC14          | Debug/programming connector |
| J2        | USB-C           | USB/power connection        |

The complete BOM is available in the hardware project files.

## Project Status

| Area                | Status                |
| ------------------- | --------------------- |
| Schematic           | Complete              |
| PCB layout          | Complete              |
| Manufacturing files | Complete              |
| PCB manufacturing   | In progress           |
| Assembly            | Pending               |
| Hardware bring-up   | Pending               |
| Firmware            | Planned               |

## Lessons & Revision History

This is my first custom PCB project, so the first revision is also intended as a learning platform.

Rather than treating Rev. 1 as a final design, the goal is to use the board to identify:

* schematic mistakes
* layout issues
* component selection problems
* power-related issues
* signal integrity problems
* usability improvements

Findings from the first hardware revision will be documented here and used to guide future revisions.
