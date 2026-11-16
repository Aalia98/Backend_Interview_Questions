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

With two messages (SYN, SYN-ACK) the server never learns that the client received its reply, so the server cannot be sure the client is reachable and has the right sequence number. The third message (ACK) confirms both directions work and that both sides agree on sequence numbers. It also helps avoid old duplicate connection requests creating half-open connections.

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

HTTP: not secure, data in plain text, port 80.

HTTPS: HTTP over TLS, so data is encrypted, cannot be changed on the way, and the server identity is checked using a certificate. Default port 443.

SSL is the old name/older versions; today we use TLS (SSL is deprecated).

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

An operation is idempotent if doing it once or many times gives the same final state. GET, PUT, DELETE are idempotent (delete the same user twice: still deleted). POST is generally not (two POSTs can create two orders). It matters for retries: if a request times out, it is safe to retry an idempotent one. For payments, use an idempotency key.

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

A reverse proxy sits in front of servers and handles client requests for them: SSL termination, caching, compression, routing, hiding the backend. A load balancer chooses which server gets each request among many servers, to scale and stay available. Nginx can do both. A load balancer is basically a reverse proxy with a "spread the traffic" job.

# Q. Why not directly query database every time?

- Databases are expensive resources.
- Frequent reads increase latency and cost.

# Q. Why use Redis before Database?

Redis keeps frequently read data in memory, so it is much faster than going to the database on disk (typically about a millisecond or less versus several milliseconds or more, depending on the query). It lowers the load on the database and speeds up responses. Use it for data that is read often and changes rarely (profiles, product lists, sessions).

# Q. What is Cache Hit?

- Requested data exists in cache.
- No database query is required.

# Q. What is Cache Miss?

- Data is absent in cache.
- Application fetches from database and updates cache.

# Q. What is Connection Pooling?

Instead of opening a new database connection for every request (slow: TCP + auth), the application keeps a set of open connections and reuses them.

Benefits: lower latency, better throughput, less load on the database, a limit on the number of connections. In Go, `sql.DB` is already a connection pool (`SetMaxOpenConns`, `SetMaxIdleConns`).

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

A small piece of data the server asks the browser to store (`Set-Cookie` response header). The browser sends it back automatically on every request to that site (`Cookie` header). Used for sessions, login state and preferences. Important flags: `HttpOnly` (JavaScript cannot read it), `Secure` (HTTPS only), `SameSite` (limits cross-site sending, helps against CSRF), `Max-Age`/`Expires`.

# Q. What is CORS?

CORS (Cross-Origin Resource Sharing) is a browser rule. A page from one origin (scheme + domain + port) cannot read responses from another origin unless that server allows it with headers like `Access-Control-Allow-Origin`. It is enforced by the browser, not the server; Postman and curl are not affected.

# Q. What is preflight request?

For some cross-origin requests (methods like PUT/DELETE, or custom headers like Authorization, or JSON content type), the browser first sends an `OPTIONS` request to ask the server if the real request is allowed. The server answers with `Access-Control-Allow-Methods` and `Access-Control-Allow-Headers`. If allowed, the browser sends the real request.

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

The server did not allow your frontend origin (missing or wrong `Access-Control-Allow-Origin`), or it did not handle the `OPTIONS` preflight request. Fix on the backend: add CORS middleware with the exact allowed origins, methods and headers. Do not use `*` together with cookies/credentials.

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

HTTPS encrypts everything between browser and server: the password, but also cookies, tokens and form data. It stops attackers on the same Wi-Fi or network from reading or changing it (man-in-the-middle). Passwords are also hashed on the server, but HTTPS protects them while travelling.

# Q. What if server crashes mid-request?

The client gets a connection error, a timeout or a 5xx (such as 502 from a proxy). The request may or may not have been processed on the server. So clients use timeouts and retries only for idempotent requests (or an idempotency key), and the server should use database transactions so half-done work is rolled back.

