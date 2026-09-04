# API Mastery — From Beginner to Pro

> A practical, interview-focused guide to understanding APIs from first principles to production architecture.
>
> **Learning order:** Internet → HTTP → API → REST → JSON → YAML → HTTP methods → status codes → headers → authentication → authorization → REST design → OpenAPI → Postman/cURL → pagination/filtering → errors → security → testing → performance → webhooks → OAuth → JWT → GraphQL → gRPC → async APIs → API gateways → microservices → production design.

---

## 📌 How to Use This Guide

For every concept, learn it using this sequence:

1. **What is it?**
2. **Why do we need it?**
3. **How does it work?**
4. **Why this approach instead of alternatives?**
5. **Where is it used in real projects?**
6. **What can go wrong?**
7. **How would I explain it in an interview?**

Do not memorize definitions first. Build the mental model first.

---

# PART 1 — THE FOUNDATION

## 1. What is an API?

### What?

**API = Application Programming Interface.**

An API is a defined way for one software system to communicate with another software system.

Think of it as a **contract between a client and a service**.

Example:

```text
Mobile App
    |
    | GET /users/123
    v
User API
    |
    v
Database
```

The mobile app does not need to know how the database works. It only needs to know how to request user information from the API.

### Why do we need APIs?

Without APIs, every application would need direct knowledge of another application's internal implementation.

APIs provide:

- Separation of systems
- Reusability
- Security boundaries
- Standard communication
- Integration between applications
- Access to data and functionality

### Real-world example

A food-delivery app may use:

```text
Restaurant API
Payment API
Maps API
Notification API
User API
Order API
```

The application combines these services to provide one user experience.

### Important interview point

**API does NOT automatically mean REST or HTTP.**

An API is a general concept.

REST API, GraphQL API and gRPC API are different styles/protocol approaches for exposing APIs.

---

# 2. API vs UI vs Database

| Component | Purpose |
|---|---|
| UI | Human interacts with the application |
| API | Software communicates with software |
| Database | Stores data |

Example:

```text
User
 ↓
UI
 ↓
API
 ↓
Business Logic
 ↓
Database
```

A user may click:

> "Show my orders"

The UI might call:

```http
GET /orders
```

The API processes the request and retrieves data from the database.

---

# 3. What happens when I call an API?

Suppose:

```http
GET https://api.example.com/users/123
```

The high-level flow is:

```text
Client
  |
  | DNS lookup
  v
Server
  |
  | HTTPS connection
  v
API endpoint
  |
  | Authentication
  v
Application logic
  |
  | Database query
  v
Database
  |
  v
Response
  |
  v
Client
```

The response might be:

```json
{
  "id": 123,
  "name": "Sai",
  "role": "data_engineer"
}
```

---

# PART 2 — INTERNET FUNDAMENTALS

# 4. What is a Client?

A **client** is the system making a request.

Examples:

- Browser
- Mobile application
- Python program
- Java application
- Postman
- cURL
- Another backend service

Example:

```text
Python program → API
```

Python is the client in this interaction.

---

# 5. What is a Server?

A **server** is the system receiving the request and providing a response/service.

Example:

```text
Client
  |
  | Request
  v
Server
  |
  | Response
  v
Client
```

A server might:

- Validate input
- Authenticate the user
- Execute business logic
- Query databases
- Call other APIs
- Return a response

---

# 6. What is DNS?

DNS = **Domain Name System**.

Humans use:

```text
api.example.com
```

Computers ultimately communicate using IP addresses such as:

```text
203.0.113.10
```

DNS translates the domain name into an address.

```text
api.example.com
       |
       v
      DNS
       |
       v
203.0.113.10
```

### Why?

Because remembering domain names is easier than remembering IP addresses.

---

# 7. What is a Port?

A port identifies a network service on a machine.

Common examples:

```text
HTTP   → 80
HTTPS  → 443
SSH    → 22
```

Conceptually:

```text
IP address = machine
Port       = service entrance
```

---

# 8. What is a Protocol?

A protocol is a set of rules for communication.

Examples:

- HTTP
- HTTPS
- TCP
- UDP
- WebSocket
- SMTP
- FTP
- gRPC/HTTP2-based communication

Protocols define how systems communicate.

---

# PART 3 — HTTP

# 9. What is HTTP?

HTTP = **HyperText Transfer Protocol**.

It is a protocol used for communication between clients and servers.

Example:

```text
Client
  |
  | HTTP Request
  v
Server
  |
  | HTTP Response
  v
Client
```

---

# 10. Why is HTTP so widely used for APIs?

HTTP became dominant for web APIs because:

1. The web already runs on HTTP.
2. Browsers support it natively.
3. Firewalls/proxies/load balancers understand it.
4. It has standardized methods and status codes.
5. It works across programming languages.
6. It supports headers, caching and content negotiation.
7. HTTP tooling is mature.
8. HTTPS provides encryption through TLS.

### Interview answer

> HTTP is widely used for APIs because it is a mature, standardized application-layer protocol with broad client/server support, well-defined request/response semantics, headers, status codes, caching and strong infrastructure support.

---

# 11. Why HTTP instead of TCP directly?

TCP provides reliable transport.

But TCP does not define application-level concepts such as:

```text
GET
POST
PUT
DELETE
Content-Type
Authorization
404
500
```

So if we built an API directly on TCP, we would need to design those rules ourselves.

HTTP gives us a standardized application protocol on top of transport.

```text
Application
     |
    HTTP
     |
    TCP
     |
     IP
```

---

# 12. HTTP vs HTTPS

| HTTP | HTTPS |
|---|---|
| Plain HTTP | HTTP over TLS |
| Traffic can be observed/modified on an untrusted network | Traffic is encrypted and authenticated |
| Usually port 80 | Usually port 443 |
| Not appropriate for sensitive production APIs | Standard choice for production APIs |

### Important

HTTPS does not change HTTP's basic API concepts.

It protects the communication channel using TLS.

---

# 13. Why not FTP for APIs?

FTP is primarily designed for file transfer.

APIs generally need:

- Resource-oriented requests
- Authentication
- Structured responses
- Status codes
- Fine-grained operations
- Caching
- Web infrastructure compatibility

FTP can be useful for file exchange, but it is not the normal choice for modern application APIs.

---

# 14. Why not SMTP?

SMTP is designed for email transfer.

It is excellent for:

```text
Application → Mail Server → Email
```

It is not designed as the general request/response protocol for application data APIs.

---

# 15. Why not UDP?

UDP is connectionless and does not provide TCP's reliable ordered delivery.

It can be useful where low latency is more important and the application can tolerate/manage loss.

For normal business APIs, reliability and standardized HTTP semantics are generally more useful.

---

# PART 4 — API TYPES

# 16. Types of APIs

There are several ways to classify APIs.

## By accessibility

### Public API

Available for external developers, usually with authentication and usage rules.

Example:

```text
Weather API
Payment API
Maps API
```

