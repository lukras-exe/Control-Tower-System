# Mini Control Tower Project

## Overview
This miniproject implements a digital control tower system for managing aircraft on taxiways. The system tracks the number of aircraft queued on two different taxiways, manages aircraft departures for takeoff, handles landing aircraft, and includes timing functionality for runway operations.

## Project Components
The project consists of several interconnected AHDL modules integrated into a top-level block diagram:

### 1. FreqDiv (Frequency Divider)
- Generates a 1Hz clock signal from the 50MHz system clock
- Uses the DIVN module from lab 7 with N=50,000,000
- Provides timing pulses for the rest of the system

### 2. UpDownCounter
- Two independent 8-bit counters for tracking aircraft in each taxiway
- Increments when aircraft are added to taxiway queue
- Decrements when aircraft depart the taxiway for takeoff

### 3. Mod60 (Modulo 60 Counter)
- 8-bit counter that counts seconds from 0 to 59
- Used to implement a 60-second (1-minute) delay for runway operations
- Based on examples from Lecture 17

### 4. StateMachine
- Controls the overall system operation
- Manages transitions between states:
  - Initial (reset) state
  - Adding aircraft to taxiway queues
  - Departing aircraft for takeoff
  - Landing aircraft operations
  - Delay timing
  - Other operational states

### 5. Binary2BCD
- Converts 8-bit binary counter values to BCD format for display
- Used for both taxiway queue counters and the 60-second countdown timer
- Outputs three 4-bit BCD digits for each input

### 6. Serial7Seg
- Four BCD-to-7-segment decoders:
  - Two for displaying number of aircraft in each taxiway
  - Two for displaying the 60-second countdown timer

## System Integration
The project integrates all components in the top-level project file "mproject". Key aspects of integration include:

- **Synchronous Cascading**: All modules connect to the 50MHz system clock
- Slower timing controlled through the Terminal Count (TC) output of the frequency divider
- The TC signal connects to the enable inputs of modules requiring slower timing:
  - State machine
  - Up/Down counters
  - Mod60 counter

## Display Configuration
- **Taxiway Displays**: Each taxiway counter connects to a Binary2BCD converter, which then connects to the Serial7Seg decoder for display. Only the least significant 4-bit BCD result is used.
- **Countdown Timer**: The Mod60 counter connects to a Binary2BCD converter, which then connects to two Serial7Seg decoders for display. The two lower 4-bit BCD results are used.

## Operation
1. The system starts in a reset state
2. Based on inputs, aircraft can be added to either taxiway queue
3. Aircraft can depart from taxiways for takeoff
4. A 60-second delay is enforced for certain operations
5. Displays show current status of taxiways and countdown timer

## Hardware Requirements
- DE0 FPGA board or equivalent
- Four 7-segment displays (on board)
- Input switches/buttons for control (on board)

## Setup and Usage
1. Open the `mproject.qpf` file in Quartus
2. Compile the project
3. Program the FPGA board
4. Use the designated input switches/buttons to:
   - Add aircraft to taxiways
   - Signal departures
   - Handle landings
   - Reset the system

## Design Notes
- All counters use 8-bit outputs
- Synchronous design principles are applied throughout
- The state machine controls all operations based on input signals
- The 1Hz clock derived from the 50MHz system clock controls the timing
