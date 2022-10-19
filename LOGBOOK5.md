## Task 1
In order to desactivate some countermessures we runned the commands:
To remove Address Space Randomization we used:
- `sudo sysctl -w kernel.randomize_va_space=0`

To configure /bin/sh we used:
- `sudo ln -sf /bin/zsh /bin/sh`

Inside the folder shellcode we runned the command `make` to compile the existing file.

When we executed `./a32.out` and `./a64.out` a root shell opened giving us full access to the computer.

## Task 2