# Q. What is Hashing?

Hashing converts input data into a fixed-length output (a hash) using a hash function.
```
mypassword123 -> hash function -> XJ82KDJ29SKDJ82...
```
Properties: same input always gives the same output; fast to compute; one-way (you cannot get the original back from the hash); a tiny change in input changes the whole hash.

# Q. Hashing vs Encryption

| Hashing                          | Encryption                 |
| -------------------------------- | -------------------------- |
| One-way                          | Two-way                    |
| Cannot recover original password | Can recover original data  |
| Used for passwords               | Used for data transmission |

# Q. Salt

Salt is random data added to a password before hashing, unique per user. Two users with the same password then get different hashes, and attackers cannot use ready-made tables (rainbow tables). The salt is stored next to the hash (it is not secret).

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

###### ------------------------------------- ######

# Q. What is an IP address? IPv4 vs IPv6? Public vs private IP?

An IP address identifies a device on a network. IPv4 is 32-bit (`192.168.1.10`, about 4 billion addresses, running out); IPv6 is 128-bit (`2001:db8::1`, huge space). Private IPs (`10.x.x.x`, `172.16-31.x.x`, `192.168.x.x`) work only inside a local network (home, office, VPC). Public IPs are reachable on the internet. A router uses NAT to let many private devices share one public IP.

# Q. What is localhost / 127.0.0.1?

The loopback address: your own machine. A server listening on `127.0.0.1` accepts connections only from the same machine; `0.0.0.0` means all network interfaces (needed in Docker to reach the app from outside).

# Q. Common DNS record types: A, AAAA, CNAME, MX, NS, TXT

A: domain to IPv4. AAAA: domain to IPv6. CNAME: alias to another domain name. MX: mail servers. NS: name servers of the domain. TXT: text (verification, SPF). TTL says how long a record may be cached.

# Q. What is a port? What is a socket? Well-known ports

An IP address finds the machine; a port (0 to 65535) finds the application on it. A socket is one endpoint of a connection: IP + port (a connection is identified by client IP:port + server IP:port). Common ports: HTTP 80, HTTPS 443, SSH 22, MySQL 3306, PostgreSQL 5432, Redis 6379, DNS 53.

# Q. The network layers in simple words (TCP/IP model)

Application (HTTP, DNS) -> Transport (TCP, UDP) -> Internet (IP) -> Link (Ethernet, Wi-Fi). The OSI model has 7 layers (physical, data link, network, transport, session, presentation, application). Interview tip: say "HTTP is at the application layer, TCP at transport, IP at network".

# Q. What is a TCP connection teardown state like TIME_WAIT? (basic)

After closing, the side that closed first waits in `TIME_WAIT` for a short time so late packets do not confuse a new connection. Many TIME_WAIT sockets on a busy server usually mean connections are not being reused (use keep-alive and connection pooling).

# Q. What is flow control / congestion control in TCP? (one line)

Flow control stops the sender from overwhelming the receiver (window size). Congestion control slows the sender when the network is busy. Both are part of why TCP is reliable.

# Q. What is TLS and what does it give us? Simple TLS handshake

TLS gives encryption (privacy), integrity (data not changed) and authentication (the server is who it says, via a certificate signed by a trusted CA). Handshake in short: client hello (supported versions/ciphers) -> server hello + certificate -> client checks the certificate -> both agree on a shared secret key (key exchange) -> encrypted communication with fast symmetric encryption starts.

# Q. HTTP/1.1 vs HTTP/2 vs HTTP/3

HTTP/1.1: text, one request at a time per connection (browsers open several connections), keep-alive. HTTP/2: binary, many requests multiplexed on one connection, header compression. HTTP/3: runs on QUIC (over UDP) instead of TCP, faster connection setup and no TCP-level blocking when packets are lost.

# Q. What is Keep-Alive?

