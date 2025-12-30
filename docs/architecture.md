# Software Architecture

## Overview

The system uses a dual-microcontroller architecture where responsibilities are divided between a Master CPU and a Co-processor.

## Master CPU (Master.X)

### Responsibilities
- LCD initialization and display management
- User input handling (button debouncing and state management)
- Number entry state machine
- UART communication protocol (send operands, receive results)
- BCD ↔ Binary conversion
- Display formatting and cursor control

### Key Components

- `main.asm`: Main program logic and state machine
- `LCD_DRIVER.inc`: LCD control routines (4-bit mode)
- `BCD_TO_LCD.inc`: BCD to LCD character conversion
- `UART.inc`: UART communication protocol
- `binary_to_bcd.inc`: Binary to BCD conversion
- `bcd_to_binary.inc`: BCD to Binary conversion

### State Machine

The Master CPU implements a state machine with the following states:

- `STATE_FIRST_NUM_INT`: Inputting integer part of first number
- `STATE_FIRST_NUM_DEC`: Inputting decimal part of first number
- `STATE_SECOND_NUM_INT`: Inputting integer part of second number
- `STATE_SECOND_NUM_DEC`: Inputting decimal part of second number
- `STATE_RESULT_RESULT`: Showing division result
- `STATE_RESULT_NUMBER_1`: Showing first number
- `STATE_RESULT_NUMBER_2`: Showing second number

## Co-processor (FPD4040U.X)

### Responsibilities
- Receive operands via UART
- Perform 40-bit floating-point division
- Send results back via UART
- Implement acknowledgment protocol

### Key Components

- `div.asm`: Main division logic and UART handling
- `ALU5B.inc`: 5-byte (40-bit) arithmetic logic unit
- `UART.inc`: UART communication protocol

### Division Algorithm

The co-processor implements a 40-bit (5-byte) division algorithm that handles floating-point numbers with high precision.

## Communication Protocol

The processors communicate using UART with an interrupt-driven acknowledgment protocol:

1. **Master → Co-processor**: Send number of bytes
2. **Co-processor → Master**: Send acknowledgment
3. **Master → Co-processor**: Send data bytes (one at a time)
4. **Co-processor → Master**: Acknowledge each byte
5. **Co-processor → Master**: Send result bytes (one at a time)
6. **Master → Co-processor**: Acknowledge each byte

### UART Configuration

Both processors use:
- Baud rate: Configured in UART.inc
- 8-bit data, no parity, 1 stop bit
- Interrupt-driven communication

## Data Format

### Number Representation

Numbers are represented internally in two formats:

1. **BCD Format** (6 bytes): Used for display and input
   - 6 digits for integer part (000000-999999)
   - 6 digits for decimal part (.000000-.999999)

2. **Binary Format** (5 bytes/40 bits): Used for calculations
   - Converted from BCD for transmission to co-processor
   - Converted back to BCD for display after receiving results

## Interrupt Handling

### Master CPU Interrupts

- Timer1 interrupt: Used for timing (1-second timeout for digit entry)
- UART receive interrupt: Used for receiving data from co-processor
- External interrupt (RB0): Used for button press detection

### Co-processor Interrupts

- UART receive interrupt: Used for receiving operands from master CPU
