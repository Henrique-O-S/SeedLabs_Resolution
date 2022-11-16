## SETUP

To setup the environment the command `sudo sysctl -w kernel.randomize_va_space=0` must be run first.

- ![SETUP](/Images/Week7/SETUP.PNG "SETUP")

## Task 1
After having the servers up and running we can now interact with them. 
- To test the server first we print a simple message such as:
- `echo hello | nc 10.9.0.5 9090`
- The result is the following:
- ![Hello](/Images/Week7/Task1-hello.png "hello")



Then we run build_string.py `python3 build_string.py`to create a set of big strings so it´s easier for us to attack the server.
<br>
<br>
The result is a binary file (badfile) with 1500 bytes worth of strings that we'll use to attack the server.
<br>
<br>

- To execute the attack we run `cat badfile | nc 10.9.0.5 9090` and the result in the server side will show us that we were able to kill the parent process:
- ![Server_down](/Images/Week7/Task1-server-down.png "Server_down")

- As we can see the server doesn't return "(ˆ_ˆ)(ˆ_ˆ) Returned properly (ˆ_ˆ)(ˆ_ˆ)", therefore we can be sure the server was properly attacked.


## Task 2

### Task 2.A

Start by creating a string that will return the 4 first bytes of your input using a python script. The last format specifier should overwite the return address content so in this case 64 `%x` were needed.

- ![String](/Images/Week7/Task2.1-string.PNG "String")

Compile the python script and run the program using the generated file as input.

- ![Run](/Images/Week7/Task2.1-run.PNG "Run")

Check the result on the server side.

- ![Result](/Images/Week7/Task2.1-result.PNG "Result")

### Task 2.B

Following the previous task, alter the script so that the last four bytes are printed in string format. To do that the format specifier that overwrites the return address content should be `%s`.

- ![String2](/Images/Week7/Task2.2-string2.PNG "String2")

Compile the python script and run the program using the generated file as input.

- ![Run2](/Images/Week7/Task2.2-run2.PNG "Run2")

Check the secret message printed on the server side.

- ![Result2](/Images/Week7/Task2.2-result2.PNG "Result2")

## Task 3

### Task 3.A

Start by creating a string where the first 4 bytes are the target's address and, so that the format specifier that overwrites the return address content is `%n`. 

- ![String3](/Images/Week7/Task3.1-string3.PNG "String3")

Compile the python script and run the program using the generated file as input.

- ![Run3](/Images/Week7/Task3.1-run3.PNG "Run3")

The new content of the target's address printed on the server side should now be 0x000000cc. When converted to decimal it equals 204, which is the number of bytes that preceed the `%n` format specifier.

- ![Result3](/Images/Week7/Task3.1-result3.PNG "Result3")

### Task 3.B

Start by creating a string where the first 4 bytes are the target's address and, so that the format specifier that overwrites the return address content is `%n`. Wanting to change the content to 0x00005000, there should be 20480 bytes before the final format specifier.

- ![String4](/Images/Week7/Task3.2-string4.PNG "String4")

Compile the python script and run the program using the generated file as input.

- ![Run4](/Images/Week7/Task3.2-run4.PNG "Run4")

The new content of the target's address printed on the server side should now be 0x00005000.

- ![Result4](/Images/Week7/Task3.2-result4.PNG "Result4")

## CTFs

### CTF 1

This CTF is about string formats. 

After analysing the `.c` file, whe realized that there is a `printf` without the format specified - that occurs on line 27.

- ![main](/Images/Week7/ctf1-main.png "main")

Using the `checksec program` command, we can verify that there is not PIE, so the binary positions are not randomized.

- ![checksec](/Images/Week7/ctf1-checksec.png "checksec")

The way to solve this consists in these steps:
- Get the `flag` address using `gdb`.
- Use that printf to print the string stored in that memory address.

- ![gdb](/Images/Week7/ctf1-gdb.png "gdb")

The `flag` address is `0x804c060`.

Send the command `\x60\xc0\x04\x08%s` via the python script in order to print the value in that address in string format `%s`.

- ![script](/Images/Week7/ctf1-script.png "script")

After compiling the script the flag is ours.

- ![final](/Images/Week7/ctf1-final.png "final")

### CTF 2

This CTF is also about string formats, but it is more difficult than the previous one.

After analysing the `.c` file, whe realized that there is a `printf` without the format specified - that occurs on line 14.
Also, the `key` buffer needs to hold the value of `0xBEEF`.

- ![main](/Images/Week7/ctf2-main.png "main")

Using the `checksec program` command, we can verify that there is not PIE, so the binary positions are not randomized.

- ![checksec](/Images/Week7/ctf2-checksec.png "checksec")

The way to solve this consists in these steps:
- Get the `key` address using `gdb`.
- Use that printf to inject values in the memory address where the `key` is stored.

- ![gdb](/Images/Week7/ctf2-gdb.png "gdb")

The `flag` address is `0x804c060sdgdf`.

Send the command `????\x34\xC0\x04\x08%.48871x%n` via the python script in order to inject the value `0xBEEF` in that memory address.

- Using the `%.Ax%n` command, we are injecting the written values in the address previously typed.

- 4 `?` were typed in order to navigate through the stack. That allows us to get to the start of the `key` buffer.

