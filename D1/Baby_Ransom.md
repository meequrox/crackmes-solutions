https://crackmes.one/crackme/5ab77f6233c5d40ad448c9e3

### Task

Write a decryptor for .corona files using the language of your choice

### Solution

Load file into Cutter and look at the graph of main().

Let's see how each file is processed. In code block 0x00000de8 we have two calls to fopen, then in code block 0x00000e3f we have a call to _IO_getc, which puts the current char from the file into variable byte[c]. In the next code block 0x00000e67 we do XOR operation between byte[c] and the unknown value from AL register and then write the new byte[c] to the output file.

Now we need to determine the value in AL register that is used in the XOR. The command at 0x00000e67 puts the value from the 'var_ach' variable into EAX register. Go back to the main() code block and find the 'var_ach' variable. It is initialized at address 0x00000cfd, then some arithmetic operations are done with it, and the final value is written to the variable at address 0x00000d4d. I ran this section through the debugger and determined the final value of the 'var_ach' variable to be 0x5c. Let's go back to address 0x00000e67, the value of AL is the lowest 8 bits of EAX, so it's also 0x5c.

Now it is possible to write a program that will restore the original files. I used the C language for this, let me quickly explain how it works:

1. Get the list of files in the current working directory.
2. For each file in the list:
    1. If it is not regular file with ".corona" extension, skip that file.
    2. XOR each byte of the file with value 0x5c.
    3. Remove the ".corona" extension from the filename.

```c
#include <dirent.h>
#include <errno.h>
#include <libgen.h>
#include <stdio.h>
#include <string.h>

#define CHUNK_SIZE 1024

void decrypt(const char* filename) {
    FILE* f;
    char buf[CHUNK_SIZE];
    int bytes;
    long int fpos;

    f = fopen(filename, "r+");
    if (!f) {
        perror(filename);
        return;
    }

    while (1) {
        if (feof(f)) break;
        if ((bytes = fread(buf, 1, CHUNK_SIZE, f)) < 1) break;

        for (int i = 0; i < bytes; i++) buf[i] ^= 0x5c;

        fpos = ftell(f);
        fseek(f, fpos - bytes, SEEK_SET);
        fwrite(buf, 1, bytes, f);

#if 0
        printf("%s %s: XOR'ed bytes %ld-%ld\n", __FUNCTION__, filename, fpos - bytes, fpos);
#endif
    }

    fclose(f);
}

int main(int argc, char* argv[]) {
    struct dirent* de;
    DIR* dr;
    char orig_filename[PATH_MAX];
    char* ext;

    if ((dr = opendir("."))) {
        while ((de = readdir(dr))) {
            ext = strrchr(de->d_name, '.');

            if (ext && strcmp(ext, ".corona") == 0 && de->d_type == DT_REG) {
                strcpy(orig_filename, de->d_name);
                orig_filename[ext - de->d_name] = '\0';

                decrypt(de->d_name);
                rename(de->d_name, orig_filename);

                printf("%s -> %s\n", de->d_name, orig_filename);
            }
        }

        closedir(dr);
    }

    return 0;
}
```

```bash
$ sha256sum Soundproof.jpg
6b0b3e9cee245e0258eed95d8c4e1613672084b9be0cab3b0c43a5b4f94221f6  Soundproof.jpg

$ ./covid-19.bin

$ sha256sum Soundproof.jpg.corona
7d03f68e929142ab314654a4b682f0bd2d8b7d15ff034f695554654e3afc36ae  Soundproof.jpg.corona

./anticovid.bin
Soundproof.jpg.corona -> Soundproof.jpg

$ sha256sum Soundproof.jpg
6b0b3e9cee245e0258eed95d8c4e1613672084b9be0cab3b0c43a5b4f94221f6  Soundproof.jpg
```

It restores the original file back, so it works.
