https://crackmes.one/crackme/5fca1e7933c5d424269a1a68

### Task

Find passcode

### Solution

Load file into Cutter and look at the graph of main() function. At address 0x000011a6 there is JNE command to compare the entered value and variable with value 0x130104. This means that the numeric password is 1245444, I entered it and got the message "right! Good job!".
