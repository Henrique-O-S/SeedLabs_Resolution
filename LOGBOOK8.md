## SETUP
- We need to add the line "10.9.0.5 ww.seed-server.com" inside the file /etc/hosts.<br>
- Afterwards we just need to build the container and run it.
- Now we need to find tge ID of the container with `dockps` and once we know the ID we run `docksh ID` to open a shell inside the container.

### Task 1
- On the containers shell we will start using all databases associated with the user root `mysql -u root -pdees`.
- Now that we're using mysql we'll need to load the sqllab_users database with `use sqllab_users`.
- To show the database we can run `show tables`.
- ![Task1](/Images/Week8/Task1.png "Task1")
- To have Alice's information we run `SELECT * FROM credential WHERE name="ALICE";`
- ![Alice](/Images/Week8/AliceInformation.png "Alice")


### Task 2
#### 2.1
- In a browser we open www.seed-server.com and now we need to access the admin account.
- Inside the "unsafe home.php" file we could find a vulnerability in the way the query was made.
- ![task2-1](/Images/Week8/task2-1.png "task2-1")
- Since we know the account name is "admin" we can use a very common sql injection `admin'-- ";`
- `admin` is the account name, `'` is used to end the input of that sql field, `-- ` will comment all sql code afterwards, `"` will end the string in php and `;` end the line in php.
- ![Task2-1-1](/Images/Week8/Task2-1-1.png "Task2-1-1")
- And the result is :
- ![Task2-1-2](/Images/Week8/Task2-1-2.png "Task2-1-2")

### 2.2

- We encoded the string `admin'-- "` in this website: https://www.urlencoder.org
- That string is equal to `admin%27--%20%22%3B`. 
- After that, we sent the command `curl 'www.seed-server.com/unsafe_home.php?username=admin%27--%20%22%3B&Password=11'` in the terminal.
- By doing that, we got access to the content.
- ![task2-2](/Images/Week8/task2-2.png "task2-2")



#### 2.3
- In the username field we inserted the following command `admin'; DELETE FROM credential WHERE name='Alice' ";#`
- We realized that, eventhough the syntax was correct, the following error was always present:
- ![task2-3-1](/Images/Week8/task2-3-1.png "task2-3-1")

- In kind of injection doesn't work since the API used in the .php file is my `mysqli`. The `querry` command only allows single querries.

- ![task2-3-2](/Images/Week8/task2-3-2.png "task2-3-2")


### Task 3
#### 3.1

- The following code to update the user info in the `unsafe_edit_backend.php` file is vulnerable
- ![task3-1-code](/Images/Week8/task3-1-code.png "task3-1-code")

- We inserted the command `9111', salary=50000 WHERE Name='Alice';-- ";` in the phone number section of the edit profile.
- ![task3-1-init](/Images/Week8/task3-1-init.png "task3-1-init")
- After that, we can verify Alice's salary has changed.
- ![task3-1](/Images/Week8/task3-1.png "task3-1")

### 3.2

- This task was almost equal to the previous one. The only thing that changed was the `Name=Boby`.

- The new command is `9111', salary=50000 WHERE Name='Boby';-- ";`

- After that, we can verify Boby's salary has changed.
- ![task3-2](/Images/Week8/task3-2.png "task3-2")

### 3.3

- The passwords on this server are encrypted via SHA1

- ![task3-3-init](/Images/Week8/task3-3-init.png "task3-3-init")



- We altered Boby's password to `7110eda4d09e062aa5e4a390b0a572ac0d2c0220`, which is the equivalent of `SHA1('1234')`.

- We used this command in the Phone number field: `9111', password='7110eda4d09e062aa5e4a390b0a572ac0d2c0220' WHERE Name='Boby';-- ";`

- ![task3-3-1](/Images/Week8/task3-3-1.png "task3-3-1")

- After that, we just logged out and logged in introducting the username `Boby` and the password `1234`.

- ![task3-3-2](/Images/Week8/task3-3-2.png "task3-3-2")

- Finally, we have access to Boby's profile

- ![task3-3-3](/Images/Week8/task3-3-3.png "task3-3-3")


## CTF 1
- Since we have access to the file index.php it's easier to notice the vulnerability inside the login query and try to control it.
- ![LoginCTF8.1](/Images/Week8/LoginQuery-8-1.png "LoginCTF8.1")
- As showned the login query uses directly the input values from the user which gives us the ability to exploit the sistem.
- As done in the lab we'll use the `admin';--` as the username inside the website.
- `admin` is the account name, `'` is used to end the input of that sql field, `;` ends the line in php, `-- ` will comment all sql code afterwards.
- ![CTF8.1](/Images/Week8/CTF8.1.png "CTF8.1")
- And so we have the flag.

## CFT 2

- Firstly we ran the `checksec` command in order to see the `protections` of the executable file.

- ![ctf-8-2-1](/Images/Week8/ctf-8-2-1.png "ctf-8-2-1")

- `NX` is `disabled`, which allows `buffer overflow attacks`, since there aren't memory addresses where code execution is blocked.
- `Pie` is `enabled`, which means that everytime the executable is ran, memory addresses are `randomized`, which makes attacks targetting specific memory addresses more difficult

Our objective is to `call a shell` on the `remote server` in order to obtain the flag (inside the flag.txt file in the working directory)

After analysing the `main.c` file and its protections, we concluded that it is `vulnerable`

- ![ctf-8-2-2](/Images/Week8/ctf-8-2-2.png "ctf-8-2-2")

- The program gives the buffer's address at `runtime`, which can be used to `override` the `return address` with `shellcode` using a buffer overflow attack

The script used to perform the attack was a given script, that `constructs and injects a payload in a specific address`, with some modifications:

- ![ctf-8-2-3](/Images/Week8/ctf-8-2-3.png "ctf-8-2-3")

- ![ctf-8-2-4](/Images/Week8/ctf-8-2-4.png "ctf-8-2-4")

- The buffers' address was read from the output of the executable
- We received the output until the moment when that address was about to be displayed, and then `read it`, saving the `decimal` value on the variable `buffer` (lines 15-17)

- We opted to put the shellcode in the `end of the payload`, which is `517 bytes long`
- Because the shellcode is `27` bytes long, `start` is equal to `490` (517-27)
- `Ret` is equal to the `address of the buffer + 490`. That corresponds to the address where our shellcode is.
- Finally, `offset` is equal to `108`, counting with the buffer size (100), the ESP size (4) and the EBP size (4) bytes.

With this script we are changing the return address of this function (address = `offset`) to `ret`, which points to the location where our shellcode exists (address = `start + 490`).

After running the script we `launch a shell in the server`, where we are able to get the flag using the `cat flag.txt` command.


- ![ctf-8-2-5](/Images/Week8/ctf-8-2-5.png "ctf-8-2-5")