### Private/Internal API

Used inside an organization.

```text
Order Service → Payment Service
```

### Partner API

Exposed specifically to approved business partners.

---

## By architecture/style

Important API approaches include:

```text
REST
SOAP
GraphQL
gRPC
WebSocket
Webhooks
Async APIs / messaging
```

These are not all identical categories. Some describe API architecture, some communication models, and some event-driven interaction patterns.

---

# 17. REST API

REST = **Representational State Transfer**.

REST is an architectural style for designing networked systems.

Typical REST APIs use HTTP and resources.

Example:

```http
GET    /users
GET    /users/123
POST   /users
PUT    /users/123
PATCH  /users/123
DELETE /users/123
```

---

# 18. Why REST became popular

REST works naturally with HTTP.

Advantages:

- Simple
- Human-readable
- Easy to debug
- Browser-friendly
- Huge tooling ecosystem
- Stateless request model
- Good fit for CRUD/resource APIs

---

# 19. SOAP API

SOAP = **Simple Object Access Protocol**.

SOAP is a protocol commonly associated with XML-based enterprise web services.

Example structure:

```xml
<Envelope>
   <Header>
   </Header>
   <Body>
      ...
   </Body>
</Envelope>
```

SOAP can provide strong formal contracts and enterprise features.

---

# 20. REST vs SOAP

| REST | SOAP |
|---|---|
| Architectural style | Protocol |
| Commonly uses HTTP | Can operate over multiple transports |
| Often JSON | XML-focused |
| Lightweight | More formal/verbose |
| Resource-oriented | Operation/service-oriented |
| Very common in web APIs | Common in legacy/enterprise systems |

### Interview trap

Do not say:

> REST is a protocol.

Better:

> REST is an architectural style; HTTP is the protocol commonly used to implement REST APIs.

---

# 21. GraphQL

GraphQL allows clients to request the fields they need.

Example:

```graphql
query {
  user(id: 123) {
    name
    email
  }
}
```

Instead of several REST endpoints, a GraphQL API commonly exposes a schema through which clients construct queries.

### Why?

Useful when clients have different data requirements and need flexible data retrieval.

### Trade-off

More flexibility can also mean more complexity around query cost, authorization, caching and server-side execution.

---

# 22. gRPC

gRPC is a high-performance RPC framework commonly used for service-to-service communication.

It commonly uses:

- Protocol Buffers
- HTTP/2
- Strongly typed contracts

Example conceptual flow:

```text
Service A
   |
   | RPC call
   v
Service B
```

### Why use gRPC?

- High performance
- Strong contracts
- Code generation
- Efficient binary serialization
- Excellent service-to-service communication

---

# 23. REST vs GraphQL vs gRPC

| REST | GraphQL | gRPC |
|---|---|---|
| Resource-oriented | Query-oriented | RPC-oriented |
| Usually JSON | Flexible query | Protobuf commonly |
| Very web-friendly | Flexible clients | Excellent service-to-service |
| Simple tooling | Schema-driven | Strong code generation |
| Easy HTTP debugging | Query complexity | Less browser-native |

---

# PART 5 — HTTP REQUEST

# 24. Anatomy of an HTTP Request

Example:

```http
POST /users HTTP/1.1
Host: api.example.com
Authorization: Bearer TOKEN
Content-Type: application/json

{
  "name": "Sai",
  "email": "sai@example.com"
}
```

It contains:

```text
Method
Path
HTTP Version
Headers
Body
```

---

# 25. HTTP Methods

The major methods:

```text
GET
POST
PUT
PATCH
DELETE
HEAD
OPTIONS
```

---

# 26. GET

Used to retrieve data.

```http
GET /users/123
```

Response:

```json
{
  "id": 123,
  "name": "Sai"
}
```

### GET should generally be safe

It should not intentionally change server state.

---

# 27. POST

Usually used to create a resource or trigger an operation.

```http
POST /users
```

Body:

```json
{
  "name": "Sai"
}
```

Possible response:

```http
201 Created
```

---

# 28. PUT

Typically replaces the representation of a resource.

```http
PUT /users/123
```

```json
{
  "name": "Sai",
  "email": "new@example.com"
}
```

If the API defines PUT as full replacement, omitted fields may be replaced/reset.

---

# 29. PATCH

Used for partial modification.

```http
PATCH /users/123
```

```json
{
  "email": "new@example.com"
}
```

Only the specified part is changed.

---

# 30. DELETE

Used to delete a resource.

```http
DELETE /users/123
```

---

# 31. HEAD

Similar to GET but requests headers without the response body.

Useful for checking metadata such as:

- Existence
- Content length
- Last modification information

---

# 32. OPTIONS

Used to discover communication options supported by a resource/server.

It is also important in browser CORS preflight behavior.

---

# 33. Safe vs Idempotent

This is an important interview topic.

### Safe

A safe method should not intentionally modify server state.

Typically:

```text
GET
HEAD
OPTIONS
```

### Idempotent

An operation is idempotent when repeating the same request has the same intended effect as making it once.

Typically:

```text
GET
PUT
DELETE
HEAD
OPTIONS
```

POST is generally **not idempotent**.

PATCH depends on the API operation and implementation.

---

# 34. Why PUT vs PATCH?

Use:

```text
PUT = replacement/update representation
PATCH = partial modification
```

Example:

Current:

```json
{
  "name": "Sai",
  "email": "a@example.com",
  "city": "Hyderabad"
}
```

PATCH:

```json
{
  "city": "Bengaluru"
}
```

Only city is changed.

---

# PART 6 — URL, URI AND ENDPOINT

# 35. URL

URL = Uniform Resource Locator.

Example:

```text
https://api.example.com/users/123
```

Parts:

```text
https://       scheme
api.example.com host
/users/123     path
```

With query:

```text
https://api.example.com/users?page=2&limit=20
```

---

# 36. Endpoint

An endpoint is a specific API location/interface through which a client can interact with a service.

Example:

```http
GET /users
```

is one endpoint/method combination.

The same path with another method can represent another operation:

```http
GET  /users
POST /users
```

---

# 37. Path Parameter

Identifies a specific resource.

```http
GET /users/123
```

Here:

```text
123 = path parameter
```

---

# 38. Query Parameter

Used to modify/filter the request.

```http
GET /users?page=2&limit=20
```

Here:

```text
page=2
limit=20
```

are query parameters.

---

# 39. Path Parameter vs Query Parameter

| Path | Query |
|---|---|
| Identifies resource | Filters/modifies retrieval |
| `/users/123` | `/users?page=2` |
| Often required | Often optional |

---

# PART 7 — HTTP HEADERS

# 40. What is a Header?

Headers carry metadata about a request or response.

Example:

```http
Content-Type: application/json
Authorization: Bearer TOKEN
Accept: application/json
```

---

# 41. Content-Type

Tells the receiver what format the request body uses.

Example:

```http
Content-Type: application/json
```

means the body is JSON.

