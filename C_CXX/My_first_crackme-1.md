https://crackmes.one/crackme/64435ef833c5d43938912ad7

### Task

Find password

### Solution

Load file into Cutter and look at the strings list.

At 0x140003430 we see this weird string right after "Key: " prompt string:

```asm
;-- str.fdooFPOkfpO_90PFJIKpofj9_O0PFJ_OPjkfopj_OPFJ_pfjOPJFOPjfopJFPOjfopk_FolpjfoFjfop_lJKFOPjfpKFLK_jofjKOLP_FJKLjfklFJKf:
0x140003430          .string "fdooFPOkfpO[90PFJIKpofj9[O0PFJ[OPjkfopj[OPFJ[pfjOPJFOPjfopJFPOjfopk]FolpjfoFjfop[lJKFOPjfpKFLK;jofjKOLP'FJKLjfklFJKf" ; len=117
```

This string is not changed in main() and is used to compare with the user input at 0x14000130e. Thus, the password is

`fdooFPOkfpO[90PFJIKpofj9[O0PFJ[OPjkfopj[OPFJ[pfjOPJFOPjfopJFPOjfopk]FolpjfoFjfop[lJKFOPjfpKFLK;jofjKOLP'FJKLjfklFJKf`

The program displays the message "Key is correct".
