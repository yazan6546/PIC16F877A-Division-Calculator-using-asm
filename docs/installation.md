# Installation and Build Instructions

## Prerequisites

### Required Software

1. **MPLAB X IDE** (v5.0 or later)
   - Download from [Microchip's website](https://www.microchip.com/mplab/mplab-x-ide)
   - Includes MPASM assembler

2. **Proteus Design Suite** (optional, for simulation)
   - Used for circuit simulation and testing
   - Download from [Labcenter Electronics](https://www.labcenter.com/)

## Installation Steps

### 1. Clone the Repository

```bash
git clone https://github.com/yazan6546/PIC16F877A-Division-Calculator-using-asm.git
cd PIC16F877A-Division-Calculator-using-asm
```

### 2. Open Projects in MPLAB X

1. Open MPLAB X IDE
2. File → Open Project
3. Navigate to `Master.X` folder and open the project
4. Repeat for `FPD4040U.X` folder

### 3. Verify Include Paths

- Ensure all `.inc` files are in the project directory
- Check that include paths are properly configured in project properties

## Build Instructions

### Building Master CPU Code

1. Open `Master.X` project in MPLAB X
2. Select "PIC16F877A" as target device
3. Clean and build the project:
   - Production → Clean and Build Main Project
   - Or use toolbar button
4. Verify successful build (check output window)
5. The HEX file will be generated in `Master.X/dist/default/production/`

### Building Co-processor Code

1. Open `FPD4040U.X` project in MPLAB X
2. Select "PIC16F877A" as target device
3. Clean and build the project:
   - Production → Clean and Build Main Project
   - Or use toolbar button
4. Verify successful build (check output window)
5. The HEX file will be generated in `FPD4040U.X/dist/default/production/`

### Alternative: Command Line Build

From the repository root:

```bash
# Build Master CPU
cd Master.X
make clean
make

# Build Co-processor
cd ../FPD4040U.X
make clean
make
```

## Configuration Bits

Both projects use the following configuration:

- **Oscillator**: XT (4 MHz crystal)
- **Watchdog Timer**: OFF
- **Power-up Timer**: OFF
- **Code Protection**: OFF
- **Low Voltage Programming**: OFF
- **Brown-out Detect**: OFF

Configuration is set in the assembly files using `__CONFIG` directive.

## Programming the Microcontrollers

### Using PICkit or ICD

1. Connect programmer to each PIC16F877A
2. In MPLAB X:
   - Make and Program Device Main Project
   - Or use toolbar button
3. Verify programming success in output window

### Using Proteus Simulation

1. Open `proteus/realtime.pdsprj`
2. Load generated HEX files:
   - Master CPU: `Master.X/dist/default/production/Master.X.production.hex`
   - Co-processor: `FPD4040U.X/dist/default/production/FPD4040U.X.production.hex`
3. Run simulation

## Troubleshooting Build Issues

### Build Errors

- Ensure all `.inc` files are in the correct project directory
- Verify include paths in MPLAB X project properties
- Check that the correct PIC16F877A device is selected
- Make sure MPASM assembler is properly installed

### Include File Not Found

If you get "include file not found" errors:

1. Right-click project → Properties
2. Select "Global Options" under "conf: [default]"
3. Add include paths to your project directory

### Linker Errors

- Check that all required files are included in the project
- Verify memory allocation doesn't exceed PIC16F877A limits
- Review linker output for specific error messages
