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

- ![Task2](/Images/Week5/Task2.PNG "Task2")

# Task 3

In buffer overflow vulnerabilities, its very important to to know the difference between the buffer's starting position and the place where the return address is stored.

Firstly, we created a file named badfile, by executing the following command
- `touch badfile`

After that, while in the sub directory 'code', we launched gbd, targeting 'stack-L1-dbg'

- ![Task3](/Images/Week5/first.png "first step")

Then, by running the following command, we created a breakpoint in the `bof` function
- `b bof`


In order to print that function's epb, we need to run the command `next`,after `run` , otherwise we would be refering to the caller's epb

- ![Task3](/Images/Week5/b bof run.png "break point and run")


After that we run the following command to get the epb value:
- `p $epb`

Then, the following command to get the buffer's address:
- `p &buffer`

Finally, we can quit, using the `quit` command

- ![Task3](/Images/Week5/p adress "getting addresses")

After understanding how this works, we need to fill a pyhton script in order to make the attack:

- ![Task3](/Images/Week5/py "python file")


The filled information:
 - `Start` is equal to 490, since our shell code has a size of 27 bytes, and the buffer being attacked has a size of 517 bytes. We decided to put the shell code in the end of the buffer.

 - `ret` is equal to 0xFFFCBA6. This is the return where we want to change our shell code. We want to place our shell code in the address of the buffer(0xFFFC9BC) + 490, which is equal to the value of ret.

 - `offset` indicates the position where the return address that points to our shellcode will be placed. If we subtract the ebp address to the buffer address, that is equal to 0x6C, which is 108. Now we need to add 4 to that result, since the ebp has 4 a size of 4 bytes. So, `offset` is equal to 112.


 - After this we just need to run the exploit, by running the .py file, followed by the stack-L1 program. That enables us to have root access.


- ![Task3](/Images/Week5/final "run the exploit")



 ## CTF 1

 - We saw that the buffer had a size of 20 bytes, but the program was reading 28. So, we just sended 20 bytes of random characters, followed by the file we wanted to print, "flag.txt"

- ![Task3](/Images/Week5/ctf "main.c")


- ![Task3](/Images/Week5/bb "ctf1")





  ## CTF 2

 - This one is simillar to the last one. The only thing that changes is that `val` needs to be equal to 0xFEFC2223
 - We figured the last 8 characters were reserved to the file name, and the 8 characters before that represented the `val`.


- ![Task3](/Images/Week5/ctf7 "main.c")

- ![Task3](/Images/Week5/cf8 "ctf2")







