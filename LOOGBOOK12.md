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
- ![script1](/Images/Week10/Task1-script.PNG "script1")

Or:
- ![script1](/Images/Week10/Task1-script.PNG "script1")

Finally if we can look into the decoded content of the X509 certificate and the RSA key:
- ![script1](/Images/Week10/Task1-script.PNG "script1")
- ![script1](/Images/Week10/Task1-script.PNG "script1")

#### What part of the certificate indicates this is a CA’s certificate?

- The digital signature in the ca.crt file

#### What part of the certificate indicates this is a self-signed certificate?

- As the subject and the issuer in ca.crt match, it indicates the certificate is a self-signed certificate.

#### In the RSA algorithm, we have a public exponent e, a private exponent d, a modulus n, and two secret numbers p and q, such that n = pq.Please identify the values for these elements in your certificate and key files.