---

# 42. Accept

Tells the server what response formats the client can accept.

```http
Accept: application/json
```

---

# 43. Authorization Header

Common example:

```http
Authorization: Bearer <token>
```

It carries credentials/token information used for authentication/authorization.

---

# 44. User-Agent

Identifies the client software.

Example:

```http
User-Agent: Mozilla/5.0
```

---

# 45. Cache-Control

Controls caching behavior.

Example:

```http
Cache-Control: no-cache
```

Caching can dramatically improve API performance when used correctly.

---

# PART 8 — HTTP RESPONSE

# 46. HTTP Response

Example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 123,
  "name": "Sai"
}
```

Contains:

```text
Status code
Headers
Body
```

---

# 47. HTTP Status Codes

Five broad classes:

```text
1xx = informational
2xx = success
3xx = redirection
4xx = client/request problem
5xx = server problem
```

---

# 48. 200 OK

Request succeeded.

Common for:

```http
GET /users/123
```

---

# 49. 201 Created

A resource was successfully created.

Common after:

```http
POST /users
```

---

# 50. 204 No Content

Request succeeded but there is no response body.

Common example:

```http
DELETE /users/123
```

---

# 51. 400 Bad Request

Request is invalid.

Examples:

- Malformed JSON
- Invalid parameter
- Invalid request structure

---

# 52. 401 Unauthorized

Authentication is missing or invalid.

Think:

> "Who are you?"

---

# 53. 403 Forbidden

The server understands who you are but you do not have permission.

Think:

> "I know who you are, but you cannot do this."

---

# 54. 404 Not Found

Requested resource/route could not be found.

---

# 55. 409 Conflict

Request conflicts with current resource state.

Example:

```text
Creating a user with an already-used unique username
```

---

# 56. 422 Unprocessable Content

Request structure may be valid, but semantic validation fails.

Example:

```json
{
  "age": -10
}
```

---

# 57. 429 Too Many Requests

Client exceeded a rate limit.

Often accompanied by:

```http
Retry-After
```

---

# 58. 500 Internal Server Error

Generic server-side failure.

---

# 59. 502 Bad Gateway

A gateway/proxy received an invalid response from an upstream server.

---

# 60. 503 Service Unavailable

Service is temporarily unable to handle the request.

Possible reasons:

- Maintenance
- Overload
- Dependency unavailable

---

# 61. 504 Gateway Timeout

Gateway/proxy did not receive a timely response from an upstream service.

---

# PART 9 — JSON

# 62. What is JSON?

JSON = **JavaScript Object Notation**.

It is a text-based data interchange format.

Example:

```json
{
  "id": 123,
  "name": "Sai",
  "skills": ["SQL", "data platform", "Python"],
  "active": true
}
```

---

# 63. Why is JSON popular for APIs?

Because it is:

- Human-readable
- Lightweight
- Language-independent
- Easy to parse
- Supported by almost every programming language
- Naturally compatible with JavaScript/web applications

---

# 64. JSON Data Types

JSON supports:

```text
String
Number
Boolean
Object
Array
null
```

Example:

```json
{
  "name": "Sai",
  "age": 30,
  "active": true,
  "skills": ["SQL", "Python"],
  "manager": null,
  "address": {
    "city": "Bengaluru"
  }
}
```

---

# 65. Why not XML?

XML is powerful and supports rich document structures, namespaces and attributes.

But JSON is often preferred for modern web APIs because it is generally:

- Less verbose
- Easier to read
- Easier to consume in web applications

XML remains important in many enterprise and legacy systems, especially SOAP.

---

# PART 10 — YAML

# 66. What is YAML?

YAML is a human-friendly data serialization/configuration format.

Example:

```yaml
name: Sai
role: Data Engineer
skills:
  - SQL
  - data platform
  - Python
```

---

# 67. Why do we use YAML?

YAML is especially popular for:

- Configuration
- CI/CD
- Kubernetes
- Docker Compose
- OpenAPI definitions
- Infrastructure tooling
- Application settings

---

# 68. YAML vs JSON

| YAML | JSON |
|---|---|
| Human-friendly | Machine/data-exchange friendly |
| Less punctuation | More explicit syntax |
| Supports comments | Standard JSON does not support comments |
| Common for configuration | Common for API payloads |
| Indentation-sensitive | Braces/brackets define structure |

---

# 69. Why not use YAML for API responses?

You can, but JSON is much more common for HTTP APIs.

JSON has:

- Broad browser support
- Mature API tooling
- Straightforward parsing
- Standardized media type
- Familiarity across clients

YAML is especially valuable for **configuration and API specifications**, not because APIs cannot return it.

---

# 70. YAML Indentation

This is valid:

```yaml
user:
  name: Sai
  role: Engineer
```

This can change the meaning:

```yaml
user:
name: Sai
role: Engineer
```

YAML is indentation-sensitive.

---

# PART 11 — API AUTHENTICATION

# 71. Authentication vs Authorization

### Authentication

Answers:

> **Who are you?**

### Authorization

Answers:

> **What are you allowed to do?**

Example:

```text
Login
 ↓
Authentication
 ↓
User identity
 ↓
Authorization
 ↓
Permissions
```

---

# 72. API Key

A client receives a key such as:

```text
X-API-Key: abc123
```

The server uses it to identify/control access.

### Advantages

Simple.

### Weaknesses

Usually less expressive than modern delegated authorization systems and must be protected carefully.

---

# 73. Basic Authentication

Example:

```http
Authorization: Basic <base64>
```

It encodes username/password credentials.

**Base64 is not encryption.**

Therefore Basic Auth should be used over HTTPS.

---

# 74. Bearer Token

Example:

```http
Authorization: Bearer eyJ...
```

The client presents a token proving it has a credential accepted by the server.

---

# 75. JWT

JWT = **JSON Web Token**.

Typical structure:

```text
header.payload.signature
```

Example conceptual payload:

```json
{
  "sub": "123",
  "role": "admin",
  "exp": 1780000000
}
```

JWTs can carry claims.

### Important

A JWT being signed does not mean its payload is secret.

Do not put sensitive secrets into an ordinary JWT payload.

---

# 76. OAuth 2.0

OAuth 2.0 is an authorization framework for delegated access.

Example:

```text
User
 ↓
Client Application
 ↓
Authorization Server
 ↓
Access Token
 ↓
Resource Server/API
```

A common example is:

> "Allow this application to access my profile."

---

# 77. OAuth vs JWT

This is a common interview question.

**OAuth 2.0** defines an authorization framework.

**JWT** is a token format.

They are not alternatives.

OAuth access tokens may be JWTs, but OAuth does not require JWT access tokens.

---

# 78. Refresh Token

A refresh token can be used to obtain a new access token without requiring the user to authenticate interactively again.

Conceptually:

```text
Refresh Token
     ↓
Authorization Server
     ↓
