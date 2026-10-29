## Network Fundamentals
- IP Address
- DNS
- Ports
- Socket
- TCP vs UDP
- Three-way handshake
- Four-way termination
- HTTP/HTTPS
- HTTP methods
- Status codes
- Cookies
- Sessions
- JWT

## Advanced Topics
- TLS handshake
- HTTP/1.1 vs HTTP/2 vs HTTP/3
- Keep-Alive
- Connection Pooling
- Load Balancer
- Reverse Proxy
- CDN
- WebSocket
- gRPC
- REST APIs

# Topics needs to cover in backend :
- Internet basics
- DNS
- IP and Ports
- TCP
- UDP
- HTTP
- HTTPS/TLS
- Cookies
- Sessions
- JWT
- REST API
- gRPC
- Load Balancer
- Reverse Proxy
- Caching
- Redis
- Kafka
- Database indexing
- Transactions
- Connection pooling
- Rate limiting
- Middleware
- Docker
- Goroutines and channels
- Context and cancellation

###### ------------------------------------- ######

# Q. What is DNS?

- DNS (Domain Name System) translates human-readable domain names into IP addresses.

Example:
google.com
↓
142.250.183.110

Humans remember names, while computers communicate using IP addresses.

# Q. Why do we need DNS?

- Without DNS, users would have to remember IP addresses for every website.
Instead of:
142.250.183.110

we can simply type:
google.com

# Q. How does DNS lookup work?

Browser checks browser cache.
Browser checks OS cache.
Browser checks Router cache.
Browser asks DNS resolver.
Resolver asks Root DNS server.
Root server points to TLD server (.com).
TLD server points to authoritative DNS server.
Authoritative server returns IP address.
Browser connects to that IP.

# Q. What is DNS caching?

- DNS results are stored temporarily to avoid repeated lookups.

Example:
google.com → 142.250.xx.xx

- This speeds up future requests.

# Q. Why does DNS usually use UDP?

- DNS responses are generally very small, so UDP is faster because it avoids connection setup overhead.
- For large responses, DNS can use TCP.

###### ------------------------------------- ######

# Q. What is TCP?

- TCP is a connection-oriented transport protocol that provides:

- Reliable delivery
- Ordered delivery
- Error detection
- Retransmission of lost packets

# Q. Why does HTTP use TCP?

- HTTP requires reliable communication.
- If HTML packets are lost or arrive out of order, the webpage would break.
- TCP guarantees:
  - No packet loss
  - Correct ordering
  - Retransmission

# Q. Explain TCP Three-Way Handshake.

Client -> SYN
Server -> SYN ACK
Client -> ACK
- This establishes a reliable connection and synchronizes sequence numbers.

# Q. Why do we need a Three-Way Handshake?

- Both client and server need to confirm they can send and receive data.
- The handshake verifies bidirectional communication.

# Q. Why not Two-Way Handshake?

- With only two messages, the client never confirms receiving the server's acknowledgment.
- This can create half-open connections.

# Q. Why does TCP use Four-Way Termination?

- TCP connections are full-duplex.
- Each side closes its sending channel independently:
    FIN
    ACK
    FIN
    ACK

# Q. TCP vs UDP?

| Feature        | TCP      | UDP          |
| -------------- | -------- | ------------ |
| Reliable       | Yes      | No           |
| Ordered        | Yes      | No           |
| Retransmission | Yes      | No           |
| Connection     | Required | Not required |
| Speed          | Slower   | Faster       |

# Q. When should we use UDP?

- When speed matters more than reliability.
Examples:
    Video streaming
    Gaming
    DNS
    Voice calls

###### ------------------------------------- ######

# Q. What is HTTP?

- HTTP (HyperText Transfer Protocol) is an application layer protocol used for communication between a client and a server.

# Q. Difference between HTTP and HTTPS?

- HTTP	
Not secure	
Plain text	
Port 80	

- HTTPS
Encrypted
SSL (Secure Sockets Layer) / TLS (Transport Layer Security) 
Port 443

# Q. What is a client?

- An application that sends requests (browser, mobile app, Postman).

# Q. What is a server?

- A system that receives requests and sends responses.

# Q. What is a request?

- A message sent by client to server asking for data/action.

# Q. What is a response?

- Server’s reply containing status, headers, and body.

# Q. What does stateless mean?

- Server does not store client session info between requests.

# Q. What is REST(Representational State Transfer)?

- Architectural style using HTTP methods to access resources.

# Q. What is an API(Application Programming Interface)?

- Interface that allows two systems to communicate.

# Q. Difference between TCP and HTTP?

- TCP(Transmission Control Protocol) handles connection, HTTP defines data format and rules.

