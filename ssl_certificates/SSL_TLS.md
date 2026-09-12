How SSL/TLS Work?
Client is the one sendign the request
Server is the one receiving the request

1. client sends the GET via HTTP
2. server sends back asking for identification via HTTP
3. we send our crednetials to server , but now its a problem if the hacker can come in the middle and can intercept it

4. so we encrypt the credentials and this is done by using a key 
But now how do we send this encryption and decryption key?

symetric key => same key for encryption and decryption 
naturally we cant send the key via the unsecured chanel cause hacker can hack it.

So we use asymterical key => private and public key both are different  
this is done via ssh-keygen

Encryption (confidentiality): public key encrypts, private key decrypts. This works because the public key is public — anyone can send you something only you can read.

Signing (authenticity): private key signs, public key verifies. Only you can produce the signature, and anyone can check it. 

Request over the internet(HTTPS):

1. user(client) sends request to the server , and this makes a s=> public key and S=>private key and this is done via OpenSSL
2. then the public key is shared to the user
3. user has their symetric key , and then using the public key of the server to encrypt this key 
4. then this is encrypted symetric key is shared to server , and using the private key of the server decrypts the encrypted symetric key and now each on has their own symetric key and a secure chanel is estabilished

But now there is one problem where the hacker in the middle can sniff and exchange in between acting as a middle communicator.

So the user/server needs to identify the endpoint
hence why we use certificates in place of keys ,

so when the server send the certificate the browser will validate the certificate and ensure it is coming from the right place and domain.
so see the certificate details we have it issued by a domain name organization via the public key and so on and then we know its the right place and server and this ensures HTTPS

server creates a certificate request (Certificate Sign Request) and done by a Certificate Authority (CA) digicrt and send a reqeust to them and then they issue a certificate by validating the request via the dns and we own the domain name, and then sends back to server and then the server sends this to client.

Now the client browser will check on the user behalf saying i have this certificate and like goes to the CA and they validate it 