New Access Token
```

---

# PART 12 — API DESIGN

# 79. Good REST Resource Naming

Prefer nouns:

```http
/users
/orders
/products
```

Instead of:

```http
/getUsers
/createOrder
/deleteProduct
```

The HTTP method expresses the operation.

---

# 80. Versioning

Common approach:

```http
/api/v1/users
/api/v2/users
```

Why?

Because clients may depend on the old contract.

Versioning allows controlled evolution.

---

# 81. Pagination

Large datasets should not normally be returned in one response.

Example:

```http
GET /users?page=2&limit=50
```

Response:

```json
{
  "data": [...],
  "page": 2,
  "limit": 50,
  "total": 1000
}
```

---

# 82. Offset vs Cursor Pagination

### Offset

```text
?page=10&limit=50
```

Easy but can become inefficient/inconsistent for frequently changing large datasets.

### Cursor

```text
?cursor=abc123&limit=50
```

The cursor represents a position in the result set.

Cursor pagination is often better for large/changing datasets.

---

# 83. Filtering

```http
GET /orders?status=completed
```

---

# 84. Sorting

```http
GET /orders?sort=created_at
```

---

# 85. Searching

```http
GET /products?search=laptop
```

---

# 86. API Error Response

A useful error should tell the client what happened without leaking sensitive internals.

Example:

```json
{
  "error": {
    "code": "INVALID_EMAIL",
    "message": "The email address is invalid.",
    "request_id": "abc-123"
  }
}
```

Avoid exposing:

```text
database passwords
stack traces
internal hostnames
SQL queries
secrets
```

---

# PART 13 — IDEMPOTENCY

# 87. Why is Idempotency Important?

Imagine a payment request:

```http
POST /payments
```

The client sends the request.

The server processes the payment.

But the network times out before the client receives the response.

The client retries.

Without idempotency protection:

```text
Payment #1
Payment #2
```

could accidentally occur.

---

# 88. Idempotency Key

Client sends:

```http
Idempotency-Key: order-123-payment-1
```

The server records the key and result.

If the same request arrives again:

```text
Same key
   ↓
Already processed
   ↓
Return previous result
```

This is extremely important in financial APIs.

---

# PART 14 — CACHING

# 89. What is API Caching?

Caching stores a response so it can be reused instead of recomputed.

```text
Client
 ↓
Cache
 ↓ hit
Response
```

If there is a miss:

```text
Client
 ↓
Cache miss
 ↓
API
 ↓
Database
```

---

# 90. Why Cache?

Benefits:

- Lower latency
- Lower database load
- Higher throughput
- Better scalability

Risks:

- Stale data
- Cache invalidation complexity
- Memory usage
- Security concerns

---

# PART 15 — CORS

# 91. What is CORS?

CORS = **Cross-Origin Resource Sharing**.

Browsers enforce same-origin security rules.

CORS lets a server explicitly indicate which browser origins may access it.

Example response:

```http
Access-Control-Allow-Origin: https://example.com
```

---

# 92. Why does CORS matter?

Suppose:

```text
Frontend:
https://app.example.com

API:
https://api.example.com
```

The browser may apply cross-origin restrictions.

CORS headers tell the browser which cross-origin requests are allowed.

---

# 93. Preflight Request

For certain cross-origin requests, the browser sends:

```http
OPTIONS
```

first.

This checks whether the actual request is permitted.

---

# PART 16 — WEBHOOKS

# 94. What is a Webhook?

A webhook is a mechanism where one system sends an HTTP request to another system when an event occurs.

Traditional polling:

```text
Client → "Anything new?"
Client → "Anything new?"
Client → "Anything new?"
```

Webhook:

```text
Event happens
     ↓
Provider
     ↓
POST webhook
     ↓
Your system
```

---

# 95. Webhook vs Polling

| Webhook | Polling |
|---|---|
| Event-driven | Repeated requests |
| Lower unnecessary traffic | Can generate many empty requests |
| Near real-time | Depends on polling interval |
| Receiver must expose endpoint | Client initiates requests |

---

# PART 17 — SYNCHRONOUS VS ASYNCHRONOUS APIs

# 96. Synchronous API

Client waits for the operation to finish.

```text
Client → Request → Server
                 |
                 | processing
                 v
Client ← Response
```

Good for fast operations.

---

# 97. Asynchronous API

Client does not need to wait for the full operation.

Example:

```text
POST /reports
       ↓
202 Accepted
       ↓
Job ID = 123
       ↓
Background processing
       ↓
GET /reports/123
```

Useful for long-running jobs.

---

# 98. 200 vs 202

```text
200 = completed successfully
202 = accepted for processing, not necessarily completed
```

This distinction is important for asynchronous APIs.

---

# PART 18 — OPENAPI AND YAML

# 99. What is OpenAPI?

OpenAPI is a standard specification for describing HTTP APIs.

It can describe:

- Endpoints
- Methods
- Parameters
- Request bodies
- Responses
- Authentication
- Schemas

Example:

```yaml
openapi: 3.0.3

info:
  title: User API
  version: 1.0.0

paths:
  /users/{id}:
    get:
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: integer

      responses:
        "200":
          description: User found
```

### Why YAML here?

OpenAPI can be represented in YAML or JSON.

YAML is popular because API specifications can become large and YAML is easier for humans to read/edit.

---

# 100. OpenAPI vs API

Important distinction:

```text
API = actual interface
OpenAPI = description/specification of the interface
```

Think:

```text
API
 ↓
What the system actually exposes

OpenAPI
 ↓
Documentation/contract describing it
```

---

# PART 19 — TOOLS

# 101. cURL

cURL is a command-line tool for making HTTP requests.

Example:

```bash
curl https://api.example.com/users
```

POST:

```bash
curl -X POST \
  -H "Content-Type: application/json" \
  -d '{"name":"Sai"}' \
  https://api.example.com/users
```

---

# 102. Postman

Postman is a tool for developing and testing APIs.

You can:

- Send requests
- Set headers
- Add authentication
- Define variables
- Inspect responses
- Build collections
- Write tests

---

# 103. Python API Call

Example:

```python
import requests

response = requests.get(
    "https://api.example.com/users/123",
    timeout=10
)

print(response.status_code)
print(response.json())
```

---

# PART 20 — API TESTING

# 104. What should I test?

Test:

### Functional

```text
Correct response
Correct status code
Correct data
```

### Negative

```text
Missing parameter
Invalid JSON
Invalid token
Unauthorized user
```

### Boundary

```text
0
1
maximum allowed value
very large input
empty string
null
```

### Security

```text
Unauthorized access
Privilege escalation
Injection
Sensitive data exposure
```

### Performance

```text
Latency
Throughput
Concurrency
Timeout behavior
```

---

# 105. Unit vs Integration vs End-to-End

### Unit test

Tests one small component.

### Integration test

Tests interaction between components.

Example:

```text
API → Database
```

### End-to-End

Tests the complete workflow.

```text
Client
 ↓
API
 ↓
