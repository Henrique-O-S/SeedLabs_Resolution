## Task 1
After having the servers up and running we can now interact with them. 
Firstly we need to login into one of the accounts. In this case we choose Samy.
Then we can edit our profile to add a malicious script:
- ![script1](/Images/Week10/Task1-script.PNG "script1")

Now, as soon as any user enters Samy's profile, the script will be executed:
- ![result1](/Images/Week10/Task1-result.PNG "result1")

## Task 2
For the second task we need to modify the first task's script to the following:
- ![script2](/Images/Week10/Task2-script.PNG "script2")

Now, as soon as any user enters Samy's profile, the script will be executed and the user's cookies will be displayed in the alert window:
- ![result2](/Images/Week10/Task2-result.PNG "result2")

## Task 3
For the third task we need to modify the previous task's script to the following:
- ![script3](/Images/Week10/Task3-script.PNG "script3")

Now, before we visit Samy's profile we need to setup a listener that prints out whatever is sent by the client:
- ![listener3](/Images/Week10/Task3-listener.PNG "listener3")

After this, as soon as any user enters Samy's profile, the script will be executed and the user's cookies will be displayed in the attacker's terminal:
- ![result3](/Images/Week10/Task3-result.PNG "result3")

## CTFs

### CTF1


When we enter the `ctf-fsi.fe.up.pt` site on port `5002`, we are presented this site:

- ![ctf10-1-1](/Images/Week10/ctf10-1-1.png "ctf10-1-1")

It is known that the objective is to avoid that the admin "clicks" the `Mark request as read` button after we enter an input. 

- We can submit a message via an HTML input (in a form)

- After doing so we are redirected to this page, which reloads every 5 seconds with the `"admin's response to our request"`

![ctf10-1-2](/Images/Week10/ctf-10-1-2.png "ctf10-1-2")

- We quickly figured that the `Give the flag` button had to be "magically" `pressed` by the "admin" 

- Going back to the initial page, we verified that the message sent throught the form had no `validation`, so we are able to `inject` a `script` to manipulate the `Give the flag` button

![ctf10-1-3](/Images/Week10/ctf-10-1-3.png "ctf10-1-3")


- After inspecting the HTML code of the page with the buttons, we extracted the `id of the button element` that needed to be clicked

![ctf10-1-4](/Images/Week10/ctf-10-1-4.png "ctf10-1-4")

- Finally we wrote the message containing the `script`. This will make that `"magic press" happen`  

![ctf10-1-5](/Images/Week10/ctf-10-1-5.png "ctf10-1-5")

- And after a few seconds the flag is ours

![ctf10-1-6](/Images/Week10/ctf-10-1-6.png "ctf10-1-6")

### CTF2

When we enter the `ctf-fsi.fe.up.pt` site on port `5000`, we are presented this server running in PHP:

- ![ctf10-2-1](/Images/Week10/ctf-10-2-1.png "ctf10-2-1")

This time we haven't got access to the code, but we realized we could access the `our network status` button without being logged in

- ![ctf10-2-2](/Images/Week10/ctf-10-2-2.png "ctf10-2-2")

- Inside this new page, we have the ability to ping a host
- The first address we tried to ping was `google.com` (by typing that address into the input field and clicking the button bellow), which worked well

- ![ctf10-2-3](/Images/Week10/ctf-10-2-3.png "ctf10-2-3")

- After reading the guide in moodle, we thought that this functionality was probably using the `ping` command in `Linux`

- After that thought, we tested if `more than 1 command` could be ran in that `shell`

- That was done by writing `google.com; ls` in the input field

- ![ctf10-2-4](/Images/Week10/ctf-10-2-4.png "ctf10-2-4")

- After seing that the end of the result matched with the `ls` command sent through the input field, we concluded that `running several commands at once works`

- So we wrote `google.com; cat /flags/flag.txt` in the input field, since we knew the `path` to the flag

- And by doing that we got access to the flag

- ![ctf10-2-5](/Images/Week10/ctf-10-2-5.png "ctf10-2-5")







