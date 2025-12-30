# PIC16F877A Division Calculator

A complex calculator system that performs floating-point division operations using two PIC16F877A microcontrollers in a master-coprocessor architecture, implemented entirely in assembly language.

## 📋 Table of Contents

- [Project Objective](#project-objective)
- [Features](#features)
- [Hardware Requirements](#hardware-requirements)
- [Pin Configuration](#pin-configuration)
- [Software Architecture](#software-architecture)
- [Installation Instructions](#installation-instructions)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [Build Instructions](#build-instructions)
- [Circuit Setup](#circuit-setup)
- [License](#license)

## 🎯 Project Objective

This project implements a floating-point division calculator using dual PIC16F877A microcontrollers. The system allows users to input two numbers (up to 999999.999999) via a push button interface, performs the division operation on a dedicated coprocessor, and displays the result on a 16x2 character LCD.

**Key Goals:**
- Demonstrate inter-processor communication using UART
- Implement interrupt-driven I/O handling
- Perform complex floating-point arithmetic operations in assembly
- Create an intuitive user interface with LCD display and button input

## ✨ Features

- **Dual Microcontroller Architecture**: Master CPU for I/O handling, Co-processor for mathematical computations
- **Floating-Point Support**: Handles numbers up to 1 million with 6 decimal digits precision (e.g., 999999.999999)
- **Interactive Input**: Digit-by-digit number entry via push button with auto-fill feature
- **Smart Input System**:
  - Single click: Increment digit (0-9)
  - Hold for 1 second: Lock digit and proceed
  - Double-click: Skip to next section (integer → decimal or first number → second number)
- **Visual Feedback**: 16x2 LCD display with blinking cursor for input guidance
- **Welcome Animation**: Blinking welcome message on startup
- **Result Navigation**: Browse through operands and results using button clicks
- **UART Communication**: Interrupt-driven data transfer between processors with acknowledgment protocol

## 🔧 Hardware Requirements

### Components

1. **2x PIC16F877A Microcontrollers**
   - Master CPU: Handles user interface and display
   - Co-processor (Auxiliary CPU): Performs division calculations

2. **16x2 Character LCD Display**
   - Operating in 4-bit mode
   - Connected to Master CPU

3. **Push Button (P)**
   - Debounced input for number entry and navigation

4. **Oscillators**
   - 2x 4 MHz crystal oscillators
   - 4x 15pF capacitors (2 per oscillator)

5. **Resistors**
   - 2x 10KΩ pull-up resistors (for MCLR pins)
   - 1x 10KΩ pull-up resistor (for push button)
   - 1x 4.7KΩ pull-up resistor (for LCD RS pin)

6. **Power Supply**
   - 5V DC power source

## 📌 Pin Configuration

### Master CPU (PIC16F877A)

| Port/Pin | Function | Connection |
|----------|----------|------------|
| PORTD | LCD Data/Control | 16x2 LCD (4-bit mode) |
| PORTD.0 (RD0) | LCD RS | LCD Register Select |
| PORTD.1 (RD1) | LCD E | LCD Enable |
| PORTD.4-7 (RD4-RD7) | LCD D4-D7 | LCD Data Lines |
| PORTB.0 (RB0) | Button Input | Push Button P (with 10KΩ pull-up) |
| PORTC | UART Communication | Data transfer to/from Co-processor |
| PORTC.6 (RC6) | TX | UART Transmit |
| PORTC.7 (RC7) | RX | UART Receive |
| MCLR | Master Reset | 10KΩ pull-up resistor |
| OSC1/OSC2 | Clock | 4 MHz crystal + 2x 15pF capacitors |

### Co-processor (Auxiliary CPU - PIC16F877A)

| Port/Pin | Function | Connection |
|----------|----------|------------|
| PORTC | UART Communication | Data transfer to/from Master CPU |
| PORTC.6 (RC6) | TX | UART Transmit |
| PORTC.7 (RC7) | RX | UART Receive |
| MCLR | Master Reset | 10KΩ pull-up resistor |
| OSC1/OSC2 | Clock | 4 MHz crystal + 2x 15pF capacitors |

## 🏗️ Software Architecture

### Master CPU (`Master.X`)

**Responsibilities:**
- LCD initialization and display management
- User input handling (button debouncing and state management)
- Number entry state machine
- UART communication protocol (send operands, receive results)
- BCD ↔ Binary conversion
- Display formatting and cursor control

**Key Components:**
- `main.asm`: Main program logic and state machine
- `LCD_DRIVER.inc`: LCD control routines (4-bit mode)
- `BCD_TO_LCD.inc`: BCD to LCD character conversion
- `UART.inc`: UART communication protocol
- `binary_to_bcd.inc`: Binary to BCD conversion
- `bcd_to_binary.inc`: BCD to Binary conversion

### Co-processor (`FPD4040U.X`)

**Responsibilities:**
- Receive operands via UART
- Perform 40-bit floating-point division
- Send results back via UART
- Implement acknowledgment protocol

**Key Components:**
- `div.asm`: Main division logic and UART handling
- `ALU5B.inc`: 5-byte (40-bit) arithmetic logic unit
- `UART.inc`: UART communication protocol

### Communication Protocol

The processors communicate using UART with an interrupt-driven acknowledgment protocol:

1. **Master → Co-processor**: Send number of bytes
2. **Co-processor → Master**: Send acknowledgment
3. **Master → Co-processor**: Send data bytes (one at a time)
4. **Co-processor → Master**: Acknowledge each byte
5. **Co-processor → Master**: Send result bytes (one at a time)
6. **Master → Co-processor**: Acknowledge each byte

## 📥 Installation Instructions

### Prerequisites

1. **MPLAB X IDE** (v5.0 or later)
   - Download from [Microchip's website](https://www.microchip.com/mplab/mplab-x-ide)

2. **MPASM Assembler** (included with MPLAB X)

3. **Proteus Design Suite** (optional, for simulation)
   - Used for circuit simulation and testing

### Setup Steps

1. **Clone the Repository**
   ```bash
   git clone https://github.com/yazan6546/PIC16F877A-Division-Calculator-using-asm.git
   cd PIC16F877A-Division-Calculator-using-asm
   ```

2. **Open Projects in MPLAB X**
   - Open MPLAB X IDE
   - File → Open Project
   - Navigate to `Master.X` folder and open the project
   - Repeat for `FPD4040U.X` folder

3. **Verify Include Paths**
   - Ensure all `.inc` files are in the project directory
   - Check that include paths are properly configured in project properties

## 🔨 Build Instructions

### Building Master CPU Code

1. Open `Master.X` project in MPLAB X
2. Select "PIC16F877A" as target device
3. Clean and build the project:
   - Production → Clean and Build Main Project
4. Verify successful build (check output window)
5. The HEX file will be generated in `Master.X/dist/default/production/`

### Building Co-processor Code

1. Open `FPD4040U.X` project in MPLAB X
2. Select "PIC16F877A" as target device
3. Clean and build the project:
   - Production → Clean and Build Main Project
4. Verify successful build (check output window)
5. The HEX file will be generated in `FPD4040U.X/dist/default/production/`

### Build Configuration

Both projects use the following configuration bits:
- Oscillator: XT (4 MHz crystal)
- Watchdog Timer: OFF
- Power-up Timer: OFF
- Code Protection: OFF
- Low Voltage Programming: OFF
- Brown-out Detect: OFF

## 💻 Usage

### System Operation Flow

#### 1. Power-On Sequence
- LCD displays "Welcome to" on line 1
- LCD displays "Division!" on line 2
- Message blinks 3 times with 0.5-second intervals
- 2-second delay before moving to input mode

#### 2. First Number Entry

**Integer Part:**
1. LCD displays "Number 1" on line 1
2. Cursor blinks on line 2 (most significant digit position)
3. Click button P to increment digit (0 → 1 → 2 → ... → 9 → 0)
4. Wait 1 second without clicking to lock the digit
5. Remaining digits auto-fill with the same value (e.g., 6 → 666666)
6. Continue clicking to modify each subsequent digit
7. Double-click P to skip to decimal part

**Decimal Part:**
1. Cursor moves to first decimal position
2. Same procedure as integer part
3. Double-click P when finished to proceed to second number

#### 3. Second Number Entry

1. LCD displays "Number 2" on line 1 for 1 second
2. Follow same entry procedure as first number
3. Double-click P after entering decimal part to execute calculation

#### 4. Calculation & Results

1. LCD displays "=" sign
2. Master CPU sends operands to Co-processor via UART
3. Co-processor performs division
4. Co-processor returns result to Master CPU
5. LCD displays:
   - Line 1: "Result"
   - Line 2: Division result

#### 5. Result Navigation

While result is displayed:
- **Single click P**: Cycle through displays
  - Result
  - First Number
  - Second Number
- **Double-click P**: Start new calculation (return to step 2)

### Input Examples

**Example 1: Simple Division**
- First number: 123456.789012
- Second number: 654321.987654
- Result: 0.188743 (approximate)

**Example 2: Using Auto-Fill**
- Enter first digit: 5 (wait 1 second)
- Auto-fills to: 555555.555555
- Modify specific digits as needed

## 📁 Project Structure

```
PIC16F877A-Division-Calculator-using-asm/
├── Master.X/                      # Master CPU project
│   ├── main.asm                   # Main program logic
│   ├── LCD_DRIVER.inc             # LCD control routines
│   ├── LCD_PIN_SETTINGS.inc       # LCD pin configuration
│   ├── BCD_TO_LCD.inc             # BCD to LCD conversion
│   ├── UART.inc                   # UART communication
│   ├── binary_to_bcd.inc          # Binary to BCD converter
│   ├── bcd_to_binary.inc          # BCD to Binary converter
│   ├── Makefile                   # Build configuration
│   └── nbproject/                 # MPLAB X project files
│
├── FPD4040U.X/                    # Co-processor project
│   ├── div.asm                    # Division logic
│   ├── ALU5B.inc                  # 40-bit ALU implementation
│   ├── UART.inc                   # UART communication
│   ├── Makefile                   # Build configuration
│   └── nbproject/                 # MPLAB X project files
│
├── proteus/                       # Proteus simulation
│   └── realtime.pdsprj            # Circuit schematic
│
├── docs/                          # Documentation
│   └── statement.pdf              # Project requirements
│
└── README.md                      # This file
```

## 🔌 Circuit Setup

### Using Proteus Simulation

1. Open `proteus/realtime.pdsprj` in Proteus Design Suite
2. The schematic includes:
   - 2x PIC16F877A microcontrollers (pre-configured)
   - 16x2 LCD display (connected to Master CPU)
   - Push button with pull-up resistor
   - 4 MHz oscillators with capacitors
   - UART connection between processors (PORTC)
   - All required pull-up resistors

3. Load HEX files:
   - Right-click Master CPU → Edit Properties → Program File → Load `Master.X/dist/.../production/Master.X.production.hex`
   - Right-click Co-processor → Edit Properties → Program File → Load `FPD4040U.X/dist/.../production/FPD4040U.X.production.hex`

4. Run simulation (press Play button)

### Physical Hardware Setup

1. **Breadboard Layout:**
   - Place both PIC16F877A chips on breadboard
   - Connect power rails (VDD to +5V, VSS to GND)

2. **LCD Connection (to Master CPU):**
   - RD0 → LCD RS
   - RD1 → LCD E
   - RD4-RD7 → LCD D4-D7
   - Add 4.7KΩ pull-up resistor to RS pin
   - Connect LCD VDD, VSS, and contrast pin (VO)

3. **Button Connection:**
   - One terminal to RB0 on Master CPU
   - Other terminal to GND
   - 10KΩ pull-up resistor between RB0 and VDD

4. **UART Connection:**
   - Master RC6 (TX) → Co-processor RC7 (RX)
   - Master RC7 (RX) → Co-processor RC6 (TX)
   - Common GND between both processors

5. **Oscillators:**
   - Connect 4 MHz crystal to OSC1/OSC2 on each PIC
   - Add 15pF capacitor from each OSC pin to GND

6. **MCLR Pins:**
   - Connect 10KΩ pull-up resistor from each MCLR to VDD

## 🎓 Educational Value

This project demonstrates:
- **Embedded Systems Design**: Real-time interrupt handling and state machines
- **Assembly Programming**: Low-level PIC microcontroller programming
- **Inter-Processor Communication**: UART protocol implementation
- **Floating-Point Arithmetic**: Software implementation of division on 8-bit MCU
- **User Interface Design**: LCD display control and button debouncing
- **Digital Circuit Design**: Complete microcontroller system with peripherals

## 📝 License

This project is part of an academic assignment for:
- **Course**: ENCS4330 - Real-Time Applications & Embedded Systems
- **Institution**: Birzeit University, Faculty of Engineering and Technology
- **Department**: Electrical & Computer Engineering
- **Semester**: 2nd Semester, 2024/2025
- **Instructor**: Dr. Hanna Bullata

## 🤝 Contributing

This is an academic project. For questions or suggestions, please contact the project maintainer.

## ⚠️ Troubleshooting

### Build Errors
- Ensure all `.inc` files are in the correct project directory
- Verify include paths in MPLAB X project properties
- Check that the correct PIC16F877A device is selected

### LCD Not Displaying
- Verify pin connections match `LCD_PIN_SETTINGS.inc`
- Check LCD contrast adjustment (potentiometer)
- Ensure 4-bit mode is properly initialized

### UART Communication Issues
- Verify TX/RX cross-connection between processors
- Check baud rate settings in UART.inc (must match on both CPUs)
- Ensure common GND connection
- Verify oscillator frequency (affects UART timing)

### Button Not Responding
- Check button debouncing logic
- Verify pull-up resistor connection
- Test with Proteus simulation first

## 📚 Additional Resources

- [PIC16F877A Datasheet](https://www.microchip.com/wwwproducts/en/PIC16F877A)
- [MPLAB X IDE Documentation](https://www.microchip.com/mplab/mplab-x-ide)
- [Proteus Design Suite](https://www.labcenter.com/)
- Course materials: See `docs/statement.pdf` for detailed requirements

---

**Project Status**: ✅ Complete and functional

**Last Updated**: June 2025