# Q. HTTP Methods

- What is GET: Fetch data.
- What is POST: Send data to server.
- What is PUT: Replace entire resource.
- What is PATCH: Update part of resource.
- What is DELETE: Remove resource.
- PUT vs PATCH: PUT replaces whole, PATCH updates part.

# Q. Is HTTP Stateful?
- No.
- HTTP is stateless.
- Every request is independent.

# Q. How does a server remember logged-in users?
Using:

- Cookies
- Sessions
- JWT

# Q. What is an HTTP Request?

Example:
    - GET /products HTTP/1.1
    - Host: example.com

Contains:
    - Method
    - Path
    - Headers
    - Body (optional)

# Q. What is an HTTP Response?

Example:
    - HTTP/1.1 200 OK

Contains:
    - Status code
    - Headers
    - Response body

# Q. What is Idempotency?

- An operation is idempotent if multiple identical requests produce the same result.

Examples:
    - GET
    - PUT
    - DELETE

- POST is generally not idempotent.

# Q. What is a Load Balancer?

- A load balancer distributes incoming traffic among multiple servers.

# Q. Why do we need a Load Balancer?

- A single server cannot handle millions of users.
- Load balancer improves:
    - Scalability
    - Availability
    - Fault tolerance

# Q. Common Load Balancing Algorithms?

- Round Robin
- Least Connections
- Least Response Time
- IP Hash

# Q. What happens if one server crashes?

- The load balancer performs health checks and stops routing traffic to unhealthy servers.

# Q. What is a Reverse Proxy?

- A reverse proxy sits between clients and backend servers.
Example:
Client
↓
Nginx
↓
Backend Server

# Q. Why use Reverse Proxy?

- SSL termination
- Compression
- Routing
- Caching
- Rate limiting

# Q. Reverse Proxy vs Load Balancer?

| Reverse Proxy                  | Load Balancer       |
| ------------------------------ | ------------------- |
| Handles client requests        | Distributes traffic |
| Can cache responses            | Chooses server      |
| Often provides SSL termination | Focuses on scaling  |

- Some tools perform both roles.

# Q. Why not directly query database every time?

- Databases are expensive resources.
- Frequent reads increase latency and cost.

# Q. Why use Redis before Database?

- Redis stores frequently accessed data in memory.
- Typical latency:
    - Redis: 1 ms
    - Database: 50-100 ms

# Q. What is Cache Hit?

- Requested data exists in cache.
- No database query is required.

# Q. What is Cache Miss?

- Data is absent in cache.
- Application fetches from database and updates cache.

# Q. What is Connection Pooling?

- Instead of creating a new database connection for every request, applications reuse existing connections.

Benefits:
    - Lower latency
    - Better throughput
    - Reduced database overhead
    - Scaling Questions

# Q. Horizontal vs Vertical Scaling?

| Vertical Scaling | Horizontal Scaling |
| ---------------- | ------------------ |
| Bigger machine   | More machines      |
| Limited growth   | Better scalability |

# Q. What is CDN?

- A CDN stores static content closer to users geographically.
Examples:
    - Images
    - CSS
    - JavaScript

# Q. Why does Google use CDNs?

- To reduce latency (the delay between a user's action and a system's response) for global users and decrease load on origin servers.

# Q. Is GET idempotent?

- Yes — multiple requests give same result.

# Q. Why should GET requests not modify data?

- GET requests should not modify data because GET is intended to be safe and idempotent. Browsers, caches, proxies, and crawlers may automatically retry or prefetch GET requests. If GET modified data, repeated requests could unintentionally create or change resources.

# Q. Can GET have a body?

- Technically yes, but not recommended and often ignored.

# Q. Why POST not idempotent?

- Multiple calls create multiple entries.

# Q. Which is safest?

- GET (no side effects).

# Q. Status Codes

- 200 — OK

When: Request successful and data returned.
Why: Server processed request correctly.
Example:
GET /users → list returned → 200 OK

- 201 — Created

When: New resource successfully created.
Why: Server created something new.
Example:
POST /users → new user created → 201 Created

- 204 — No Content

When: Request successful but no data to return.
Why: Operation succeeded, nothing to send back.
Example:
DELETE /users/5 → deleted → 204 No Content

- 301 — Moved Permanently

When: URL has permanently changed.
Why: Resource moved to new location.
Example:
/old-page → now /new-page → 301

- 302 — Found (Temporary Redirect)

When: Temporary redirect.
Why: Resource temporarily available somewhere else.
Example:
Redirect user to login page temporarily → 302

- 400 — Bad Request

When: Client sends wrong or incomplete data.
Why: Server can’t process invalid input.
Example:
POST /login with missing password → 400

