https://crackmes.one/crackme/619eda7b33c5d455dece628d

### Task

Find a key

### Solution

Load the file into Cutter and look at the graph of main(). At address 0x000011a6 there is a call to strcmp() to compare the entered string and the string located at address 0x00002027. It contains "KS-LICENSE-KEY-2021-REV-1".

I entered KS-LICENSE-KEY-2021-REV-1 and got the message "Congratulations ! You have successfully registered your premium service".
