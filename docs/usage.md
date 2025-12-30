# Usage Guide

## System Operation Flow

### 1. Power-On Sequence

When the system is powered on:

1. LCD displays "Welcome to" on line 1
2. LCD displays "Division!" on line 2
3. Message blinks 3 times with 0.5-second intervals
4. After 2-second delay, system moves to input mode

### 2. First Number Entry

#### Integer Part Entry

1. LCD displays "Number 1" on line 1
2. Cursor blinks on line 2 at the most significant digit position
3. Press button P to increment digit (0 → 1 → 2 → ... → 9 → 0)
4. Wait 1 second without pressing to lock the digit
5. After locking, remaining digits auto-fill with the same value
   - Example: If first digit is 6, auto-fills to 666666
6. Continue pressing button to modify each subsequent digit
7. Double-click button P to skip directly to decimal part

#### Decimal Part Entry

1. After integer part, cursor moves to first decimal position
2. Follow same procedure as integer part
3. Auto-fill works the same way for decimal digits
4. Double-click button P when finished to proceed to second number

### 3. Second Number Entry

1. LCD displays "Number 2" on line 1 for 1 second
2. Follow same entry procedure as first number:
   - Enter integer part digit by digit
   - Double-click to move to decimal part
   - Enter decimal part digit by digit
3. Double-click button P after completing decimal part to execute calculation

### 4. Calculation and Results

1. LCD displays "=" sign briefly
2. Master CPU sends operands to Co-processor via UART
3. Co-processor performs division calculation
4. Co-processor returns result to Master CPU
5. LCD displays:
   - Line 1: "Result"
   - Line 2: Division result

### 5. Result Navigation

While result is displayed on screen:

- **Single click button P**: Cycle through displays
  - First click: Shows Result
  - Second click: Shows First Number
  - Third click: Shows Second Number
  - Fourth click: Back to Result
  - (continues cycling)

- **Double-click button P**: Start new calculation (returns to step 2)

## Button Input Guide

### Single Click
- Increments the current digit (0→1→2→...→9→0)
- Used during number entry

### Hold (1 second without clicking)
- Locks the current digit
- Moves to next digit position
- Triggers auto-fill of remaining digits

### Double-Click
- Skips to next major section:
  - From integer part → decimal part
  - From decimal part → next number
  - From result display → new calculation

## Input Examples

### Example 1: Simple Division

**First Number**: 123456.789012
1. Enter first digit: 1 (click once, wait 1 second)
2. Auto-fills to: 111111.111111
3. Modify digits as needed: 123456.789012
4. Double-click to proceed

**Second Number**: 654321.987654
1. Follow same procedure
2. Enter and modify digits
3. Double-click to calculate

**Result**: Approximately 0.188743

### Example 2: Using Auto-Fill Feature

**First Number**: 555555.555555
1. Enter first digit: 5 (click 5 times)
2. Wait 1 second (locks and auto-fills)
3. Result: All digits become 5
4. Keep as is or modify specific digits
5. Double-click to proceed

**Second Number**: 111111.111111
1. Enter first digit: 1 (click once)
2. Wait 1 second (locks and auto-fills)
3. Result: All digits become 1
4. Double-click to calculate

**Result**: 5.0

### Example 3: Quick Entry

**First Number**: 100000.000000
1. Enter first digit: 1 (click once, wait)
2. Auto-fills to: 111111.111111
3. Change second digit to 0, wait
4. Changes remaining to: 100000
5. Double-click to skip to decimal
6. First decimal digit is 0 (already there)
7. Double-click to proceed

## Tips

1. **Use auto-fill wisely**: If many consecutive digits are the same, lock early
2. **Double-click to skip**: Save time by skipping to next section
3. **Watch the cursor**: Blinking cursor shows current digit position
4. **Check before calculating**: Review numbers during result navigation
5. **Timeout is 1 second**: Don't wait too long between button presses when entering different digits

## Error Cases

### Division by Zero

If second number is zero (000000.000000):
- System behavior depends on implementation
- Check co-processor error handling

### Invalid Input

- System validates input automatically
- Cannot enter values exceeding 999999.999999
- Each digit wraps from 9 to 0

## Troubleshooting

### Button Not Responding

- Check button debouncing logic
- Verify pull-up resistor connection (10KΩ)
- Test with Proteus simulation first

### LCD Not Displaying

- Verify pin connections match configuration
- Check LCD contrast adjustment
- Ensure 4-bit mode is properly initialized
- Verify power supply to LCD (5V)

### Incorrect Results

- Verify UART communication between processors
- Check TX/RX cross-connection
- Ensure common GND between both processors
- Verify oscillator frequency (affects UART timing)
