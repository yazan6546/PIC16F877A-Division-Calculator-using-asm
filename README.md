# PIC16F877A Division Calculator

A floating-point division calculator using two PIC16F877A microcontrollers in a master-coprocessor architecture, implemented in assembly language.

## Overview

This project implements a calculator that performs division on floating-point numbers up to 999999.999999. The system uses a Master CPU for user interface (LCD display and button input) and a Co-processor for mathematical computations, communicating via UART.

## Quick Start

1. **Clone the repository**
   ```bash
   git clone https://github.com/yazan6546/PIC16F877A-Division-Calculator-using-asm.git
   cd PIC16F877A-Division-Calculator-using-asm
   ```

2. **Open in MPLAB X IDE**
   - Open `Master.X` project
   - Open `FPD4040U.X` project

3. **Build both projects**
   - Production → Clean and Build Main Project (for each)

4. **Program or simulate**
   - Load HEX files into PIC16F877A microcontrollers
   - Or use Proteus simulation with `proteus/realtime.pdsprj`

## Features

- Dual microcontroller architecture (Master and Co-processor)
- Floating-point division with 6 decimal digits precision
- Interactive digit-by-digit input via push button
- 16x2 LCD display with cursor feedback
- UART communication between processors
- Auto-fill feature for faster number entry

## Project Structure

```
PIC16F877A-Division-Calculator-using-asm/
├── Master.X/              # Master CPU code (UI, LCD, button handling)
├── FPD4040U.X/            # Co-processor code (division computation)
├── proteus/               # Circuit schematic for Proteus simulation
└── docs/                  # Detailed documentation
    ├── architecture.md    # Software architecture details
    ├── hardware.md        # Hardware specifications and pin configuration
    ├── installation.md    # Installation and build instructions
    ├── usage.md           # Detailed usage guide
    └── troubleshooting.md # Common issues and solutions
```

## Documentation

For detailed information, see the `docs/` directory:

- **[Architecture](docs/architecture.md)** - Software design, state machines, and communication protocol
- **[Hardware](docs/hardware.md)** - Component list, pin configuration, and circuit setup
- **[Installation](docs/installation.md)** - Setup, build instructions, and programming steps
- **[Usage](docs/usage.md)** - Complete operation guide with examples
- **[Troubleshooting](docs/troubleshooting.md)** - Solutions for common issues

## Hardware Requirements

- 2x PIC16F877A microcontrollers
- 16x2 character LCD display
- Push button with pull-up resistor
- 2x 4 MHz crystal oscillators (with capacitors)
- Pull-up resistors (10KΩ and 4.7KΩ)
- 5V power supply

See [Hardware Documentation](docs/hardware.md) for complete details.

## Basic Usage

1. Power on - Welcome message blinks 3 times
2. Enter first number digit by digit (button clicks increment digit 0-9)
3. Wait 1 second to lock digit and auto-fill remaining digits
4. Double-click to move to decimal part
5. Enter second number using same method
6. System calculates and displays result
7. Single-click to cycle through result/operands, double-click for new calculation

See [Usage Guide](docs/usage.md) for complete instructions.

## Build Requirements

- MPLAB X IDE (v5.0 or later)
- MPASM Assembler
- Proteus Design Suite (optional, for simulation)

## Configuration

Both processors use:
- Oscillator: XT (4 MHz crystal)
- Watchdog Timer: OFF
- Power-up Timer: OFF
- Code Protection: OFF
- Low Voltage Programming: OFF

## License

Academic project for ENCS4330 - Real-Time Applications & Embedded Systems
Birzeit University, 2nd Semester 2024/2025
Instructor: Dr. Hanna Bullata

## Additional Resources

- [PIC16F877A Datasheet](https://www.microchip.com/wwwproducts/en/PIC16F877A)
- [MPLAB X IDE](https://www.microchip.com/mplab/mplab-x-ide)
- Project requirements: `docs/statement.pdf`