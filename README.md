# John-Deere-Tractor-Driving-Simulator
John Deere Tractor Driving Simulator developed with the Unity game engine included with vehicle control via a Terasic DE10-Lite FPGA board programmed with Quartus Prime software VHDL.

---

The agricultural sector and its products are a cornerstone of the sustainability of human survival and activity due to its provision of energy and nutrients required by the human population scattered all across the world. In this project, we collaborated with John Deere, one of the most important global corporations in this sector, and delved into programmable logic devices, with an FPGA as the main tool for digital design.

The design of a user interface composed of a configurable logic device, the **Field Programmable Gate Array**, provided two useful tools and a unique enriching experience for the creation of prototypes necessary to validate ideas, concepts, and solutions to real world problems. The **DE10-Lite FPGA Board** was fundamental in our project and facilitated the creation of electronic device prototypes with embedded systems in an efficient and economic form. FPGAs allow for configuration and re-configuration of hardware according to the specified needs of the project without the need to manufacture multiple testing chips. This not only optimized our resources, but also opened us to a wide array of opportunities for innovation and practical learning, essential in the technological industry.

<p align='center'>
  <img src='https://github.com/user-attachments/assets/aee6c0af-6d8f-406a-87b8-1e93b461cace' />
</p>

For the creation of the software environment of the simulation, we used **Unity**, widely used for videogame development. In addition, to manage version control of the final product and the advancement of multiple conforming aspects of the project, we used **Plastic SCM**. By using these tools, we maintained constant and structured progress and fluid and efficient collaboration. 

---

## State Machine

Finally, a logic state machine was created to represent the behavior of our game implemented in **Quartus Prime** with **Very High Level Descriptive Language**, better known as **VHDL**.

![Simulator State Machine](https://github.com/user-attachments/assets/ea884359-6ad7-404a-937d-3498867ed0a2)

### Initial State Indication

The initial state is declared automatically by initializing the videogame and won't change until receiving the signal from the graphic card that receives information from Unity that the game state is Game Initialize. This takes us to the next state named Game Start which lets the controller interact with a menu with the option to play to starte the game.

### Transitions between States

Once in the initial menu, the controller programmed in the DE-10 FPGA board can select the 'Play' button to change the value of the Game Start state to '0' and the value of userOption to 'play'. This leads us to the first level, and at any moment of the levels, the user can press a 'Pause' button to pause the game by reaching the Game Pause state. In the first level, if the user recollects the indicated amount of crops within the set time limit, they will encounter a 'Congratulations!' screen from the Game Complete state. If the user loses all lives in any level, they will reach a Game Failed state and will go back to Game Start where they have the option to play again, starting from the first level.

### Inputs, Outputs, and Variables

The input and output variables define the state changes such as how gameStart is defined to always lead to Game Start being the output variable that lets the DE-10 Lite board in which state the game is in. The input variable gamePause defines at what moment the game is paused, and stops running the timer and momentarily saves the game progress. The variables 'crop counter', 'number of lives', and 'clock timer' are variables that have values from the state machine and are defined by the game. Finally, gameFailed stops the game and goes back to the gameStart state.

### Arithemtic Expressions

The states change according to the level. Each time a level is passed, the 'level' variable is added '1' to it and the amount of crops to collect is added a specific amount along with the given amount of time.

### Equality and Relational Operations

Equality operations are used for each change of state as the sustem is based on identifying when a button or switched are changed from '1' to '0' or from '0' to '1', for which cases for each are created. The values in the states gameInit and gameStart start at '0' and need to be activated to '1'. Here are the Mealy Machines that compare the corresponding variable to each state with '0' amd '1'. For the game levels, we start by comparing the amount of remaining lives to know if the game will be kept or to the gameFailed state, then the amount of collected crops are constantly compared with the number of objective crops for the level until reaching the desired number as long as the time is not up.

---

## FPGA DE10-Lite Programming

### de10_lite.vhd (Top-Level Entity)
- **Purpose**: Main design for DE10-Lite board.
- **Ports**:
    Internal clock, push buttons (keys), switches (switches).
    Serial communication (TX, RX).
    LEDs, accelerometer (G sensor).
    Motion control (up/down) and RGB colors.
    7-segment displays (game score).
- **Instantiated Components**:
    **UART**: Serial communication.
    **Debounce**: Button noise filtering.
    **Reset Delay**: Reset signal generation.
    **SPI PLL/EEPROM**: SPI configuration and EEPROM management.
    **LED Driver**: LED control based on accelerometer data.
    **Digit Counter**: Data display on 7-segment displays.

### uart.vhd (Serial communication)
- **Purpose**: Enable serial communication between the FPGA and Unity.
- **Ports**: Clock, reset, transmission (TX), reception (RX).
- **Key Signals**:
    **tx_state / rx_state**: State machine states.
    **baud_pulse / os_pulse**: Baud rate control.
    **parity_error**: Error detection.
- **Processes**:
    TX/RX state machines.
    Parity calculation (error detection).

### debounce.vhd (Noise Filtering)
- **Purpose**: Filter noise from button presses.
- **Ports**: Clock, button input, filtered output **(debounced)**.
- **Logic**:
    Pulse counting for signal stabilization **(COUNT)**.
    **done** signal indicates process completion.

### DigitCounter.vhd (Binary to Decimal Conversion)
- **Purpose**: Convert 8-bit binary input to 3 decimal digits (units, tens, hundreds).
- **Components:**
    Decoder.vhd instance to display digits on 7-segment displays.
- **Operations**: Uses modulo and integer division for decomposition.

### Decoder.vhd (4-bit to 7-Segment Decoder)
- **Purpose**: Convert hexadecimal values to 7-segment display patterns.
- **Logic:**
    Direct mapping with with select for each digit (0-F).

### Pin Assignments (Pin Planner)
- **Board**: MAX 10 10M50DAF484C76.
- **Standards**:
    Main signals: 3.3-V LVTTL (8 mA).
    GPIO (TX/RX): 2.5 V (12 mA).
**Mapped Peripherals**:
  Clock (CLOCK_50), accelerometer (GSENSOR), displays (HEXL), LEDs (LEDR), buttons, and switches.

![PinPlanner](https://github.com/user-attachments/assets/a7553692-bbf9-4e35-8ee8-ceccf73aaac4)
![PinPlanner2](https://github.com/user-attachments/assets/996cdbd2-0e36-48eb-98b9-ecced2497890)

### Additional Modules (Provided by Intel)
- spi_ee_config.v, spi_controller.v, reset_delay.v, led_driver.v, DE_LITE_GSensor.v.
- **Purpose**: Accelerometer handling and motion data transmission to Unity.
