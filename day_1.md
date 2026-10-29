# What is REST API? And why do we use it?

- REST API stands for Representational State Transfer Application Programming Interface. It is an architectural style used to design APIs where client and server communicate over HTTP.
- It follows principles like stateless communication, resource-based URLs, and uses standard HTTP methods like GET, POST, PUT, and DELETE.
- We use REST APIs to allow different systems, like frontend and backend, to communicate in a simple, scalable, and platform-independent way.
- For example, a GET request to /users will fetch user data from the server.

# What is the difference between PUT and PATCH?

- PUT is used to update or replace the entire resource, whereas PATCH is used to partially update a resource.
- In PUT, we send the complete updated object, even if only one field changes. In PATCH, we only send the fields that need to be updated.

- For example, if a user object has name and email:

In PUT, we must send both name and email even if only email changes
In PATCH, we can send only the email field

# What is statelessness in REST API? Why is it important?

- Statelessness means that the server does not store any client session information between requests, and each request must contain all the required data.
- This is important because it improves scalability and reliability, as any server can handle any request without depending on previous interactions.
- For example, in REST APIs, we often send a token like JWT with every request instead of maintaining sessions on the server.

# HTTP Methods

- What is GET: Fetch data.
- What is POST: Send data to server.
- What is PUT: Replace entire resource.
- What is PATCH: Update part of resource.
- What is DELETE: Remove resource.
- PUT vs PATCH: PUT replaces whole, PATCH updates part.

# Status Codes

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

- 405 — Method Not Allowed

- 500 — Internal Server Error

When: Server crashed or bug occurred.
Why: Something went wrong internally.
Example:
DB down → API fails → 500

# What is JWT? How does authentication using JWT work?

- JWT stands for JSON Web Token. It is used for authentication and securely transmitting information between client and server.
- It consists of three parts: header, payload, and signature.These are encoded and separated by dots.
- When a user logs in, the server generates a token and sends it to the client. Then the client includes this token in every request, usually in the Authorization header.
- The server verifies the token and grants access if it is valid.
- JWT is useful because it enables stateless authentication and improves scalability.

# Where do you store JWT on client side? And what are the risks?

- JWT is typically stored on the client side in localStorage, sessionStorage, or cookies.
- localStorage is commonly used but is vulnerable to XSS attacks since JavaScript can access it.
- Cookies are more secure if we use HttpOnly and Secure flags, but they can be vulnerable to CSRF attacks (Cross site request forgery (CSRF or XSRF) refers to an attack that makes the end-user perform unwanted actions within a web application that has already granted them authentication.).
- So the choice depends on the security requirements of the application.

# Find the first non-repeating character in a string (easy-medium DSA):

Example:
input: "aabccbd"
output: "d"

- I would use a frequency map to count occurrences of each character in the string.
- Traverse the string and store frequency in a hash map
- Traverse the string again
- Return the first character with frequency = 1

# What happens when you type a URL in the browser?

- When we enter a URL, the browser first performs a DNS lookup to get the IP address. Then it establishes a TCP connection followed by a TLS handshake for secure communication. After that, the browser sends an HTTP request to the server. The server processes it and returns the response, finally renders the HTML, CSS, and JavaScript to display the content on your screen.
                                        OR
- Browser first performs DNS lookup to get Google's IP address. Then it establishes a TCP connection using the three-way handshake. Since Google uses HTTPS, a TLS handshake happens to establish encryption. The browser sends an HTTP request to Google's servers. The request reaches a load balancer, which forwards it to an application server. The application server may fetch data from cache or database and generates the response. The response travels back through TCP and the browser renders the HTML, CSS, and JavaScript to display the page.

# What happens when a client sends an HTTP request to your Go backend?

The client sends an HTTP request to the server. The server routes it to the appropriate handler, and configured middleware can perform tasks such as logging and authentication. The handler validates the request and calls the service layer, which executes the business logic and interacts with the repository if database access is required. Finally, the server returns an appropriate HTTP status code and response, often in JSON format.

# Explain encoding vs decoding and why valid JSON can still fail validation?

JSON decoding converts JSON data, such as an HTTP request body, into a Go value like a struct. JSON encoding converts a Go value into JSON, which we can send in an HTTP response. In Go, we commonly use json.NewDecoder(r.Body).Decode(&req) for decoding and json.NewEncoder(w).Encode(response) for encoding. We must check the returned error in both cases.

valid JSON can still fail validation because successfully decoding JSON does not mean the request is valid. For example, {"name":"","email":""} is valid JSON but fails your required-field validation.

# Why return immediately after 400 Bad Request?

We return immediately after sending a 400 Bad Request response because the request is invalid and should not proceed to further processing. The return prevents the handler from continuing and accidentally sending a success response or performing unnecessary operations.

# Malformed JSON vs valid JSON with missing required fields

Malformed JSON cannot be parsed successfully, so the decoder returns an error. Valid JSON with missing or empty required fields can be decoded into a Go struct, but it may fail business validation. In both cases, we can return 400 Bad Request, but for different reasons: parsing failure versus validation failure.

# Why check r.Method before decoding the body?

We check the HTTP method first to ensure that the endpoint only accepts the intended operation. Since this endpoint creates a user, it accepts POST requests. For unsupported methods, we return 405 Method Not Allowed before processing the request body, avoiding unnecessary work.

# Why is 400 Bad Request incorrect when response encoding fails?

A JSON encoding failure is generally a server-side response error, not a client request error. Therefore, returning 400 Bad Request would be misleading. We should log the error and handle it appropriately. If the response has already been committed, we usually cannot change its status code.

# What would you improve before production?

Before using this handler in production, I would add proper email-format validation, limit the request body size, and move the user-creation logic into a service layer. I would also add unit and integration tests, structured logging, and appropriate error handling. If the endpoint persists users in a database, I would handle database errors and duplicate email addresses as well.