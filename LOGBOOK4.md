# Trabalho realizado na Semana #4
## Task 1
After opening a shell we type the command "printenv" and it shows all the environment variables:
- ![Task1-1](/Images/Task1-1.png "Task1-1")

Then we try a different command "export":
- ![Task1-2](/Images/Task1-2.png "Task1-2")

## Task 2
nao ha diferenca pois o processo filho herdou as variaveis do pai

## Task 3
We start by executing what is descripted in the guide:
- Compile the file that includes the given program and execute it
![Task3](/Images/Task3-1.png "Task3-1")

Then we change one line on the myenv.c file as showned:
- ![Task3-2-1](/Images/Task3-2-1.png "Task3-2-1")
- ![Task3-2-2](/Images/Task3-2-2.png "Task3-2-2")

Lastly recompile this file and execute it:
- ![Task3-2](/Images/Task3-2.png "Task3-2")

## Task 6
We start by executing what is descripted in the guide:
- Compile the file that includes the given program
- Change it's ownership to root using "sudo chown root <filepath/filename>"
- Become root user using "sudo -i"
- Make the program a SET-UID program by using "chmod u+s <filepath/filename>"
- Logout to a common user using "logout"

Now we can run malicious code by creating a new ls command and changing the PATH variables to lead to it instead of the original /bin/ls:
- Create a new .sh file with a function in it:
Per example,
"#!/bin/bash

function ls() {
  echo 'You have been hacked!'
}"
- Open ~/.bashrc using any text editor
- Add the following command "source <filepath/filename>"
