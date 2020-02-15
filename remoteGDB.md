
# Remote GDB Debugging Without Cross Compiling 

The following demonstrates a simple example where a target is built on an embedded target and then debugged remotely. 

### Create hello.c 

First create a `hello.c` on your raspberry PI: 

```c
#include <stdio.h>
int main() {
   printf("Hello, World!\r\n");
   return 0;
}
```

### Create exc 

Compile hello.c with `gcc`

```console
gcc -g -o hello hello.c
```
> Note `-g` builds a debug application and `-o` denotes the output exc. 

One can run the exc

```console
./hello
Hello, World!
```

### Spin up GDB Server

If not already installed, install `gdbserver`

```console
sudo apt-get install gdbserver -y
```

Next start the program on a port of your choice pointing at your exc. 

```console
gdbserver localhost:2001 hello
```

One should see an output similar to the following: 

```console
pi@pi:~/hello $ gdbserver localhost:2001 hello
Process hello created; pid = 18371
Listening on port 2001

```

### Connecting to GDB Server With Windows Machine via CMD

First one must download the applicable embedded ARM toolchain for windows.

https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-a/downloads

Extract the toolchain, and find the applicable GDB .exe and run it targeting your remotely built executable. To avoid cross compiling, the executable is mapped via a network location.

```console
arm-none-linux-gnueabihf-gdb.exe \\192.168.0.141\pi\hello\hello
```

> Note, above you must replace the IP and location of the executable as applicable 

**Windows machine output:**

```console
GNU gdb (GNU Toolchain for the A-profile Architecture 9.2-2019.12 (arm-9.10)) 8.3.0.20190709-git
Copyright (C) 2019 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "--host=i686-w64-mingw32 --target=arm-none-linux-gnueabihf".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<https://bugs.linaro.org/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from \\192.168.0.141\pi\hello\hello...
(gdb)
```

Now we need to feed the GDB client some information such as the target, 

```console
target remote 192.168.0.141:2001
```

Add the directory location from the PI 

```console
directory \\\\192.168.0.141\\pi\\hello
```

Now time to set a break point in main and continue to it 

```console
b main
c
```

With any luck one should see the following: 

```console
(gdb) b main
Breakpoint 1 at 0x10444: file hello.c, line 4.
(gdb) c
Continuing.

Breakpoint 1, main () at hello.c:4
4          printf("Hello, World!\r\n");
(gdb)
```

Use command `next` to step the through the rest of your program. Consult `help` in the need of ... 