Keeping the TCP (and TLS) connection open to send several requests, instead of opening a new connection for each (saves handshake time). It is default in HTTP/1.1 (`Connection: keep-alive`). Go's `http.Client` reuses connections, so read and close the response body to allow reuse.

# Q. What does a full HTTP exchange look like? (curl example)

```bash
curl -i -X POST http://localhost:8080/users \
  -H "Content-Type: application/json" \
  -d '{"name":"Aalia"}'
```
Request: method, path, headers, body. Response: status line (`HTTP/1.1 201 Created`), headers (`Content-Type: application/json`), body.

# Q. Authentication vs authorization

Authentication: who are you (login). Authorization: what are you allowed to do (permissions, roles). 401 relates to authentication, 403 to authorization.

# Q. Sessions vs JWT: how do they work and when to use which?

Session: after login the server stores session data (in memory/Redis/DB) and gives the browser a session ID in a cookie; the server looks it up on each request (stateful, easy to revoke). JWT: the server signs a token containing user info; the client sends it in `Authorization: Bearer <token>`; the server only verifies the signature (stateless, scales easily, but hard to revoke before expiry). Typical: short-lived access token + refresh token.

# Q. What is a JWT? Structure and common mistakes

Three Base64 parts separated by dots: header (algorithm), payload (claims such as user id, `exp` expiry), signature. The payload is only encoded, not encrypted, so never put secrets in it. Always verify the signature and expiry, use a strong secret/keys, keep tokens short-lived, send over HTTPS.

# Q. Where should the frontend store a token: cookie or localStorage?

`localStorage` is readable by JavaScript, so an XSS attack can steal the token. An `HttpOnly` + `Secure` + `SameSite` cookie cannot be read by JavaScript (safer against XSS), but then you must think about CSRF. Mention this trade-off in the interview.

# Q. What are XSS, CSRF and SQL injection? (one line each)

XSS: attacker injects script into a page that runs in other users' browsers (escape output). CSRF: another site makes the user's browser send an authenticated request (SameSite cookies, CSRF tokens). SQL injection: user input changes the SQL query (use parameterised queries / placeholders).

# Q. What is basic auth vs bearer token vs API key?

Basic: base64 `user:password` in the header (only over HTTPS). Bearer: a token (like JWT) in `Authorization: Bearer ...`. API key: a secret string identifying the calling app, sent in a header.

# Q. HTTP methods: safe and idempotent table

| Method | Safe (no change) | Idempotent |
|---|---|---|
| GET | Yes | Yes |
| HEAD | Yes | Yes |
| OPTIONS | Yes | Yes |
| PUT | No | Yes |
| DELETE | No | Yes |
| POST | No | No |
| PATCH | No | Not guaranteed |

# Q. PUT vs POST (create vs replace)

POST creates a new resource where the server chooses the ID (`POST /users`). PUT replaces a resource at a known URL (`PUT /users/5`) and repeating it gives the same result.

# Q. More status codes to remember: 304, 409, 422, 429, 502, 503, 504

304 Not Modified (use your cached copy). 409 Conflict (duplicate email, version conflict). 422 Unprocessable Entity (valid JSON but failed validation). 429 Too Many Requests (rate limit). 502 Bad Gateway (proxy got a bad answer from the backend). 503 Service Unavailable (overloaded or down). 504 Gateway Timeout (backend too slow). Groups: 2xx success, 3xx redirect, 4xx client error, 5xx server error.

# Q. 400 vs 422 vs 404: which code for which case?

400 malformed request (bad JSON, missing required param). 422 well-formed but invalid data (age = -5). 404 resource or route not found.

# Q. How do you choose status codes for a REST API? (example)

Create user: 201. Get user: 200 or 404. Update: 200 (or 204). Delete: 204. Not logged in: 401. Not allowed: 403. Validation failure: 400/422. Duplicate: 409. Server bug/DB down: 500. Return a consistent JSON error body like `{"error":"email already exists"}`.

