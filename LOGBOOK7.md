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

Start by creating a string that will return you the 4 first bytes of your input using a python script. In this case 64 `%x` were needed.

- ![String](/Images/Week7/Task2.1-string.PNG "String")

Compile the python script and run the program using the generated file as input.

- ![Run](/Images/Week7/Task2.1-run.PNG "Run")

Check the result on the server side.

- ![Result](/Images/Week7/Task2.1-result.PNG "Result")

### Task 2.B

## Task 3
