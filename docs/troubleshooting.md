# Troubleshooting Guide

## Build and Compilation Issues

### Build Errors

**Symptoms:**
- Compilation fails with errors
- Cannot build project successfully

**Solutions:**
- Ensure all `.inc` files are in the correct project directory
- Verify include paths in MPLAB X project properties
- Check that the correct PIC16F877A device is selected
- Make sure MPASM assembler is properly installed
- Review error messages for specific file or line numbers

### Include File Not Found

**Symptoms:**
- Error: "Cannot open include file"
- Build fails on INCLUDE directive

**Solutions:**
1. Right-click project → Properties
2. Select "Global Options" under "conf: [default]"
3. Add include paths to your project directory
4. Ensure `.inc` files exist in the specified locations

### Memory Overflow

**Symptoms:**
- Linker error about program memory overflow
- Error: "Address exceeds maximum"

**Solutions:**
- Check that code fits in PIC16F877A memory (8K words)
- Review memory allocation in linker output
- Optimize code if necessary

## Hardware Issues

### LCD Not Displaying

**Symptoms:**
- LCD shows nothing or random characters
- Backlight works but no text

**Solutions:**
- Verify pin connections match `LCD_PIN_SETTINGS.inc`:
  - RD0 → LCD RS
  - RD1 → LCD E
  - RD4-RD7 → LCD D4-D7
- Check LCD contrast adjustment (potentiometer on VO pin)
- Ensure 4-bit mode is properly initialized in code
- Verify power supply to LCD (5V on VDD, GND on VSS)
- Check 4.7KΩ pull-up resistor on RS pin

### LCD Displays Gibberish

**Symptoms:**
- LCD shows random or incorrect characters
- Characters appear corrupted

**Solutions:**
- Verify initialization sequence in LCD_DRIVER.inc
- Check timing delays in LCD commands
- Ensure stable 5V power supply
- Verify oscillator frequency (4 MHz)

### Button Not Responding

**Symptoms:**
- Button presses have no effect
- System doesn't react to input

**Solutions:**
- Check button debouncing logic in code
- Verify pull-up resistor connection (10KΩ between RB0 and VDD)
- Test button continuity with multimeter
- Verify RB0 pin connection
- Enable internal weak pull-up resistor if not using external
- Test with Proteus simulation first

### Button Too Sensitive

**Symptoms:**
- Single press registers as multiple clicks
- Difficult to control digit entry

**Solutions:**
- Adjust debouncing delay in code
- Check for hardware bouncing issues
- Add capacitor across button terminals (0.1µF)
- Increase software debounce time

## Communication Issues

### UART Communication Failure

**Symptoms:**
- No data transfer between processors
- Calculation never completes
- System hangs after entering numbers

**Solutions:**
- Verify TX/RX cross-connection:
  - Master RC6 (TX) → Co-processor RC7 (RX)
  - Master RC7 (RX) → Co-processor RC6 (TX)
- Check baud rate settings in UART.inc (must match on both CPUs)
- Ensure common GND connection between both processors
- Verify oscillator frequency (affects UART timing)
- Check UART initialization in both processors
- Test each processor individually first

### Data Corruption

**Symptoms:**
- Incorrect calculation results
- Wrong numbers displayed

**Solutions:**
- Verify UART baud rate calculation
- Check for timing issues (oscillator accuracy)
- Ensure proper acknowledgment protocol
- Add error checking in UART communication
- Verify data format consistency

## Simulation Issues (Proteus)

### Simulation Doesn't Start

**Symptoms:**
- Play button doesn't work
- Simulation immediately stops

**Solutions:**
- Verify HEX files are loaded correctly
- Check for missing components in schematic
- Ensure all connections are proper
- Verify oscillator settings in component properties

### Simulation Runs But No Output

**Symptoms:**
- Simulation runs but LCD shows nothing
- No response to button presses

**Solutions:**
- Check LCD initialization in simulation
- Verify oscillator frequency in component properties (4 MHz)
- Ensure button is properly configured with pull-up
- Check MCLR pin has pull-up resistor

## Runtime Issues

### System Hangs

**Symptoms:**
- System stops responding
- LCD freezes
- No response to button

**Solutions:**
- Check for infinite loops in code
- Verify interrupt handlers return properly
- Ensure watchdog timer is disabled (WDT_OFF)
- Check stack overflow possibility
- Review state machine logic

### Incorrect Calculations

**Symptoms:**
- Division results are wrong
- Numbers don't match expected output

**Solutions:**
- Verify BCD to Binary conversion routines
- Check Binary to BCD conversion routines
- Test division algorithm with known values
- Verify data transmission between processors
- Check for overflow in calculation
- Review 40-bit arithmetic implementation

### Timing Issues

**Symptoms:**
- 1-second timeout doesn't work correctly
- Delays are too fast or too slow

**Solutions:**
- Verify Timer1 configuration
- Check timer preload values
- Ensure oscillator frequency is correct (4 MHz)
- Review interrupt service routine for Timer1
- Verify overflow count calculation

## Power Issues

### Intermittent Operation

**Symptoms:**
- System works sometimes, fails other times
- Random resets or hangs

**Solutions:**
- Check power supply stability (regulated 5V)
- Add decoupling capacitors near each IC (0.1µF)
- Verify current capacity of power supply
- Check for loose connections
- Ensure proper ground connections

### Reset on Button Press

**Symptoms:**
- System resets when button is pressed
- Unexpected behavior on input

**Solutions:**
- Add decoupling capacitor near power pins
- Check for power supply noise
- Verify MCLR pin has proper pull-up (10KΩ)
- Add capacitor on MCLR if needed (0.1µF to GND)

## Getting Help

If issues persist after trying these solutions:

1. Review the project documentation in `docs/` folder
2. Check the original project specification in `docs/statement.pdf`
3. Test individual components separately
4. Use Proteus simulation to isolate hardware vs. software issues
5. Review assembly code for logical errors
6. Check MPLAB X IDE output window for detailed error messages

## Additional Resources

- [PIC16F877A Datasheet](https://www.microchip.com/wwwproducts/en/PIC16F877A)
- [MPLAB X IDE Documentation](https://www.microchip.com/mplab/mplab-x-ide)
- [Proteus Help Documentation](https://www.labcenter.com/)