- 401 — Unauthorized

When: User not authenticated.
Why: Token or login missing/invalid.
Example:
Access /profile without login → 401

- 403 — Forbidden

When: User authenticated but not allowed.
Why: No permission for that resource.
Example:
Normal user accessing admin route → 403

- 404 — Not Found

When: Requested resource does not exist.
Why: Wrong URL or deleted resource.
Example:
GET /users/9999 (not exist) → 404

- 500 — Server Error

When: Server crashed or bug occurred.
Why: Something went wrong internally.
Example:
DB down → API fails → 500

# Q. What is a header?

- Metadata about request/response.

# Q. Content-Type?

- Format of data (JSON, HTML, etc).

# Q. Authorization header?

- Contains auth token or credentials.

# Q. Accept header?

- Response format client wants.

# Q. User-Agent?

- Client information.

# Q. What is a cookie?

- Small data stored on client.

# Q. What is CORS?

- Security rule restricting cross-origin access.

# Q. What is preflight request?

- OPTIONS request to check permissions.

# Q. Query vs Path params?

- Query parameters are used to filter, sort, or paginate resources and are appended to the URL as optional key-value pairs after a question mark.
- Query: ?page=2

- Path parameters are used to identify a specific resource and are a mandatory part of the URL structure.
- Path: /users/10

# Q. Headers vs Body?

- Headers = metadata, Body = actual data.

# Q. 401 vs 403?

- 401 = not logged in
- 403 = logged in but no permission

# Q. Why CORS error?

- Server didn’t allow your origin.

# Q. What happens when typing google.com?

- DNS → TCP → TLS → HTTP request → response → render.
- Browser first performs a DNS lookup to find Google's IP address, then establishes a secure HTTPS connection (TLS handshake) to the server, sends an HTTP GET request for the page, and finally receives and renders the HTML, CSS, and JavaScript to display the Google homepage on your screen.

OR

Browser first performs DNS lookup to get Google's IP address. Then it establishes a TCP connection using the three-way handshake. Since Google uses HTTPS, a TLS handshake happens to establish encryption. The browser sends an HTTP request to Google's servers. The request reaches a load balancer, which forwards it to an application server. The application server may fetch data from cache or database and generates the response. The response travels back through TCP and the browser renders the HTML, CSS, and JavaScript to display the page.

Browser
 ↓
DNS
 ↓
IP Address
 ↓
TCP Handshake
 ↓
TLS Handshake
 ↓
HTTPS Request
 ↓
Load Balancer
 ↓
Reverse Proxy
 ↓
Application Server
 ↓
Redis Cache
 ↓
Database
 ↓
Response

# Q. Why HTTPS for login?

- Encrypts passwords.

# Q. What if server crashes mid-request?

- Client gets 5xx or timeout.

# Q. What is Hashing?

- Hashing converts input data into a fixed-length output.

Example:
mypassword123
↓
Hash Function
↓
XJ82KDJ29SKDJ82JDJ...

Properties:

Same input → same output
Fast to compute
Cannot

# Q. Hashing vs Encryption

| Hashing                          | Encryption                 |
| -------------------------------- | -------------------------- |
| One-way                          | Two-way                    |
| Cannot recover original password | Can recover original data  |
| Used for passwords               | Used for data transmission |

# Q. Salt

Salt is random data added before hashing.

# Q. Why bcrypt Is Popular?

bcrypt automatically:

- Generates salt
- Stores salt with hash
- Is intentionally slow
- Protects against brute force attacks

# Q. Why Slow Is Good?

- Normally we want fast algorithms.
- For passwords we want the opposite.
- Suppose attacker steals database:
SHA256
1 billion guesses per second

Attacker quickly tests:
password123
admin123
qwerty123

bcrypt
100 guesses per second
Brute force becomes extremely expensive.

Modern Password Hashing Algorithms

Common choices:
bcrypt
Argon2
scrypt

Avoid:
MD5
SHA1
These are too fast and insecure for passwords.

# Q. Why don't we store passwords in plain text?

- If a database is compromised, attackers would immediately gain access to every user's password. Since users often reuse passwords across multiple services, this can lead to compromise of email, banking, and other accounts. Instead, passwords should be stored using one-way hashing algorithms such as bcrypt or Argon2.

# Q. Why not encrypt passwords?

- Encryption is reversible if the key is compromised. Password verification does not require recovering the original password, so one-way hashing is a safer approach.

# Q. Why bcrypt instead of SHA256?

- SHA256 is designed to be fast, which makes brute force attacks easier. bcrypt is intentionally slow and includes salting, making password cracking significantly more difficult.