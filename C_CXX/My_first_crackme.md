https://crackmes.one/crackme/644af68433c5d43938912c6b

### Task

Find password

### Solution

Load file into Cutter and look at the strings list.

At 0x00002004 we see this string along with the strings "Welcome!" and "Go away!":

```asm
;-- str.P_ssword:
0x00002004          .string "P@ssword" ; len=9
```

Let's see how this string changes in the code:

At address 0x00001343 we have the original string "P@ssword", then at address 0x00001354 we make a copy of this string, the pointer to the new string is now in RAX. At address 0x0000135d, we replace the second char in the string with 0x61, which means 'a' in ASCII.

The correct password is "Password", I entered it and got the message "Welcome!". By the way, it works with any phrase that starts with "Password", even if you type "PasswordTHIS_WILL-BE+ACCEPTED" because that's how fgets() trims user input.
