# Q. What is JWT?

- JWT (JSON Web Token) is a signed token used for stateless authentication. It contains claims (such as user ID and role) and a signature that allows the server to verify the token without storing session data.

# Q. Why use JWT?

- Stateless authentication
- Scales well across multiple servers
- Easy to use with APIs and microservices

# Q. Can JWT be modified?

- No—at least not safely.
- Anyone can decode the header and payload because they are Base64URL encoded, not encrypted. However, if someone changes the payload (for example, changes "role": "user" to "role": "admin"), the signature no longer matches. The server verifies the signature using its secret key (or public/private key pair), detects the tampering, and rejects the token with a 401 Unauthorized.

# Q. How does JWT authentication work?

- The user logs in with credentials. The server verifies the credentials and generates a signed JWT. The client sends the JWT with subsequent requests. Authentication middleware verifies the token's signature and expiration, extracts the user's identity, and passes the request to the application.

# Q. Why is JWT called stateless? 

- JWT is called stateless because the server does not store any session information. The token itself contains the required user information, and the server only verifies its signature before processing the request.

# Q. Why use access and refresh tokens?

- Access tokens are short-lived and used for API access. Refresh tokens are longer-lived and used to obtain new access tokens without requiring the user to log in again.

# Q. Why not make access token valid for 30 days?

- If a long-lived access token is stolen, the attacker can potentially use it for a long time. Short-lived access tokens reduce the exposure window.

# Q. If JWT is stateless, why do companies still use Redis with JWT? 

- JWT itself is stateless, but companies often use Redis to store revoked tokens, refresh tokens, or token metadata. This allows immediate logout, token revocation, and better session management without giving up JWT's scalability.

# Q. Why can't we store JWT in Local Storage in some applications?

- Local Storage is accessible through JavaScript. If the application has an XSS vulnerability, an attacker can steal the JWT. Storing sensitive tokens in HttpOnly cookies reduces this risk because JavaScript cannot access those cookies.