Service
 ↓
Database
 ↓
Response
```

---

# PART 21 — API SECURITY

# 106. HTTPS/TLS

Always protect sensitive API communication with HTTPS.

---

# 107. SQL Injection

Bad:

```python
query = "SELECT * FROM users WHERE name = '" + name + "'"
```

Safer:

```python
cursor.execute(
    "SELECT * FROM users WHERE name = %s",
    (name,)
)
```

Use parameterized queries.

---

# 108. Authentication Security

Do not:

- Put passwords in URLs
- Hard-code secrets
- Log access tokens
- Commit API keys to Git
- Return unnecessary sensitive data

---

# 109. Rate Limiting

Rate limiting restricts how many requests a client can make.

Example:

```text
100 requests/minute
```

Why?

- Prevent abuse
- Protect infrastructure
- Control cost
- Improve fairness

---

# 110. API Gateway

An API Gateway sits between clients and backend services.

```text
Client
  ↓
API Gateway
  ↓
 ┌──────────────┐
 ↓              ↓
User Service   Order Service
```

It may handle:

- Routing
- Authentication
- Rate limiting
- TLS termination
- Logging
- Monitoring
- Request transformation

---

# PART 22 — MICROSERVICES

# 111. What is a Microservice?

A microservice is a relatively small independently deployable service focused on a business capability.

Example:

```text
User Service
Order Service
Payment Service
Inventory Service
```

They may communicate through APIs.

---

# 112. API in Microservices

Example:

```text
Order Service
      |
      | POST /payments
      v
Payment Service
```

The API becomes the contract between services.

---

# PART 23 — API GATEWAY VS LOAD BALANCER

# 113. Load Balancer

Primarily distributes traffic across backend instances.

```text
Client
 ↓
Load Balancer
 ↓ ↓ ↓
App App App
```

---

# 114. API Gateway

Provides API-specific management capabilities.

```text
Client
 ↓
API Gateway
 ↓
Services
```

A gateway can route based on API paths, enforce authentication and rate limits, and apply API policies.

They can coexist.

---

# PART 24 — RETRIES AND TIMEOUTS

# 115. Why do APIs need timeouts?

Without a timeout, a client can wait indefinitely.

Example:

```text
Client
  |
  | request
  v
Slow Service
  |
  | never responds
```

Timeouts prevent resources from being held forever.

---

# 116. Why can retries be dangerous?

Suppose:

```text
POST /payment
```

times out.

Client retries.

The first request may have succeeded even though the response was lost.

Therefore retries should be designed with:

- Idempotency
- Exponential backoff
- Maximum retry limits
- Retryable-status classification

---

# 117. Exponential Backoff

Instead of:

```text
retry immediately
retry immediately
retry immediately
```

use increasing delays:

```text
1 sec
2 sec
4 sec
8 sec
```

Often with jitter to avoid synchronized retry storms.

---

# PART 25 — OBSERVABILITY

# 118. What should I monitor for APIs?

Important metrics:

```text
Request count
Error rate
Latency
Throughput
CPU
Memory
Database latency
Timeouts
Rate-limit events
```

---

# 119. Correlation / Request ID

Example:

```http
X-Request-ID: abc-123
```

The same identifier can be included in logs across services.

```text
Client
 ↓ request-id abc
Gateway
 ↓ abc
Service
 ↓ abc
Database-related logs
```

This makes troubleshooting distributed systems much easier.

---

# PART 26 — API DESIGN INTERVIEW QUESTIONS

# 120. Design a User API

Possible endpoints:

```http
GET    /users
GET    /users/{id}
POST   /users
PATCH  /users/{id}
DELETE /users/{id}
```

Consider:

- Authentication
- Authorization
- Validation
- Pagination
- Error format
- Versioning
- Rate limiting
- Logging
- Idempotency where needed

---

# 121. Design a Payment API

Possible:

```http
POST /payments
GET  /payments/{id}
```

Important considerations:

```text
HTTPS
Authentication
Authorization
Idempotency
Validation
Fraud controls
Audit logging
Retries
Timeouts
Rate limiting
```

The key interview point is **idempotency**.

---

# 122. Design a Data-Ingestion API

Example:

```http
POST /ingestion/jobs
GET  /ingestion/jobs/{job_id}
```

Flow:

```text
Client
 ↓
POST ingestion job
 ↓
API validates request
 ↓
202 Accepted
 ↓
Job queue
 ↓
Worker
 ↓
Storage/Warehouse
 ↓
Validation
 ↓
Job completed
```

This is better than making the client wait for a large ingestion job.

---

# PART 27 — API DATA CONSUMPTION PATTERNS

## 123. API Client Design

A good API client should centralize:

- Base URL
- Authentication
- Headers
- Timeout
- Retry policy
- Error handling
- Serialization/deserialization
- Logging

Instead of repeating these in every API call:

```python
client.get("/users")
client.get("/orders")
client.post("/payments")
```

This keeps API consumption consistent and maintainable.

---

## 124. API Pagination

An API may return only a limited number of records per response.

Example:

```http
GET /users?page=1&limit=100
```

A client may need to continue until there are no more results.

Common approaches:

```text
Offset/page pagination
Cursor pagination
Link-based pagination
```

---

## 125. API Incremental Retrieval

Some APIs support parameters such as:

```text
updated_since
created_after
from
to
```

This allows a client to retrieve only changed data instead of requesting everything repeatedly.

Conceptually:

```text
Last successful timestamp
        ↓
API request
        ↓
Changed records
        ↓
Process
        ↓
Save checkpoint
```

---

## 126. API Rate Limits

An API provider may limit requests:

```text
100 requests/minute
```

A well-designed client should:

- Detect 429 responses
- Respect `Retry-After`
- Use backoff
- Avoid unnecessary requests
- Batch requests where supported

---

## 127. API Response Size

Large responses can increase:

- Network usage
- Memory usage
- Processing time
- Latency

APIs may support:

```text
Pagination
Field selection
Filtering
Compression
```

For example:

```http
GET /users?fields=id,name,email
```

---

# PART 28 — API DATA FORMATS

## 128. JSON API Response

Example:

```json
{
  "order_id": 101,
  "customer": {
    "id": 20,
    "name": "Sai"
  },
  "items": [
    {
      "product_id": 1,
      "quantity": 2
    }
  ]
}
```

The client must correctly understand:

- Objects
- Arrays
- Nested structures
- Optional fields
- Null values
- Data types

---

## 129. JSON Schema

JSON Schema can describe and validate the structure of JSON data.

Example:

```json
{
  "type": "object",
  "required": ["id", "name"],
  "properties": {
    "id": {
      "type": "integer"
    },
    "name": {
      "type": "string"
    }
  }
}
```

It can help detect malformed or unexpected data.

---

## 130. Serialization and Deserialization

### Serialization

Convert an in-memory object into a transferable representation.

```text
Python object
     ↓
