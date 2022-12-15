## Task 1
After having the conatiners up and running we can now start the first task.

Firstly we need to copy the configuration file openssl.conf to our current directory using `cp "/usr/lib/ssl/openssl.cnf" "/home/seed/Documents/seedlabs/seed-labs/category-crypto/Crypto_PKI/Labsetup"`

Then we create the directory demoCA with the subdirectories certs, crl, and newcerts and the files serial.txt an index.txt using the following batch of commands:
`   
    mkdir demoCA
    ls
    cd demoCA
    mkdir certs crl newcerts
    echo 1000 > serial
    gedit index.txt
`

Now coming back to the initial directory we can generate a self-signed certificate for our CA using: 
- ![generate-CA](/Images/Week12/Task1-generate-CA.PNG "generate-CA")

Or:
- ![generate-CA2](/Images/Week12/Task1-generate-CA2.PNG "generate-CA2")

Finally if we can look into the decoded content of the X509 certificate and the RSA key:
- ![X509](/Images/Week12/Task1-X509.PNG "X509")
- ![RSA](/Images/Week12/Task1-RSA.PNG "RSA")

#### What part of the certificate indicates this is a CA’s certificate?

- The digital signature in the ca.crt file

#### What part of the certificate indicates this is a self-signed certificate?

- As the subject and the issuer in ca.crt match, it indicates the certificate is a self-signed certificate.
- ![ca-crt](/Images/Week12/Task1-ca-crt.PNG "ca-crt")

#### In the RSA algorithm, we have a public exponent e, a private exponent d, a modulus n, and two secret numbers p and q, such that n = pq.Please identify the values for these elements in your certificate and key files.

Public exponent - e:
- ![e](/Images/Week12/Task1-e.PNG "e")

Private exponent - d:
- ![d](/Images/Week12/Task1-d.PNG "d")

Modulus - n:
- ![modulus](/Images/Week12/Task1-modulus.PNG "modulus")

p:
- ![p](/Images/Week12/Task1-p.PNG "p")

q:
- ![q](/Images/Week12/Task1-q.PNG "q")

## Task 2

Firstly, we need to generate a CSR for our website: 
- ![generate-CSR](/Images/Week12/Task2-generate-CSR.PNG "generate-CSR")

Now we can look at the decoded content of the CSR and private key files:
- ![req](/Images/Week12/Task2-req.PNG "req")
- ![rsa](/Images/Week12/Task2-rsa.PNG "rsa")

Finally, to add two alternative names to the certificate signing request we add the -addtext option to the `openssl req` command:
- ![alt-names](/Images/Week12/Task2-alt-names.PNG "alt-names")

The results can be seen in the server.csr file:
- ![result](/Images/Week12/Task2-result.PNG "result")

## Task 3

Firstly, we need to make a copy of the openssl file with the name of myCA_openssl.

Then, we need to turn the certificate signing request (server.csr) into an X509 certificate (server.crt), using the CA’s ca.crt and ca.key. For that we need to edit the copied openssl file to enable 'openssl ca' to copy the extension field:
- ![extension](/Images/Week12/Task3-extension.PNG "extension")

Only then we can run the command:
- ![certificate](/Images/Week12/Task3-certificate.PNG "certificate")

After signing the certificate, we can check that the alternative names are not included in the server.crt:
- ![result](/Images/Week12/Task3-result.PNG "result")

## Task 4

Firstly, we need to enable Apache’s ssl module and then enable this site:
- ![config](/Images/Week12/Task4-config.PNG "config")

Then, we need to start the Apache server:
- ![start](/Images/Week12/Task4-start.PNG "start")

Now, if we access the site one warning will be presented:
- ![warning](/Images/Week12/Task4-warning.PNG "warning")

This happens because, while our certificate is being recognized, it is still an untrusted CA.

We can add it to the browser's list in the Security section of Preferences, clicking the button "View Certificates":
- ![preferences](/Images/Week12/Task4-preferences.PNG "preferences")

- ![certificate](/Images/Week12/Task4-certificate.PNG "certificate")

After this, we can successfully access the site:
- ![result](/Images/Week12/Task4-result.PNG "result")

## Task 5

Firstly, we need to create a website to impersonate www.example.com. For this we can change the config file for our already certified website, ww.bank32.com:
- ![config](/Images/Week12/Task5-config.PNG "config")

We also need to add www.example.com to the hosts file to emulate the result of a DNS cache positing attack:
- ![hosts](/Images/Week12/Task5-hosts.PNG "hosts")

Now, we can rebuild the docker container, start the server up again and access it. However we will find a warning because it uses an certificate that is not valid for www.example.com:
- ![warning](/Images/Week12/Task5-warning.PNG "warning")

As predicted the PKI defeated the Man-In-The-Middle attack.

## Task 6

Assuming we have access to a compromised CA, we can repeat what we did in the tasks 2 and 3 and certify our fraudulent website:
- ![setup](/Images/Week12/Task6-setup.PNG "setup")

Then we need to config the file bank32_apache_ssl.conf to match our new website:
- ![config](/Images/Week12/Task6-config.PNG "config")

Now, we can rebuild the docker container, start the server up again and access it. This time we will be able access the website without warnings:
- ![result](/Images/Week12/Task5-result.PNG "result")

We can conclude that this time the Man-In-The-Middle attack was successful, as we have a valid certificate signed by a CA that matches.
