# Challenge
Zip file containing an RSA key and flag.enc

# Method of solve
First ran file private.pem and file flag .enc and got the following
**private.pem: PEM RSA private key**
**flag.enc: OpenPGP Secret Key**
after this we run wc -c flag.enc to check the cipher text size
**128**
then we run
**openssl rsa -in private.pem -text -noout** 
We get the bit value of 1024.
From here we run 
```
openssl pkeyutl -decrypt -inkey private.pem -in flag.enc -pkeyopt rsa_padding_mode:pkcs1
```
and get the output of 
**FLAG-vOAM5ZcReMNzJqOfxLauakHx**
