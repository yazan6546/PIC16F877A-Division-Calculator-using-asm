# Hardware Requirements and Pin Configuration

## Components List

### Required Components

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

## Pin Configuration

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

## Circuit Setup

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
