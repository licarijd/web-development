Protocols

Protocols are ways to connect to computers and have a shared agreement of how
to comunicate.


HTTP

- allows you to send files over the internet like a HTML, CSS, or JavaScript file 


FTP 

- allows you to send files over the internet 

- often used when you upload files to something like hostgator or a generic hosting platorm
(or uploading app binaries to Google Play)


HTTPS 

- similar to HTTP, but encrypted 


SSH 

- protocol used over a shell 

- a shell, unlike a browser, allows you to talk to the operating system 

- used over a terminal 

- uses encryption

- SSH Command: ssh { user } @ { host } (this allows you to remotely access another computer)

- once connected, you have access to that machine's command like

- on Ubuntu for example, you can clone git repos, install packages etc.


Techniques used in SSH 


Symmetric Encryption 

- uses one secret key for both the encryption and decryption by both parties

Example:

encryption:     Hello  ->  keyA  ->  EI22@0A

decryption:     EI22@0A  ->  keyA  ->  Hello

Since anyone with the key can decrypt the message, we need a key exchange algorithm.

Secret keys are specific to each SSH session.


Key Exchange Algorithm 

- a secure way to exchange keys without interception 

- uses Asymmetrical Encryption 


Asymmetrical Encryption 

- uses two seperate keys for encryption and decryption 


                public_key1     public_key2

    Computer1                                   Computer2

    private_key1                                private_key2


- a computer's public key is used to encrypt messages 

- the same computer's private key is used to decrypt the message


Example:

- Computer 1 requests Computer 2's public key 

- Computer 2 sends it's public key 

- Computer 1 uses Computer 2's public key to encrypt a message, and sends it back to 
Computer 2


Encryption and SSH 

- before a secure connection is initiated, both parties generate temporary public and 
private keys, and share their respective keys with one another 

- then, we can get the symmetric keys using the Diffie Hellman Key Exchange 

- the Diffie Hellman Key Exchange generates a public key based on data from the computers 

- the Diffie Hellman Key Exchange Algorithm makes it possible for each party to combine 
their own private data with public data from other systems to arrive at an identical 
secret session key 


Diffie Hellman Key Exchange Algorithm 

- exchange public variables, and combine them with some private variables so that both
computers can create the same key (symmetric)

- each symmetric key contains public variables and private data of each computer 


SSH protocol only uses asymmetric encryption (which takes longer) to share a secret key, 
then uses symmetric encryption for further communication.

Once a secure symmetric communication has been established, the server uses the client's 
public key and generates a challenge. It sends to challenge to the client.

If the client can successfully decrypt the message, the SSH session begins.


** More on the Diffie Hellman Key Exchange: https://www.youtube.com/watch?v=NmM9HA2MQGI


Hashing

- another form of cryptography used in secure shell connections

- generates a unique value of a fixed length for each input 

- not intended to be reversible 

- used to verify the authentication of the messages between client and host 

- done using HMX (Hash Based Message Authentication codes)

- using a hash function, each message that is transmitted must use something called Mac 
(a hash generated from the symmetric key), the packet sequence number, and the message 
contents we're sending

- the host uses the symmetric key (which is the same as the client's), the packet sequence number,
and the message, and runs it through it's own hash function

- it then checks if the result is the same as the client's hash 


User Authentication (Passwords) in Summary

SSH to a server:

1) Diffie-Hellman Key Hellman 

2) Arrive at Symmetric Key 

3) Make sure of no interference (server sends challenge to client)

4) Authenticate User 


SSH to Server 

- innefficient to type password every time 

- with SSH, we can use something called RSA which allows us to provide or prove identity:

    ssh-keygen -C "{ email-address }"

    -> generates public/private rsa key pair 

example:

    id_rsa_digitalocean         - private 

    id_rsa_digitalocean.pub     - public

- place public key code in ~/.ssh/authorized_keys on a server 

- use:

    ssh-add ~/.ssh/id_rsa_digitalocean

    to add your identity 

- now you can SSH to the server without the password 


Recommended ssh-keygen command:

    ssh-keygen -t rsa -b 4096 -C "{ email }"


SSH Command Chaining 

Example 

    ssh -tt pc1@10.2.1.12 ssh -tt pc2@10.1.1.1

- connects to 10.2.1.12, and then connects to 10.1.1.1 from there

- this can keep going for multiple computers