JSON
```

### Deserialization

Convert the received representation back into an application object.

```text
JSON
 ↓
Python object
```

---

# PART 29 — API SCHEMA EVOLUTION

## 131. What if an API changes?

Old:

```json
{
  "customer_name": "Sai"
}
```

New:

```json
{
  "customer": {
    "name": "Sai"
  }
}
```

A robust client should not blindly assume the old structure forever.

Use:

- Versioning
- Schema validation
- Contract testing
- Backward-compatible changes
- Monitoring
- Clear deprecation notices

---

## 132. Backward Compatibility

A change is backward compatible when existing clients continue to work.

Generally safer:

```text
Add an optional response field
```

Potentially breaking:

```text
Remove an existing field
Change its type
Change its meaning
Require a previously optional request field
```

---

# PART 30 — API SECURITY

## 133. HTTPS/TLS

Use HTTPS to protect API communication in production.

TLS provides:

- Encryption
- Server authentication
- Integrity protection

---

## 134. Never Put Secrets in URLs

Avoid:

```http
GET /users?api_key=SECRET
```

URLs may appear in:

- Logs
- Browser history
- Monitoring systems
- Proxy records

Use appropriate authentication headers instead.

---

## 135. SQL Injection

If an API receives user input and constructs SQL unsafely, attackers may manipulate the query.

Bad:

```python
query = "SELECT * FROM users WHERE name = '" + name + "'"
```

Prefer parameterized queries:

```python
cursor.execute(
    "SELECT * FROM users WHERE name = %s",
    (name,)
)
```

---

## 136. Sensitive Data Exposure

Do not return unnecessary sensitive information.

Avoid exposing:

```text
Passwords
Secrets
Internal tokens
Database credentials
Internal stack traces
Private infrastructure details
```

---

## 137. Input Validation

Validate:

```text
Required fields
Data types
Length
Range
Allowed values
Formats
```

Example:

```json
{
  "age": -10
}
```

should be rejected if age must be positive.

---

# PART 31 — API RELIABILITY

## 138. Timeouts

Every external API call should have a sensible timeout.

Example:

```python
requests.get(url, timeout=10)
```

Without a timeout, a client could wait indefinitely.

---

## 139. Retries

Retry only when the failure may be temporary.

Potential retry cases:

```text
Timeout
503
Some 502 failures
429 according to server guidance
```

Do not blindly retry every error.

---

## 140. Exponential Backoff

Instead of:

```text
retry immediately
retry immediately
retry immediately
```

use:

```text
1 second
2 seconds
4 seconds
8 seconds
```

Usually add jitter to avoid many clients retrying simultaneously.

---

## 141. Circuit Breaker

If a dependency repeatedly fails:

```text
Service A → Service B
              X
```

A circuit breaker can temporarily stop calls to the unhealthy dependency.

Typical states:

```text
Closed
Open
Half-Open
```

Purpose:

> Prevent repeated failures from spreading through the system.

---

## 142. Bulkhead Pattern

Separate resources for different workloads.

Example:

```text
Payment API calls → Resource Pool A
Reporting API calls → Resource Pool B
```

A problem in one workload is less likely to consume all resources.

---

# PART 32 — API OBSERVABILITY

## 143. What should we monitor?

Important metrics:

```text
Request count
Error rate
Latency
Throughput
Timeouts
429 responses
5xx responses
Dependency latency
CPU
Memory
```

---

## 144. Request ID

Example:

```http
X-Request-ID: abc-123
```

The identifier can appear in logs across services.

```text
Client
 ↓ abc-123
Gateway
 ↓ abc-123
Service
 ↓ abc-123
Dependency
```

This makes troubleshooting much easier.

---

## 145. Distributed Tracing

In a microservice architecture:

```text
Client
 ↓
Gateway
 ↓
Order Service
 ↓
Payment Service
 ↓
Database
```

Distributed tracing helps identify which component caused latency or failure.

---

## 146. Logs vs Metrics vs Traces

### Logs

Detailed events.

```text
Payment request failed
```

### Metrics

Numerical measurements.

```text
Error rate = 2.4%
```

### Traces

Follow one request across multiple services.

---

# PART 33 — API GATEWAY

## 147. What is an API Gateway?

An API Gateway is a front door for API traffic.

```text
Client
  ↓
API Gateway
  ↓
 ┌──────────────┐
 ↓              ↓
User Service   Order Service
```

It may handle:

- Routing
- Authentication
- Rate limiting
- TLS termination
- Logging
- Request transformation
- API policies

---

## 148. API Gateway vs Load Balancer

### Load Balancer

Primarily distributes traffic across instances.

```text
Client
 ↓
Load Balancer
 ↓ ↓ ↓
App App App
```

### API Gateway

Provides API-aware capabilities such as:

```text
Routing
Authentication
Rate limiting
API policies
Request transformation
```

They can coexist.

---

# PART 34 — API ARCHITECTURE PATTERNS

## 149. Backend for Frontend

A BFF provides an API tailored to a specific frontend.

```text
Mobile → Mobile BFF
Web    → Web BFF
```

This can reduce frontend complexity when different clients need different data.

---

## 150. API Composition

One API calls multiple services and combines their responses.

```text
Client
 ↓
Composition API
 ├── User Service
 ├── Order Service
 └── Payment Service
       ↓
Combined Response
```

---

## 151. Microservice API Communication

Example:

```text
Order Service
      |
      | POST /payments
      v
Payment Service
```

The API acts as a contract between services.

---

# PART 35 — ASYNC APIs AND MESSAGING

## 152. Request/Response vs Event-Driven

Request/response:

```text
A → request → B
A ← response ← B
```

Event-driven:

```text
A → Event → Broker
              ├→ B
              ├→ C
              └→ D
```

---

## 153. Message Queue

A queue temporarily holds messages.

```text
Producer
   ↓
Queue
   ↓
Consumer
```

Useful for:

- Decoupling
- Buffering
- Asynchronous processing
- Retry handling

---

## 154. Event vs Command

### Command

Usually means:

> "Please perform this action."

Example:

```text
CreateOrder
```

### Event

Usually means:

> "This already happened."

Example:

```text
OrderCreated
```

This distinction is important in event-driven architectures.

---

## 155. Webhook Reliability

A webhook receiver should assume:

```text
Duplicate delivery
Delayed delivery
Out-of-order delivery
Temporary failure
```

Good practices:

- Verify signatures
- Return successful responses quickly
- Process asynchronously when appropriate
- Make processing idempotent
- Store event IDs
- Retry safely

---

# PART 36 — API CONTRACTS

## 156. What is an API Contract?

An API contract defines expectations between provider and consumer.

It can specify:

```text
Endpoint
Method
Parameters
Request schema
Response schema
Status codes
Authentication
Errors
```

---

## 157. Contract Testing

Contract testing verifies that the provider and consumer agree on the expected interface.

```text
Consumer expectation
        ↕
