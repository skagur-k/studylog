# JavaScript: Inside the Networking Layer + How to Optimize Its Performance and Security

Date: Dec 18, 2020 1:40 PM
Property: https://blog.sessionstack.com/how-javascript-works-inside-the-networking-layer-how-to-optimize-its-performance-and-security-f71b7414d34c
Tags: Javascript, Notes, Study

The overall performance of the browser is determined by a number of large components:

- parsing, layout, style calculation, JavaScript and WebAssembly execution, rendering
- and the **networking stack**

![JavaScript%20Inside%20the%20Networking%20Layer%20+%20How%20to%20Op%20197ee9c266194871b76e3035f5531126/Untitled.png](JavaScript%20Inside%20the%20Networking%20Layer%20+%20How%20to%20Op%20197ee9c266194871b76e3035f5531126/Untitled.png)

**Process of network interaction**

- User enters a URL in the browser address bar
- Given the URL, the browser checks its local and application caches and tries to use local copy to fulfill the request
- If cache cannot be used, browser takes the domain name from the URL and requests the IP address of the server from a **DNS.** if the domain is cached, no DNS query is needed.
- HTTP packet is created which requests a web page located on a remote server.
- Packet sent to TCP layer which adds its own information on top of the HTTP packet.
- Packet sent to IP layer which figures out a way to send the packet from the user to the server. Information also stored on top of the packet.
- Packet sent
- Gets back response.

![JavaScript%20Inside%20the%20Networking%20Layer%20+%20How%20to%20Op%20197ee9c266194871b76e3035f5531126/Untitled%201.png](JavaScript%20Inside%20the%20Networking%20Layer%20+%20How%20to%20Op%20197ee9c266194871b76e3035f5531126/Untitled%201.png)

### Socket management

- **Origin —** A triple of application protocol, domain name and port number

    e.g. (https, www.example.com, 443)

- **Socket pool** — a group of sockets belonging to the same origin (all major browsers limit this to 6 socvkets)

![JavaScript%20Inside%20the%20Networking%20Layer%20+%20How%20to%20Op%20197ee9c266194871b76e3035f5531126/Untitled%202.png](JavaScript%20Inside%20the%20Networking%20Layer%20+%20How%20to%20Op%20197ee9c266194871b76e3035f5531126/Untitled%202.png)

JavaScript and WebAssembly do not allow managing the lifecycle of individual network sockets. **Browsers handle them all**.

Opening new TCP connection comes at an additional cost.

Reuse of connections introduces great performance benefirts.

By defaault, browsers use the "keepalive" mechanism which saves time from opening new connection the server. 

**Average time for opening new TCP connection**

![JavaScript%20Inside%20the%20Networking%20Layer%20+%20How%20to%20Op%20197ee9c266194871b76e3035f5531126/Untitled%203.png](JavaScript%20Inside%20the%20Networking%20Layer%20+%20How%20to%20Op%20197ee9c266194871b76e3035f5531126/Untitled%203.png)

The requests can be executed in a different order depending on priority.

Browser can optimize the bandwidth allocation accross all sockets or can open sockets in anticipation of a request.

## Network Security and Sandboxing

Browser enables the enforcement of a consistent set of security and policy constraints on untrusted application resources.

Browser formats all outgoing requests to enforce consistent and well-formed protocol semantics to protect the server. 

### TLS negotiation

**Transport Layer Security (TLS):** cryptographic protocol that provides communication security over network.

**TLS handshake**

1. Client sends "**Client hello**" message to the server (w/  client's random value and supported cipher suites.
2. Server responds by sending "**Server hello"** message to the client with server's random value
3. Server sends its certificate for authentication to the client and may request similar cert. from the client.
    - Server sends the "**Server hello done**" message.
    - If server requested certificate, client sends it.
4. The client creates a random Pre-Master Secret and encrypts it with the public key from the server's certificate, sends the encrypted secret to the server.
5. Server receives the secret. The server and the client each generate the Master Secret and session keys based on the Pre-Master Secret.
6. The client sends a "Change cipher spec" notification to the server to note that the client will start using the new session keys for hashing and encryption. 

    → Sends "**Client finished**" message

7. Server receives the "Change cipher spec" and switches its record layer security state to symmetric encryption using the session keys. 

    → Server sends "Server finished" to client

8. Client and Server exchange application data through such secured line. 

### Same-origin policy

Two pages have same origin if the protocol, port and host are the same for both pages.

### Improving web app performance and security

- Always use `"Connection: Keep-Alive"` header.
- Use proper Cache-control, Etag and Last-Modified headers
- Always use TLS

**READING LIST**

[High Performance Networking in Google Chrome](https://www.igvita.com/posa/high-performance-networking-in-google-chrome/)