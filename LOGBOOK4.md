# Trabalho realizado na Semana #4
## Task 1
After opening a shell we type the command `printenv` and it shows all the environment variables:
- ![Task1-1](/Images/Task1-1.png "Task1-1")

Then we try a different command `export`:
- ![Task1-2](/Images/Task1-2.png "Task1-2")

## Task 2
We started by compiling `myprintenv.c` with 
- `$gcc myprintenv.c` which gerenated a binary called `a.out`.

Then we runned it and saved the output into a file using:
- `a.out > file`

After commenting out the `printenv()` statement in the child process case, and uncomment the `printenv()` statement in the parent process case we redid the first steps.
- `$gcc myprintenv.c`
- `a.out > file2`

This time we called the file `file2` so we could use the command 
- `diff file file2`
- ![Task2-3](/Images/Task2-3.png "Task2-3")

Which showed us that there are not any differences between the father and child variables because the child process inherited all his father's variables.

## Task 3
We start by executing what is descripted in the guide:
- Compile the file that includes the given program and execute it
- ![Task3](/Images/Task3-1.png "Task3-1")

Then we change one line on the`myenv.c` file as showned:
- ![Task3-2-1](/Images/Task3-2-1.png "Task3-2-1")
- ![Task3-2-2](/Images/Task3-2-2.png "Task3-2-2")

Lastly recompile this file and execute it:
- ![Task3-2](/Images/Task3-2.png "Task3-2")

## Task4 
After creating the file with the given code, we compiled and executed it as showned:
- `gcc task4.c -o task4`
- `./task4`
- ![Task4](/Images/Task4.png "Task4")
## Task 6
We start by executing what is descripted in the guide:
- Compile the file that includes the given program
- Change it's ownership to root using `sudo chown root <filepath/filename>`
- Become root user using `sudo -i`
- Make the program a SET-UID program by using `chmod u+s <filepath/filename>`
- Logout to a common user using `logout`

Now we can run malicious code by replacing the ls command and or by redirecting the shell program to execute another malicious command named cmd, per example:
"#!/bin/bash

function ls() {
  echo 'You have been hacked!'
}"

As the program is SET-UID and the program owner is the root, the program will run with root privileges, accounting that /bin/sh links to /bin/zsh instead of /bin/dash, so the `sudo ln -sf /bin/zsh /bin/sh` is needed.

<br>

## CTF
### Resolution

#### Recognition:

- Wordpress version and plugins:

![Recognition](/Images/CTF4/Recognition.PNG "Recognition")

- Possíveis utilizadores e nomes de utilizadores:

![Users](/Images/CTF4/Users.PNG "Users")
