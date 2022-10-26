## Task 1
In order to desactivate some countermessures we runned the commands:
To remove Address Space Randomization we used:
- `sudo sysctl -w kernel.randomize_va_space=0`

To configure /bin/sh we used:
- `sudo ln -sf /bin/zsh /bin/sh`

Inside the folder shellcode we runned the command `make` to compile the existing file.

When we executed `./a32.out` and `./a64.out` a root shell opened giving us full access to the computer.

## Task 2

Firstly, inside the folder code, we ran the command `touch badfile` to create the required file.

Then we compile the program stack.c using `gcc -DBUF_SIZE=100 -m32 -o stack -z execstack -fno-stack-protector stack.c` and run the commands `sudo chown root stack` and `sudo chmod 4755 stack` to make it a root-owned Set-UID program .

Now we can run the program using `stack.c` and check it is indeed a root-owned Set-UID program using `ls -l stack`.

