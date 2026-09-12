How SSL/TLS Work?
Client is the one sending the request
Server is the one receiving the request

1. client sends the GET via HTTP
2. server sends back asking for identification via HTTP
3. we send our credentials to the server, but now it's a problem if a hacker can come in the middle and intercept them

4. so we encrypt the credentials and this is done by using a key 
But now how do we send this encryption and decryption key?

symmetric key => same key for encryption and decryption
naturally we can't send the key via the unsecured channel because a hacker can intercept it.

So we use an asymmetric key => private and public keys are different
this is done via ssh-keygen

Encryption (confidentiality): public key encrypts, private key decrypts. This works because the public key is public — anyone can send you something only you can read.

Signing (authenticity): private key signs, public key verifies. Only you can produce the signature, and anyone can check it. 

Request over the internet(HTTPS):

1. user(client) sends request to the server , and this makes a s=> public key and S=>private key and this is done via OpenSSL
2. then the public key is shared to the user
3. user has their symmetric key, and then uses the public key of the server to encrypt this key
4. then this encrypted symmetric key is shared with the server, and the private key of the server decrypts the encrypted symmetric key. Now each one has their own symmetric key and a secure channel is established

But now there is one problem where the hacker in the middle can sniff and exchange in between acting as a middle communicator.

So the user/server needs to identify the endpoint
hence why we use certificates in place of keys ,

so when the server send the certificate the browser will validate the certificate and ensure it is coming from the right place and domain.
so see the certificate details we have it issued by a domain name organization via the public key and so on and then we know its the right place and server and this ensures HTTPS

server creates a certificate request (Certificate Signing Request), which is sent to a Certificate Authority (CA), such as DigiCert. The CA validates the request via DNS and domain ownership, then issues a certificate and sends it back to the server.

Now the client browser will check on the user behalf saying i have this certificate and like goes to the CA and they validate it 


