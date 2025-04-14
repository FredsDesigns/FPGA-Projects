Please note that the code shared in this directory could be capable of being used on other hardware, but was developed with the Altera DE2 board on Quartus II software. 
This code is split between two parts for visibility and vertification of the addition and subtraction components.
It is also shared on one file, but I highly recommend you split it amongst seperate files that could be reused and called in the main.

To start things off, the first part has four parts:
1. Main file
   2. P-Adder
   3. Full-Adder
   4. Hex Display
The main file is used for both entity and architecture declartion. Both require the file name which in this case is called "LAB6", so if you decide to change the name for better declaration for either that or any other name, please make sure every instance is changed to match your new title.

Your main file is used to declare the ports used, in this case, I used the onboard switches to handle numeric inputs / selection of mode, and seven-segment displays (sSD) to handle the output results/numbers selected from binary to decimal. Since our sSD's require 7 input pins to control each of their LED's, we use the declaration of std_logic_vector, and declare it "out" to make sure its set as an output. (0 to 6) indicates seven unique spots are registered for each port name so for example:
seg0, seg1, seg2: out std_logic_vector(0 to 6) makes seg0/seg1/seg2 all outputs that are vectors with seven (0/1/2/3/4/5/6) dedicated lines to each.

Our architecture is where we can make references to call our other two files p_adder and hex. the line "component p_adder" calls the file, and then listed after is the entity code found in the file your calling. You'll notice that the p_adder is using a generic(n:positive), this means that rather then creating hundreds of specific lines of code to handle every single unique input, the code will update based on the amount of inputs used, its very necessary when working with variable inputs, and outputs.

Signals are a bit like your inputs and outputs, except they are internal signals the user will not see, but knows occured. Essentially they are used to store result data, that can be used to confirm conditionals, and help input to output processes without directly altering them.

Now we see p1: p_adder generic map(n => 8) ... This is a call to the p_adder declaring that n is equal to and greater than 8, you can edit this to alter the exact size of the p_adder you can use in your system, however, recall in the initial code that it takes a,b,c_in,sum,c_out, where one element like a is an input "in std_logic_vector((n-1) downto 0);
Using the 8 here means its actually a vector from 7 to 0 (7/6/5/4/3/2/1/0), the size is still 8 but it doesnt begin from 8.
Either way, you will include the port map after, which doesnt need to be altered, but what were essentially doing here is making sure that every element in the original component call is checked off.

We can then used h1,h2,h3, which are calls to the hex port map, to show the results and the carry if there is one. hex0 takes care of the lower four bits, while hex1 takes care of the higher four bits.

Finally lets understand the three parts. The P_adder which we know is used in the main file, handles the initial call with the inputs from the boards switches, and will generate the resulting carry from the p_adder's c_int(n), where the full adder is simply a recreation of a regular full adder circuit, so sum and c_out will handle the xor/and/or logic on the inputs and deliver its results back.

The full adder logistics are as follows:
take a = 01100101 and b = 00010010. In the first iteration, it will sum the beginning of each a(0) and b(0)
which are bits 1 and 0 from 0110010(1) and 0001001(0). This results in a sum of 1 and a carry of 0, so it will keep the sum value stored as sum(0) = 1, and c_out(1)=0. If there was a carry in bit assigned the process would account for that initial carry as well. This will continue until all sums and carrys are calculated and the final carry is stored to c_out from c_int(n) to be displayed on the sSD.

Finally the hex code is simply a case statement that will map four bits at a time to translate them to their proper seven segment display value. values 0 thru 9 will appear as numbers but recall that A = 10, b = 11, C = 12, d = 13, E = 14, F = 15. These will then light up the two sSD's with the proper summed values while also using one display to show the extra carry.

That's it for parts 1 understanding of the code! Now all you need to do is upload the pin planner inputs and properly map the switches for input a/b/c_in, and then you can program your board to add the bit values!

Part 2 is simply the subtraction version, it does the addition from the first portion but now also does subtraction with a switch dedicated to choosing which mode you want to use. It  still makes usage of the p-adder/full-adder/hex converter so please dont delete those files.

Only key note difference is this uses twos complement subtraction and counts for overflows, the handling is not much different from the addition however, so you should be able to understand its process.
