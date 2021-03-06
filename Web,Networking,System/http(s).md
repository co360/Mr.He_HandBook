## HTTP, HTTP(S) with SSL/TLS

- [How SSL works](#how-ssl-works)
- [Redirection](#redirection)
- [SSL Cert with Let's encrypt](#Ssl-cert-with-lets-encrypt)
- [HTTP/2](#http/2)
- [CA](#ca)

### How SSL works

`SSL/TLS` connection enforces data encryption during transmission over the network:

1. `Browser` connects to a web server (website) secured with SSL (https). `Browser` requests that the `Server` identifies itself.
2. `Server` sends a copy of its `SSL Certificate`, including the `Server's` public key.
3. `Browser` checks the certificate root against a list of trusted CAs (comes with Browsers) and that the certificate is unexpired, unrevoked and that its common name is valid for the website that it is connecting to. If the `Browser` trusts the certificate, it creates, encrypts and sends back a symmetric session key using the `Server's` public key.
4. `Server` decrypts the symmetric session key using its private key and sends back an acknowledgement encrypted with the session key to start the encrypted session.
5. `Server` and `Browser` now encrypt all transmitted data with the session key.

Please note, for all above to work, `ssl cert` needs to be placed under a particular directory on `Server` side for `ssl` server to locate.

### Redirection

`302` - tells the browsers to not cache new url at all unless response header specifies `Cache-Control` or `Expires` with particular values. It's aka `temporary redirection`.
`301` - tells the browsers to cache new url permanently. It's aka `permanent redirection`.
In case you don't want browser to cache it, modify response headers:

```
Cache-Control: no-store, no-cache, must-revalidate
Expires: Thu, 01 Jan 1970 00:00:00 GMT
```

### Ssl cert with Let's encrypt

- The objective of Let’s Encrypt and the ACME protocol is to make it possible to set up an HTTPS server and have it automatically obtain a browser-trusted certificate, without any human intervention. This is accomplished by running a certificate management agent on the web server.
- There are two steps to this process. First, the agent proves to the CA that the web server controls a domain (DNS challenge). Then, the agent can request, renew, and revoke certificates for that domain.

![ACME_CA](./ACME_let's_encrypt.png)

Certbot is a very popular agent.

![How Https work](./how_https_work.png)

---

### HTTP/2

- Multiplexing
  - With HTTP/1.1, you can only download one resource at time. When your site needs two resources `a.css` and `b.js`, a needs to be downloaded first
    before connection to download b can be established. It is really inefficient since client and server don't do too much.
  - To mitigate this, browsers allow for opening multiple connections (typically 6-8) to download them simultaneously. But there is cost involved - setup/manage multiple connections which impact both client and browser.
  - With HTTP/2, it allows you to send off multiple requests on the **same** connection. The requested resources are fetched in parallel and received
    **in any order**.
  - Note, HTTP/1.1 has a concept of `pipelining` which also allows multiple requests to be sent off at once but they need to be returned **in the order they were requested**. This feature is nowhere near as good as HTTP/2 so it is hardly used.

### CA

- Verify identity of servers clients trying to connect. It's done by verifying the cert servers respond with against CAs installed on clients' browsers.
- Issue cert to servers. Done through asking clients to complete DNS challenge and issuing CA signed cert upon DNS challenge success.
- CA warns clients with a message of `your connection is not private` when either servers present a self-signed cert or no cert to clients. Self-signed cert means servers use their own private key to sign and generate the cert rather than obtaining it from CAs.








