# John-Deere-Tractor-Driving-Simulator
John Deere Tractor Driving Simulator developed with the Unity game engine included with vehicle control via a Terasic DE10-Lite FPGA board programmed with Quartus Prime software VHDL.

---

The agricultural sector and its products are a cornerstone of the sustainability of human survival and activity due to its provision of energy and nutrients required by the human population scattered all across the world. In this project, we collaborated with John Deere, one of the most important global corporations in this sector, and delved into programmable logic devices, with an FPGA as the main tool for digital design.

The design of a user interface composed of a configurable logic device, the **Field Programmable Gate Array**, provided two useful tools and a unique enriching experience for the creation of prototypes necessary to validate ideas, concepts, and solutions to real world problems. The **DE10-Lite FPGA Board** was fundamental in our project and facilitated the creation of electronic device prototypes with embedded systems in an efficient and economic form. FPGAs allow for configuration and re-configuration of hardware according to the specified needs of the project without the need to manufacture multiple testing chips. This not only optimized our resources, but also opened us to a wide array of opportunities for innovation and practical learning, essential in the technological industry.

![DE10-Lite Board](https://github.com/user-attachments/assets/aee6c0af-6d8f-406a-87b8-1e93b461cace){ width="800" height="600" style="display: block; margin: 0 auto" }

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

### Equality and Relational Operations

---

## Board Programming

### de10_lite.vhd