Provider implementation
```

It helps detect breaking changes before production.

---

# PART 37 — API LIFECYCLE

## 158. API Lifecycle

```text
Design
 ↓
Specification
 ↓
Development
 ↓
Testing
 ↓
Security Review
 ↓
Deployment
 ↓
Monitoring
 ↓
Versioning
 ↓
Deprecation
```

---

## 159. API Deprecation

A typical migration:

```text
v1
 ↓
Deprecated
 ↓
Migration period
 ↓
v2
 ↓
v1 removed
```

Clients should receive clear migration guidance.

---

# PART 38 — API DESIGN SCENARIOS

## 160. Design a User API

Possible endpoints:

```http
GET    /users
GET    /users/{id}
POST   /users
PATCH  /users/{id}
DELETE /users/{id}
```

Consider:

- Authentication
- Authorization
- Validation
- Pagination
- Error format
- Versioning
- Rate limiting
- Logging

---

## 161. Design a Payment API

Possible:

```http
POST /payments
GET  /payments/{id}
```

Important:

```text
HTTPS
Authentication
Authorization
Idempotency
Validation
Audit logging
Retries
Timeouts
Rate limiting
```

---

## 162. Design a Long-Running Job API

Example:

```http
POST /reports
GET  /reports/{job_id}
```

Flow:

```text
Client
 ↓
POST /reports
 ↓
202 Accepted
 ↓
Job ID
 ↓
Background processing
 ↓
GET /reports/{job_id}
```

---

# PART 39 — PRODUCTION TROUBLESHOOTING

## 163. API Returns 401

Check:

```text
Token present?
Token expired?
Correct authentication scheme?
Credentials valid?
```

---

## 164. API Returns 403

Check:

```text
User identity
Role
Permissions
Resource policy
```

---

## 165. API Returns 404

Check:

```text
Correct URL?
Correct HTTP method?
Correct version?
Correct resource ID?
Route exists?
```

---

## 166. API Returns 429

Check:

```text
Rate limit
Retry-After
Request frequency
Backoff
Batching
```

---

## 167. API Returns 500

Check:

```text
Server logs
Request ID
Database
Dependencies
Recent deployment
Application errors
```

---

## 168. API is Slow

Measure each layer:

```text
Client
 ↓
Gateway
 ↓
Application
 ↓
Database
 ↓
External dependency
```

Do not guess. Identify where the latency occurs.

---

## 169. API Works in Postman but Not Browser

Investigate:

```text
CORS
Preflight
Cookies
Authentication
Browser security policies
```

---

# PART 40 — ADVANCED API CONCEPTS

## 170. API Aggregation

Combining multiple backend results into one response.

Useful when:

```text
Client needs data from many services
```

Trade-off:

The aggregator becomes responsible for coordinating multiple dependencies.

---

## 171. Fan-Out

One incoming request triggers multiple downstream calls.

```text
Request
  |
  ├── Service A
  ├── Service B
  └── Service C
```

Problems can include:

- Increased latency
- Dependency failures
- Resource exhaustion

---

## 172. Fan-In

Multiple results are combined into one response.

```text
A ─┐
B ─┼→ Aggregator → Client
C ─┘
```

---

## 173. Graceful Degradation

If an optional dependency fails, return partial functionality instead of failing the entire request.

Example:

```text
Product data → available
Recommendations → temporarily unavailable
```

The product page may still work.

---

## 174. Eventual Consistency

In distributed systems, different services may temporarily have different views of data.

Example:

```text
Order created
 ↓
Order Service updated immediately
 ↓
Inventory Service updated shortly after
```

The systems become consistent over time.

---

# PART 41 — API PERFORMANCE

## 175. Latency

Latency is the time taken to complete a request.

Example:

```text
Request → 120 ms → Response
```

---

## 176. Throughput

Throughput measures how much work the system handles over time.

Example:

```text
5,000 requests/second
```

---

## 177. Latency vs Throughput

```text
Latency
→ How long one request takes

Throughput
→ How much traffic/work can be handled
```

A system can have high throughput while individual requests still have significant latency.

---

## 178. Connection Pooling

Creating a new network connection for every request can be expensive.

Connection pooling reuses connections.

```text
Pool
 ├── Connection 1
 ├── Connection 2
 ├── Connection 3
 └── Connection 4
```

This can reduce connection setup overhead.

---

## 179. Compression

Responses can be compressed to reduce network transfer size.

Example:

```http
Accept-Encoding: gzip
```

Useful for large text responses.

---

# PART 42 — API CACHING

## 180. What is API Caching?

Caching stores data/results so they can be reused.

```text
Client
 ↓
Cache
 ↓ hit
Response
```

Cache miss:

```text
Client
 ↓
Cache miss
 ↓
API
 ↓
Database
```

---

## 181. Cache-Control

Example:

```http
Cache-Control: max-age=3600
```

This communicates caching behavior.

---

## 182. Cache Risks

Caching can cause:

```text
Stale data
Invalidation problems
Memory usage
Security issues
```

Never cache sensitive data carelessly.

---

# PART 43 — COMPLETE API COMPARISON

| Technology/Concept | Main Purpose |
|---|---|
| API | Software interface |
| HTTP | Web communication protocol |
| HTTPS | HTTP protected by TLS |
| REST | Architectural style |
| SOAP | Web-service protocol |
| GraphQL | Query-based API approach |
| gRPC | RPC framework |
| JSON | Data interchange format |
| YAML | Configuration/specification format |
| OpenAPI | API specification |
| API Key | Credential mechanism |
| OAuth 2.0 | Authorization framework |
| JWT | Token format |
| Webhook | Event notification |
| Polling | Repeated client requests |
| API Gateway | API traffic/policy layer |
| Load Balancer | Traffic distribution |
| Rate Limiting | Controls request frequency |
| Idempotency | Safe repeated operations |
| Pagination | Splits large results |
| Caching | Reuses data/responses |
| CORS | Browser cross-origin mechanism |
| Queue | Asynchronous message buffering |
| Circuit Breaker | Protects failing dependencies |
| Contract Testing | Validates API agreements |

---

# PART 44 — 50 HIGH-VALUE INTERVIEW QUESTIONS

## Fundamentals

1. What is an API?
2. Why do we need APIs?
3. API vs UI?
4. API vs database?
5. What is a client?
6. What is a server?
7. What is DNS?
8. What is a port?
9. What is a protocol?
10. What happens when an API is called?

## HTTP

11. What is HTTP?
12. Why is HTTP commonly used for APIs?
13. Why not TCP directly?
14. HTTP vs HTTPS?
15. HTTP vs FTP?
16. What is an HTTP request?
17. What is an HTTP response?
18. What are headers?
19. What is Content-Type?
20. What is Accept?

## Methods

21. GET vs POST?
22. PUT vs PATCH?
23. DELETE?
24. HEAD?
25. OPTIONS?
26. What is safe?
27. What is idempotent?
28. Why is idempotency important?

## Status Codes

29. 200 vs 201?
30. 400 vs 401?
31. 401 vs 403?
32. 404?
33. 409?
34. 422?
35. 429?
36. 500 vs 502 vs 503 vs 504?

## Data Formats

37. What is JSON?
38. Why JSON?
39. JSON vs XML?
40. What is YAML?
41. Why YAML?
42. YAML vs JSON?
43. Why is YAML common in OpenAPI?

## Security

44. Authentication vs authorization?
45. API key vs OAuth?
46. OAuth vs JWT?
47. What is CORS?
48. What is rate limiting?
49. How do you secure an API?
50. How do you prevent duplicate payment requests?

---

# PART 45 — 25 SCENARIO QUESTIONS

## Scenario 1 — 429

**Your API suddenly returns 429. What do you do?**

```text
Confirm rate limit
 ↓
