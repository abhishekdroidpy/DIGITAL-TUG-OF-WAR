# Digital Tug of War

An interactive two-player tug-of-war game built with an Arduino, a four-bit up/down counter, pushbuttons, LEDs, and a buzzer. Each valid button press moves the illuminated marker one position toward that player's side. The first player to reach an endpoint wins.

## Project overview

The project demonstrates how digital counting and microcontroller control can be combined in a simple game. The Arduino reads and debounces the player and reset buttons, generates clock pulses for the counter, reads the counter state, and lights the matching position LED. A buzzer provides an audible endpoint indication.

The physical circuit uses a **74LS193** as its four-bit up/down counter. The Tinkercad version implements the counter with flip-flops and basic logic gates because the 74LS193 model was unavailable there. The simulation counter is therefore a gate-level representation of the counter function, not a separate game subsystem.

## Components

- Arduino Uno
- 74LS193 four-bit up/down counter (physical circuit)
- Flip-flops and basic logic ICs to model the counter in Tinkercad
- Three pushbuttons: Player 1, Player 2, and reset
- Nine position LEDs: four for each side and one at the centre
- Current-limiting and pull-up/pull-down resistors as used in the circuit
- NOT-gate stages and buzzer
- Breadboards, jumper wires, and a regulated 5 V supply

## How it works

1. **Reset:** The counter is cleared to `0000`, lighting the centre LED.
2. **Player input:** A press is debounced in software so that one press is treated as one move.
3. **Counting:** The Arduino sends a pulse to the counter's up or down clock input. The 74LS193 updates its four-bit state by one count.
4. **Position display:** The Arduino reads the counter's Q outputs and maps the state to one of the nine position LEDs.
5. **Win indication:** At an endpoint, the endpoint indicator signal passes through two cascaded NOT gates to the buzzer. Two inversions restore the signal's original logical polarity.

## Position encoding

The states below are shown from the left endpoint to the right endpoint. The centre state is `0000`.

| Position | Counter state |
| --- | --- |
| Left endpoint | `1100` |
| Left 1 | `1101` |
| Left 2 | `1110` |
| Left 3 | `1111` |
| Centre | `0000` |
| Right 1 | `0001` |
| Right 2 | `0010` |
| Right 3 | `0011` |
| Right endpoint | `0100` |

## Arduino pin map

This table reflects the final sketch shared for the project.

| Arduino pin | Connection |
| --- | --- |
| D4 | Player 1 / UP button; other button terminal to GND |
| D3 | Reset button input; other button terminal to GND |
| D2 | Player 2 / DOWN button; other button terminal to GND |
| D1 | 74LS193 CPU / up-clock input, pin 5 |
| D0 | 74LS193 CPD / down-clock input, pin 4 |
| A0 | 74LS193 MR / reset input, pin 14 |
| A4 | Counter Q0, 74LS193 pin 3 |
| A3 | Counter Q1, 74LS193 pin 2 |
| A2 | Counter Q2, 74LS193 pin 6 |
| A1 | Counter Q3, 74LS193 pin 7 |
| D13–D5 | Nine position LEDs, each with a series resistor |

The 74LS193 is powered from +5 V at pin 16 and GND at pin 8. Arduino and counter grounds must be common. Pins used for parallel loading and the parallel data inputs must be held at the levels required by the counter circuit; see the project schematic for the exact wiring.

> D0 and D1 are also the Uno's hardware serial pins. If uploading the sketch becomes unreliable, disconnect the counter clock wires from D0 and D1 during upload, then reconnect them.

## Tinkercad implementation

Tinkercad did not provide the 74LS193 component used in the physical build. To simulate its behavior, the counter was constructed from flip-flops and basic gates. The four counter output LEDs on that section are for observing the binary count; they are separate from the nine game-position LEDs.

## Notes

- The player buttons use the Arduino's internal pull-ups in the shared sketch and are wired to ground when pressed.
- Each position LED needs a series current-limiting resistor.
- The buzzer path described here uses two NOT gates in series from the endpoint indicator signal; it is not a two-input endpoint-combining OR function.

