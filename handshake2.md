# handshake2

The following exercise specifies the negotiation and bootstrapping of a secure channel between two peers. The initiator is called the client, the other peer is called the server.

Consider the following scenario:

1. The client sends a `client_hello` message to the server containing:
    - a subset from the list of cipher suite (**as defined in Table 1**)
    - the name of the host it is trying to connect to (e.g. google.com)
2. The server replies with a `server_hello` message which contains:
    - a single cipher suite chosen from the `client_hello`'s list
3. The server sends a `certificate` message to the client containing its certificate including its long-term public key
4. The server generates an ephemeral RSA key pair according to the cipher suite chosen.
5. The server then sends a `server_key_exchange` message containing:
    - its ephemeral RSA public key
	- **a signature over the ephemeral key**
6. The client verifies the certificate of the server, if it has not been signed by a trusted certificate from the certificate store of the client's OS, the client abort the protocol.
7. The client generates a pre-master secret.
8. The client sends a `client_key_exchange` message containing:
    - the pre-master secret encrypted using the server's ephemeral RSA public key
9. Both peers compute encryption and MAC keys:
    - enc_key = Hash(pre-master secret, "encryption key")
    - mac_key = Hash(pre-master secret, "mac key")

---

**Table 1: List of Ciphersuites**

<table>
    <tr>
        <td>CipherSuite name</td><td>Key Exchange Algorithm</td><td>Authenticated Encryption Algorithm</td><td>Message Authentication Code Algorithm</td><td>Hash algorithm</td>
    </tr>
    <tr>
        <td>RSA_DES_ECB_HMACMD5_MD5</td><td>2048 bit RSA</td><td>DES-64-ECB</td><td>HMAC-MD-5</td><td>MD-5</td>
    </tr>
    <tr>
        <td>RSA_AES128_GCM_HMACSHA256_SHA256</td><td>2048 bit RSA</td><td>AES-128-GCM</td><td>HMAC-SHA-256</td><td>SHA-256</td>
    </tr>
    <tr>
        <td>RSA_AES128_GCM_KMAC_SHA256</td><td>2048 bit RSA</td><td>AES-128-GCM</td><td>KMAC</td><td>SHA-256</td>
    </tr>
    <tr>
        <td>RSA_AES128_GCM_HMACSHA256_SHA256_EXPORT</td><td>2048 bit RSA</td><td>AES-128-GCM_EXPORT</td><td>HMAC-SHA-256</td><td>SHA-256</td>
    </tr>
</table>

---

Describe at least one way to attack this handshake protocol.

> Hint: What is a downgrade attack?

How to prevent such attack?