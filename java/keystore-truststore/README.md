### Difference between TrustStore and KeyStore
TrustStore is used by TrustManager and keyStore is used by KeyManager class in Java.
KeyManager and TrustManager performs different job in Java.
TrustManager determines whether remote connection should be trusted or not and KeyManager decides which authentication credentials should be sent to the remote host for authentication during SSL handshake.   

If you are an SSL Server you will use private key during key exchange algorithm and send certificates corresponding to your public keys to client, this certificate is acquired from keyStore. On SSL client side, if its written in Java, it will use certificates stored in trustStore to verify identity of Server.  

>* **KeyStore** contains private keys and required only if you are running a Server in SSL connection or you have enabled client authentication on server side.  
>* **TrustStore** stores public key or certificates from CA (Certificate Authorities) which is used to trust remote party or SSL connection.

### Reference
https://blog.csdn.net/fw0124/article/details/41013333
https://communities.ca.com/docs/DOC-231162731
http://www.cnblogs.com/devinzhang/archive/2012/02/28/2371631.html
http://yqbjtu.blog.163.com/blog/static/52942620134121109547/