# Q. What are the REST principles and good REST URL design?

Resources as nouns, HTTP methods as actions, stateless, JSON, proper status codes. Good: `GET /users`, `GET /users/10`, `POST /users`, `PUT /users/10`, `DELETE /users/10`, `GET /users/10/orders`. Bad: `/getUser`, `/deleteUser?id=10`. Add pagination (`?page=2&limit=20`), filtering and versioning (`/v1/users`).

# Q. What is gRPC? gRPC vs REST

gRPC is a framework for service-to-service calls using HTTP/2 and Protocol Buffers (binary, defined in a `.proto` file, generated client/server code). Faster and strongly typed, supports streaming. REST/JSON is simpler, human readable and works in browsers. Use gRPC for internal microservices, REST for public APIs.

# Q. What is WebSocket? WebSocket vs HTTP

WebSocket keeps one persistent two-way connection (started by an HTTP upgrade), so the server can push messages to the client any time (chat, live scores, notifications). Normal HTTP is request-response: the client must ask each time.

# Q. What is API versioning and pagination?

Versioning: keep old clients working when the API changes (`/v1/`, `/v2/`). Pagination: return data in pages (`limit` + `offset`, or cursor based) instead of the whole table.

# Q. What are timeouts, retries and backoff?

Always set timeouts on HTTP and DB calls. Retry only idempotent requests, with exponential backoff (wait 1s, 2s, 4s...) and a maximum count, so you do not overload a struggling service. In Go: `http.Client{Timeout: ...}` and `context.WithTimeout`.

# Q. Forward proxy vs reverse proxy

A forward proxy sits in front of clients (hides the client, e.g. company proxy/VPN). A reverse proxy sits in front of servers (hides the servers, e.g. Nginx).

# Q. What is an API gateway?

A single entry point for microservices: routing, authentication, rate limiting, logging, request transformation. Examples: Kong, AWS API Gateway, Nginx.

# Q. What is rate limiting? Common algorithms

Limiting how many requests a client can make in a time window, to protect the service from abuse. Algorithms: fixed window, sliding window, token bucket, leaky bucket. Response: 429 with `Retry-After`. Often implemented with Redis counters.

# Q. HTTP caching: Cache-Control, ETag, 304

`Cache-Control: max-age=3600` lets the browser/CDN reuse a response for an hour. `ETag` is a version tag of the response; the client sends `If-None-Match`, and the server replies `304 Not Modified` with no body if unchanged. `no-store` means never cache.

# Q. Caching strategies: cache-aside, TTL, invalidation

Cache-aside: app checks Redis; on a miss it reads the DB and stores the result in Redis. TTL (expiry) removes stale data automatically. Invalidation: delete or update the cache entry when the data changes ("there are only two hard things: cache invalidation and naming things").

# Q. Load balancer: Layer 4 vs Layer 7 and sticky sessions

L4 balances by IP/port (TCP), fast and simple. L7 understands HTTP, so it can route by URL or header. Sticky sessions send a user to the same server (needed if session data is in server memory); better to store sessions in Redis so any server works.

# Q. Health checks, scaling and availability terms

Health check: the load balancer calls `/health` on each server and stops sending traffic to failing ones. Availability: system keeps working (redundancy, no single point of failure). Latency: time for a response. Throughput: requests per second.

# Q. How do you store and verify a password in Go?

Use bcrypt: `hash, _ := bcrypt.GenerateFromPassword([]byte(pw), bcrypt.DefaultCost)` and store the hash; on login `bcrypt.CompareHashAndPassword(hash, []byte(pw))`. Never log passwords and never return them in JSON.

# Q. Symmetric vs asymmetric encryption

Symmetric: one shared key for encrypt and decrypt (AES), fast, used for the actual data. Asymmetric: public key encrypts/verifies, private key decrypts/signs (RSA, ECC), slower, used for the key exchange and certificates. TLS uses both.