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
- ![generate-CA2](/Images/Week10/Task1-generate-CA2.PNG "generate-CA2")

Finally if we can look into the decoded content of the X509 certificate and the RSA key:
- ![X509](/Images/Week12/Task1-X509.PNG "X509")
- ![RSA](/Images/Week12/Task1-RSA.PNG "RSA")

#### What part of the certificate indicates this is a CA’s certificate?

- The digital signature in the ca.crt file

#### What part of the certificate indicates this is a self-signed certificate?

- As the subject and the issuer in ca.crt match, it indicates the certificate is a self-signed certificate.
- ![ca-crt](/Images/Week12/Task1-ca-crt.PNG "ca-crt")

#### In the RSA algorithm, we have a public exponent e, a private exponent d, a modulus n, and two secret numbers p and q, such that n = pq.Please identify the values for these elements in your certificate and key files.

Public expoent - e:
- ![e](/Images/Week12/Task1-e.PNG "e")

Private expoent - d:
- ![d](/Images/Week12/Task1-d.PNG "d")

Modulus - n:
- ![modulus](/Images/Week12/Task1-modulus.PNG "modulus")

p:
- ![p](/Images/Week12/Task1-p.PNG "p")

q:
- ![q](/Images/Week12/Task1-q.PNG "q")
