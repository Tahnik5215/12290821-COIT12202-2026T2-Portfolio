# PKI and HTTPS

This folder contains my work and evidence for the PKI and HTTPS section of Assessment 1.
<img width="940" height="587" alt="image" src="https://github.com/user-attachments/assets/281c259d-72cb-412c-b189-0d396f481af0" />
Fig1 - GNS3 CA–Server–Client topology
The topology consists of separate CA, Server and Client Linux hosts connected through an Ethernet switch. The CA issues certificates, the Server provides the HTTPS service, and the Client verifies and accesses the protected website.
<img width="940" height="619" alt="image" src="https://github.com/user-attachments/assets/b48e5c8b-8ccb-4444-9c4b-79e5f8e12763" />
Fig2 - Root CA certificate creation and inspection
OpenSSL was used to generate the Root CA certificate and its private key. The matching subject and issuer values confirm that the certificate is self-signed, establishing it as the primary trust anchor for the public key infrastructure.
<img width="940" height="603" alt="image" src="https://github.com/user-attachments/assets/8e8e2731-149b-4b67-8eb3-116c865b509f" />
Fig3 - Server CSR and SAN extension configuration
The common name on the server certificate signing request is www.12290821.lab. The extension configuration adds the same domain as a Subject Alternative Name, restricts the certificate from acting as a CA and enables it for server authentication.

<img width="940" height="639" alt="image" src="https://github.com/user-attachments/assets/1cecc3fc-07da-452a-b25c-149f3e2282b3" />
Fig4 - Nginx HTTPS configuration
Nginx was set-up to listen for encrypted connections on the TCP port 443.Nginx was configured to listen for encrypted connections on the TCP port 443. The configuration specifies a server certificate chain, associated private key, website document root and index page to offer the HTTPS service.

<img width="940" height="624" alt="image" src="https://github.com/user-attachments/assets/fc5dffb3-bb88-49b2-9b6c-c702e76b92cf" />
Fig5 - Full-chain creation and successful nginx -t
The server certificate and CA chain were merged into one single file called server-fullchain.crt. The output of `nginx -t` is successful, which means that the syntax of the configuration is correct and Nginx could load the needed certificate and private-key files.


<img width="940" height="607" alt="image" src="https://github.com/user-attachments/assets/6e5c06b5-3c1b-4d32-91dd-1101c632c910" />
Fig6 - Client connectivity and domain mapping
The successful ping responses show that the Client was able to communicate with the CA and Server hosts. The /etc/hosts file entry of Client maps www.12290821.lab to 10.10.1.20, making it possible for the Client to connect to the Server with the domain name specified in the Server's certificate.

<img width="940" height="619" alt="image" src="https://github.com/user-attachments/assets/22551be3-9801-4ac2-9b84-6226395278a4" />
Fig7 - Transfer of Root, Intermediate and Server certificates
Successfully moved the Root CA and Intermediate CA and server certificates from the CA host to the Client. These files contain the entire certificate chain needed to verify certificates at the client.

<img width="940" height="611" alt="image" src="https://github.com/user-attachments/assets/62f53c86-d2c4-402f-b9b3-1d79bcf7727b" />
Fig8 - Certificate-chain verification showing OK.
The Root CA was the trusted certificate and the Intermediate CA was used to form the certificate chain. We can see that the server certificate was issued via the correct chain and was able to be validated successfully at the bottom of the output `/tmp/server.crt: OK`.

<img width="940" height="630" alt="image" src="https://github.com/user-attachments/assets/8c27c0df-9044-4dcc-9119-fbff8c9c5931" />
Fig9 - Successful HTTPS request using curl
Using the Root CA certificate for trust validation, the `curl` command used to connect to `https://www.12290821.lab/`. The returned message, Secure HTTPS Server – Student 12290821, states that verification of the certificate, matching of the host and encrypted access to the website was successful.  

<img width="940" height="623" alt="image" src="https://github.com/user-attachments/assets/8e958836-92db-4fb2-aabb-84d6f3398000" />
Fig10 - TLS 1.3 handshake with Verify return code: 0 (ok)
The server certificate's subject is www.12290821.lab and the issuer is the Intermediate CA. The negotiated TLS 1.3 protocol is TLS_AES_256_GCM_SHA384 and certificate validation was successful with a return code of 0 (ok).

<img width="940" height="362" alt="image" src="https://github.com/user-attachments/assets/18fe8b0b-1656-4521-8d8b-44fc60f82465" />
Fig11 - Wireshark capture showing TLS traffic on port 443
The Wireshark capture shows the TCP connection establishment, TLS 1.3 Client Hello and Server Hello messages, and the following application data sent in encrypted form between the Client and the Server. This is an indicator at the network level that the communication to the website was encrypted as opposed to unencrypted HTTP.



