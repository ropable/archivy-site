---
date: 08-16-21
id: 48
path: ''
tags: []
title: TLS/SSL certificates
type: note
---

**Transport Layer Security** (TLS) is a protocol that establishes an encrypted session between two computers on the Internet.

# Asymmetric encryption

To securely communicate with a remote server, we need to encrypt the connection (symmetric encryption). However, to do this we need to first securely transmit a security key to the remote server. We do this, we use **asymmetric encryption**. Asymmetric encryption involves two parts:
- **Public key** - anyone can have this, and use it to encrypt a message to you.
- **Private key** - only you have a copy of this, and only it can be used to decrypt a message encrypted using the public key.

The basic idea of establishing a secure symmetric encryption channel is as follows (communication between the browser and a bank website):

1. The browser sends a request to the bank website in the clear.
2. The bank sends back a copy of its public key.
3. The browser encrypts a symmetric key using the bank's public key and sends it to the bank.
4. The bank decrypts the symmetric key using its private key, and uses the symmetric key to establish a secure communication channel with your browser.

Summary:

![](/images/assymetric_comms.jpg)

# TLS certificates
TLS certificates are a type of digital certificate, issued by a Certificate Authority (CA). Digital certificates, also known as identity certificates or public key certificates, are digital files that are used to certify the ownership of a public key. The CA signs the certificate, certifying that they have verified that it belongs to the owners of the domain name which is the subject of the certificate. TLS certificates usually contain the following information:
- The subject domain name
- The subject organization
- The name of the issuing CA
- Additional or alternative subject domain names, including subdomains, if any
- Issue date
- Expiry date
- The public key
- The digital signature by the CA

# Notes
On the server, the public key (certificate) is usually named `*.crt` or `*.pem`. E.g. server.crt, server.pem, client.crt, client.pem. The private key is usually named `*.key` or `*-key.pem`. E.g. server.key, server-key.pem, client.key, client-key.pem.

![](/images/certificates.jpg)