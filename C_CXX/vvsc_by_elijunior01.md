https://crackmes.one/crackme/5ab77f6233c5d40ad448c9e3

### Task

Find password

### Solution

Load file into Cutter and look at the graph of main() function. At address 0x00400a98 there is JE command to compare the entered value with number 0xd80b1. This means that the numeric password is 884913, I entered it and got the message "Allowed access".
