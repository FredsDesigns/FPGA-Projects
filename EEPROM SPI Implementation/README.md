The "eeprom_spi_controller.txt" file contains the verilog code used on Xilinx Vivado (2024.2) to program a Basys 3 Board. It has the capabaility to perform EWEN/EWDS/Write/Read when connected with a proper AT93C46D EEPROM (In 8-bit mode)

*Please note this project focuses on the 8-bit mode of the EEPROM, please view the data sheet attached in the project folder for timings/command/setup verification!*

The code is separated dual finite state machine architecture to handle the inputs made, and the commands that are assigned to those specific inputs.
On the input side, its responsible for reading in from switches 7 down to switch 0 (This was done to send the max amount of bits at any point; Opcode requires 2 / Address requires 7 / Data requires 8)
It also has counters to time debouncing periods to produce clean push button confirmation signals.
Three push buttons are used (one to reset, another to store combined inputs, and the final button to release all inputs in a sequence following synchronized clock timing)

On the command side, it handles the different spi commands that can be sent to the eeprom, automatically handling the bits transfered/recieved.
For example, based on your selected opcode, it will recognize that only a certain amount of bits are expected, and in the case of EWEN, the mandatory address bits (11XXXXX) are already assigned when the Opcode (00) is selected.
After a succesful execution of any command (EWEN/EWDS/READ/WRITE) it will recognize this and return to idle, meaning you can input another command starting from the opcode.
The previous data and address will be shown until a command that updates either is recorded.

A valid input example is as follows:
First we need to send the Opcode (We will send a write command 01)
We adjust the switches to match this (switch 0 is 1, switch 1 is 0)
Then with a press of the upper push button, we can store this input
The command side recognizes that we have sent the opcode, so it will store 101, automatically including the start bit
Then we can adjust the switches to any address, once we have set our address we can press the upper push button again
For the sake of our example lets say were storing a Hex address of 0x10
The corresponding LEDs on board will light to show the stored address, and the command with store the entire bit-string to this point (101 0001010)
We do the same for our data, lets say we are storing 255 = 0xFF
All switches will be pulled high, and once the push button is pressed, the seven segment displays will properly update to show what your switch input is currently set to.
At this point the bit string holds (101 0001010 11111111)
With a press of the left push button, you can submit this entire bit string to the connected eeprom, and by using a read command at address 10, verify that indeed 0xFF is stored under that address, as shown on the seven segment display!

*To see a proper read signal (Address 00 / Data 10) please see the attached screenshot in the project folder!*
