This code includes multiple parts that form the completed Memory System. This project was done using the Altera DE2 Cyclone 2 board so be aware if using a different system.

Part 1A focuses entirely on pulsing to different instructions. What the goal to do here was, to be create the logic needed to properly shift through each memory address that you can see in the memory file. A visualization of this is done using the on board LEDs to show the address we are currently on. So everytime we pulse with the internal clock and a push button to confirm, the board should be increasing the LED count to mirror the next addresses we visit. 

Part 1B is using the logic we created in Part 1A and introducing the actual addresses from the uROM file. If all is done well we can see it access the various addresses as such: 00 -> 01 -> 02 -> AA -> AB. Pulsing at address AB should allow us to return back to address 00, that way we can continually manipulate the addresses.

Part 2 takes the work done in part 1B and allows us to write to adresses, that way if we have wrote to an address and revisit it once more, we will see the data we have written is now saved at that specific address. Please make sure to involve the new RAM file as you will need it to save data, and update your uROM file to match.
