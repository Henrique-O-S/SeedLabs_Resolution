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
-


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
- As done in the lab we'll use the `admin';--` as the username inside the website.
- `admin` is the account name, `'` is used to end the input of that sql field, `;` ends the line in php, `-- ` will comment all sql code afterwards.
- ![CTF8.1](/Images/Week8/CTF8.1.png "CTF8.1")
- And so we have the flag.