Read Retry-After
 ↓
Reduce request frequency
 ↓
Use backoff
 ↓
Batch efficiently
 ↓
Monitor recovery
```

---

## Scenario 2 — 401

**An API returns 401.**

Check:

```text
Token
Expiration
Authentication scheme
Credentials
```

---

## Scenario 3 — 403

**An API returns 403.**

Check:

```text
Role
Permissions
Resource policy
```

---

## Scenario 4 — Slow API

**An API suddenly becomes slow.**

Check:

```text
Gateway
Application
Database
External dependencies
Network
CPU/memory
```

---

## Scenario 5 — Payment Timeout

**A payment request times out. Should you retry?**

Not blindly.

Ask:

```text
Could the first request have succeeded?
Is an idempotency key available?
Is the operation safe to retry?
```

---

## Scenario 6 — Huge Response

**An API returns one million records.**

Consider:

```text
Pagination
Filtering
Field selection
Compression
Incremental retrieval
```

---

## Scenario 7 — API Schema Changed

**A provider changed the response structure.**

Use:

```text
Schema validation
Versioning
Contract tests
Backward compatibility
Monitoring
```

---

## Scenario 8 — Browser Failure

**Postman works but browser fails.**

Investigate:

```text
CORS
Preflight
Cookies
Authentication
Browser security
```

---

## Scenario 9 — 500 Error

**API returns 500.**

Investigate:

```text
Logs
Request ID
Database
Dependencies
Recent deployment
```

---

## Scenario 10 — Long Operation

**Generating a report takes 10 minutes.**

Do not make the client wait for 10 minutes.

Prefer:

```http
POST /reports
```

Return:

```http
202 Accepted
```

with a job ID.

Then:

```http
GET /reports/{job_id}
```

---

# PART 46 — API PROJECT ROADMAP

## Project 1 — Beginner

Build:

```text
User CRUD API
```

Learn:

- GET
- POST
- PUT
- PATCH
- DELETE
- JSON
- Status codes
- Request/response
- Path/query parameters

---

## Project 2 — Intermediate

Build:

```text
Authentication API
```

Learn:

- API keys
- JWT
- Authentication
- Authorization
- Roles
- Error handling
- Password security

---

## Project 3 — External API Client

Build:

```text
Application
   ↓
External REST API
   ↓
Pagination
   ↓
Rate-limit handling
   ↓
Retries
   ↓
Caching
```

Learn:

- Real API consumption
- Authentication
- Pagination
- Rate limits
- Retry/backoff
- Timeouts
- Error handling

---

## Project 4 — Webhook System

Build:

```text
External Provider
       ↓
Webhook
       ↓
Your API
       ↓
Queue
       ↓
Worker
```

Add:

- Signature verification
- Idempotency
- Event IDs
- Retry handling
- Logging

---

## Project 5 — Production-Style Microservices

Build:

```text
API Gateway
      ↓
 ┌────┼────┐
 ↓    ↓    ↓
User Order Payment
Service Service Service
```

Add:

- Authentication
- Authorization
- Rate limiting
- Request IDs
- Timeouts
- Retries
- Circuit breaker
- Logging
- Metrics
- Tracing

---

# PART 47 — FINAL API MENTAL MODEL

When someone says:

> "Build an API."

Think:

```text
                  CLIENT
                    |
                    v
                   DNS
                    |
                    v
                HTTPS / HTTP
                    |
                    v
               API GATEWAY
                    |
          Authentication
                    |
          Authorization
                    |
                    v
                API SERVER
                    |
              Business Logic
                    |
          ┌─────────┴─────────┐
          v                   v
      Database          External API
          |                   |
          └─────────┬─────────┘
                    v
                Response
                    |
                    v
              JSON / XML
                    |
                    v
                 CLIENT
```

For production, think beyond the request:

```text
Security
Authentication
Authorization
Validation
Timeouts
Retries
Idempotency
Rate limiting
Caching
Pagination
Versioning
Logging
Metrics
Tracing
Testing
Monitoring
Failure handling
```

---

# PART 48 — ONE-PAGE API CHEAT SHEET

```text
API
→ Software-to-software interface

HTTP
→ Web communication protocol

HTTPS
→ HTTP + TLS

REST
→ Architectural style

JSON
→ Common API data format

YAML
→ Human-friendly configuration/specification format

GET
→ Retrieve

POST
→ Create/trigger

PUT
→ Replace/update representation

PATCH
→ Partial modification

DELETE
→ Delete

200
→ Success

201
→ Created

204
→ Success/no body

400
→ Bad request

401
→ Authentication problem

403
→ Permission problem

404
→ Not found

409
→ Conflict

422
→ Semantic validation error

429
→ Rate limited

500
→ Server error

502
→ Bad gateway/upstream response problem

503
→ Service unavailable

504
→ Gateway timeout

Authentication
→ Who are you?

Authorization
→ What can you do?

OAuth
→ Authorization framework

JWT
→ Token format

API Key
→ Credential mechanism

CORS
→ Browser cross-origin access mechanism

Webhook
→ Server sends event notification

Polling
→ Client repeatedly asks

OpenAPI
→ API specification

Gateway
→ API traffic/policy layer

Rate Limit
→ Controls request frequency

Idempotency
→ Safe repeated operation

Pagination
→ Split large result sets

Cache
→ Reuse previous result

Timeout
→ Stop waiting indefinitely

Retry
→ Try again carefully

Backoff
→ Wait progressively longer

Circuit Breaker
→ Stop calling failing dependency

Queue
→ Buffer asynchronous messages
```

---

# FINAL INTERVIEW RULE

For any API question, do not answer only with a definition.

Use:

```text
WHAT
 ↓
WHY
 ↓
HOW
 ↓
WHY THIS INSTEAD OF ALTERNATIVES
 ↓
REAL-WORLD EXAMPLE
 ↓
FAILURE SCENARIO
 ↓
PRODUCTION CONSIDERATIONS
```

The goal is not to memorize API terminology.

The goal is to understand how APIs are:

```text
Designed
Built
Consumed
Secured
Tested
Documented
Versioned
Scaled
Monitored
Troubleshot
```
