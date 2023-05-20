https://crackmes.one/crackme/645d3d4e33c5d43938913079

### Task

Find password

### Solution

Load file into Cutter and look at the graph of main().

We see an input prompt at 0x00001181 and a comparison of numbers at 0x000011ad. If the input number is equal to dword[var_ch], then we get the flag.

What is the value of dword[var_ch]? Let's look at 0x0000115d: the initial value is 2, then it multiplied in a loop. If you wish, you can count all 18 iterations by hand, but I will write a simple C program for this:

```c
#include <stdio.h>
#include <inttypes.h>

int main() {
	int64_t var_10h;
	uint64_t var_ch;

	var_ch = 2;
	var_10h = 2;

	while (var_10h <= 0x13) {
		var_ch *= var_10h;
		var_10h++;
	}

	printf("%" PRIu64 "\n", var_ch);
	printf("%d\n", (int)var_ch);

	return 0;
}
```

Why `uint64_t`? It was mentioned here:

```asm
int main (int argc, char **argv, char **envp);
; var int64_t var_14h @ stack - 0x14
; var int64_t var_10h @ stack - 0x10
; var uint64_t var_ch @ stack - 0xc
```

```bash

$ ./pass
243290200817664000
219283456

```

There are 2 compatible passwords. The second password is just 32-bit int overflow. Let's check them out.

```bash
$ ./a.out
Enter a number : 243290200817664000
There is the flag : I_LOVE_YOU

$ ./a.out
Enter a number : 219283456
There is the flag : I_LOVE_YOU
```

Perfect!
