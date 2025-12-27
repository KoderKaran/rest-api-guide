## REST API Documentation Topics

This document outlines key topics essential for comprehensive REST API documentation, providing detailed explanations suitable for integration into markdown.

---

### 1. HTTP Methods

HTTP methods, also known as verbs, define the action to be performed on a resource. They are fundamental to RESTful communication.

*   **GET**:
    *   **Description**: Retrieves a representation of a resource. It should only retrieve data and have no other effect on the server.
    *   **Idempotency**: Yes. Making multiple identical GET requests should have the same effect as making a single request.
    *   **Usage**: Retrieving lists of resources, retrieving a specific resource by its identifier.
    *   **Example**: `GET /users` (retrieve all users), `GET /users/123` (retrieve user with ID 123).

*   **POST**:
    *   **Description**: Submits data to be processed to a specified resource. This typically results in a change in state or side effects on the server. It's often used for creating new resources.
    *   **Idempotency**: No. Repeated POST requests may create multiple new resources or perform the action multiple times.
    *   **Usage**: Creating new resources, submitting data for processing (e.g., form submissions), triggering actions.
    *   **Example**: `POST /users` (create a new user), `POST /orders` (place a new order).

*   **PUT**:
    *   **Description**: Replaces the entire current representation of a target resource with the request payload. If the resource does not exist, it may create it.
    *   **Idempotency**: Yes. Making multiple identical PUT requests will result in the same final state of the resource.
    *   **Usage**: Updating an existing resource completely, creating a resource if it doesn't exist at a specific URI.
    *   **Example**: `PUT /users/123` (update user with ID 123, replacing all fields), `PUT /products/abc` (create product with ID 'abc' if it doesn't exist).

*   **PATCH**:
    *   **Description**: Applies partial modifications to a resource. It's used to update specific fields of a resource without sending the entire representation.
    *   **Idempotency**: No (though it can be designed to be idempotent in specific implementations). A PATCH request might be applied multiple times with different partial updates, leading to different outcomes.
    *   **Usage**: Updating specific fields of a resource.
    *   **Example**: `PATCH /users/123` (update only the email address of user 123).

*   **DELETE**:
    *   **Description**: Deletes the specified resource.
    *   **Idempotency**: Yes. Making multiple DELETE requests for the same resource will result in the same state (the resource being deleted). Subsequent requests might return a 404 Not Found.
    *   **Usage**: Removing a resource.
    *   **Example**: `DELETE /users/123` (delete user with ID 123).

---

### 2. Request/Response Formats

The format of data exchanged between the client and server. JSON and XML are the most common.

*   **JSON (JavaScript Object Notation)**:
    *   **Description**: A lightweight data-interchange format that is easy for humans to read and write and easy for machines to parse and generate. It's based on a subset of the JavaScript programming language.
    *   **Key Features**:
        *   Uses key-value pairs.
        *   Supports basic data types: strings, numbers, booleans, null, objects, and arrays.
        *   Widely adopted, especially for web APIs.
    *   **Content-Type Header**: `application/json`
    *   **Example Request Body**:
        ```json
        {
          "name": "John Doe",
          "email": "john.doe@example.com",
          "age": 30
        }
        ```
    *   **Example Response Body**:
        ```json
        {
          "id": "user-123",
          "name": "John Doe",
          "email": "john.doe@example.com",
          "age": 30,
          "createdAt": "2023-10-27T10:00:00Z"
        }
        ```

*   **XML (eXtensible Markup Language)**:
    *   **Description**: A markup language that defines a set of rules for encoding documents in a format that is both human-readable and machine-readable. It's more verbose than JSON.
    *   **Key Features**:
        *   Uses tags to define elements and attributes.
        *   Can represent complex data structures.
        *   Used in older systems and some enterprise applications.
    *   **Content-Type Header**: `application/xml` or `text/xml`
    *   **Example Request Body**:
        ```xml
        <user>
          <name>Jane Doe</name>
          <email>jane.doe@example.com</email>
          <age>25</age>
        </user>
        ```
    *   **Example Response Body**:
        ```xml
        <user id="user-456">
          <name>Jane Doe</name>
          <email>jane.doe@example.com</email>
          <age>25</age>
          <createdAt>2023-10-27T10:05:00Z</createdAt>
        </user>
        ```

---

### 3. Status Codes

HTTP status codes are three-digit numbers that indicate the outcome of an HTTP request. They are categorized into five classes.

*   **1xx Informational**:
    *   **Description**: The request was received and understood. The client should continue with the request.
    *   **Common Codes**:
        *   `100 Continue`: The server has received the request headers and the client should proceed to send the request body.

*   **2xx Success**:
    *   **Description**: The request was successfully received, understood, and accepted.
    *   **Common Codes**:
        *   `200 OK`: The request has succeeded.
        *   `201 Created`: The request has succeeded and one or more new resources have been created as a result. Typically returned after a POST request.
        *   `204 No Content`: The server successfully processed the request, but is not returning any content. Often used for DELETE or PUT requests where no response body is needed.

*   **3xx Redirection**:
    *   **Description**: Further action needs to be taken by the client to complete the request.
    *   **Common Codes**:
        *   `301 Moved Permanently`: The requested resource has been permanently moved to a new URL.
        *   `302 Found` (or `Moved Temporarily`): The requested resource has been temporarily moved to a new URL.
        *   `304 Not Modified`: Used for caching purposes. Indicates that the requested resource has not been modified since the last requested.

*   **4xx Client Error**:
    *   **Description**: The request contains bad syntax or cannot be fulfilled. The client is responsible for the error.
    *   **Common Codes**:
        *   `400 Bad Request`: The server cannot or will not process the request due to something that is perceived to be a client error (e.g., malformed request syntax).
        *   `401 Unauthorized`: The request requires user authentication. The client needs to authenticate itself to get the requested response.
        *   `403 Forbidden`: The client does not have access rights to the content; that is, it is unauthorized, so the server is refusing to give the requested information. Unlike `401`, the client's identity is known.
        *   `404 Not Found`: The server cannot find the requested resource.
        *   `405 Method Not Allowed`: The request method is known by the server but is not supported by the target resource.
        *   `409 Conflict`: The request could not be completed due to a conflict with the current state of the target resource.
        *   `429 Too Many Requests`: The user has sent too many requests in a given amount of time ("rate limiting").

*   **5xx Server Error**:
    *   **Description**: The server failed to fulfill an apparently valid request. The server is responsible for the error.
    *   **Common Codes**:
        *   `500 Internal Server Error`: A generic error message, given when an unexpected condition was encountered and no more specific message is suitable.
        *   `503 Service Unavailable`: The server is not ready to handle the request. This may be because the server is down for maintenance or overloaded.

---

### 4. Authentication Methods

Mechanisms used to verify the identity of a client making requests to the API.

*   **API Keys**:
    *   **Description**: A simple token that is passed in the request to identify the calling application. It's often included in the request header or as a query parameter.
    *   **Pros**: Simple to implement and use.
    *   **Cons**: Less secure if not handled properly (can be leaked). Not suitable for user-specific authentication.
    *   **Example Header**: `X-API-Key: YOUR_API_KEY`
    *   **Example Query Parameter**: `?api_key=YOUR_API_KEY`

*   **OAuth 2.0**:
    *   **Description**: An authorization framework that enables applications to obtain limited access to user accounts on an HTTP service, such as Facebook or Twitter. It allows users to grant third-party applications access to their data without sharing their credentials.
    *   **Key Concepts**:
        *   **Access Token**: A credential representing the authorization granted.
        *   **Refresh Token**: Used to obtain a new access token when the current one expires.
        *   **Scopes**: Define the specific permissions the client application is requesting.
        *   **Grant Types**: Different flows for obtaining tokens (e.g., Authorization Code, Client Credentials, Implicit, Password Credentials).
    *   **Usage**: User authentication and authorization for third-party applications.

*   **JWT (JSON Web Tokens)**:
    *   **Description**: A compact, URL-safe means of representing claims to be transferred between two parties. The claims are encoded as a JSON object, which is then signed and/or encrypted.
    *   **Structure**: Consists of three parts separated by dots: Header, Payload, Signature.
        *   **Header**: Contains metadata about the token (e.g., algorithm used).
        *   **Payload**: Contains the claims (e.g., user ID, roles, expiration time).
        *   **Signature**: Used to verify that the sender of the JWT is who it says it is and to ensure that the message was not changed along the way.
    *   **Usage**: Often used in conjunction with OAuth 2.0 or as a stateless authentication mechanism. The server issues a JWT to the client after successful authentication, and the client includes this token in subsequent requests.
    *   **Example Header**: `Authorization: Bearer YOUR_JWT_TOKEN`

---

### 5. Error Handling Strategies

How an API communicates errors to its clients. Effective error handling is crucial for developer experience and API usability.

*   **Consistent Error Response Structure**:
    *   **Description**: Define a standardized format for error messages that clients can parse reliably. This typically includes an error code, a human-readable message, and potentially additional details or links to documentation.
    *   **Example Error Response (JSON)**:
        ```json
        {
          "error": {
            "code": "INVALID_INPUT",
            "message": "The provided 'email' field is not a valid email address.",
            "details": {
              "field": "email",
              "received_value": "invalid-email"
            },
            "documentation_url": "https://api.example.com/docs/errors#INVALID_INPUT"
          }
        }
        ```

*   **Appropriate HTTP Status Codes**:
    *   **Description**: Use HTTP status codes accurately to indicate the type of error (client-side vs. server-side) and the specific problem.
    *   **Examples**: `400 Bad Request` for invalid input, `401 Unauthorized` for authentication issues, `404 Not Found` for missing resources, `500 Internal Server Error` for unexpected server issues.

*   **Idempotent Error Handling**:
    *   **Description**: Ensure that retrying a request that resulted in an error does not cause unintended side effects, especially for non-idempotent operations.

*   **Logging and Monitoring**:
    *   **Description**: Implement robust logging on the server-side to capture detailed information about errors, which aids in debugging and troubleshooting. Monitor error rates to identify systemic issues.

*   **Informative Error Messages**:
    *   **Description**: Error messages should be clear and actionable, guiding the developer on how to resolve the issue. Avoid overly technical jargon where possible.

---

### 6. API Design Best Practices

Principles and guidelines for creating well-designed, maintainable, and user-friendly REST APIs.

*   **Resource-Based Design**:
    *   **Description**: APIs should be designed around resources (nouns) rather than actions (verbs). Use HTTP methods to operate on these resources.
    *   **Example**: Instead of `/getUserById`, use `GET /users/{id}`.

*   **Use Plural Nouns for Collections**:
    *   **Description**: Represent collections of resources using plural nouns.
    *   **Example**: `/users`, `/products`, `/orders`.

*   **Use HTTP Methods Correctly**:
    *   **Description**: Adhere to the standard meanings of HTTP methods (GET, POST, PUT, PATCH, DELETE) for their intended operations.

*   **Consistent Naming Conventions**:
    *   **Description**: Use consistent naming for resources, fields, and parameters (e.g., camelCase, snake_case).
    *   **Example**: `userId` or `user_id`, `firstName` or `first_name`.

*   **Statelessness**:
    *   **Description**: Each request from a client to the server must contain all the information necessary to understand and complete the request. The server should not store any client context between requests.

*   **Use HATEOAS (Hypermedia as the Engine of Application State)**:
    *   **Description**: Include links in API responses that guide the client on the next possible actions or related resources. This makes the API more discoverable and less coupled to specific URIs.

*   **Support Filtering, Sorting, and Pagination**:
    *   **Description**: Allow clients to efficiently retrieve only the data they need by supporting these features.

*   **Security**:
    *   **Description**: Implement appropriate authentication and authorization mechanisms. Use HTTPS to encrypt communication.

---

### 7. Versioning Strategies

Methods for managing changes to an API while maintaining backward compatibility for existing clients.

*   **URI Versioning**:
    *   **Description**: Include the version number directly in the URI path.
    *   **Pros**: Explicit and easy to understand.
    *   **Cons**: Pollutes the URI space. Can be cumbersome to manage.
    *   **Example**:
        *   `https://api.example.com/v1/users`
        *   `https://api.example.com/v2/users`

*   **Header Versioning**:
    *   **Description**: Specify the API version in a custom HTTP header.
    *   **Pros**: Keeps URIs clean.
    *   **Cons**: Less discoverable than URI versioning. Requires clients to manually set headers.
    *   **Example Header**: `Accept-Version: v1` or `X-API-Version: 1.0.0`
    *   **Example Request**: `GET /users` with `Accept-Version: v1` header.

*   **Query Parameter Versioning**:
    *   **Description**: Include the version number as a query parameter in the URL.
    *   **Pros**: Simple to implement and test.
    *   **Cons**: Can clutter URLs. May not be as semantically clear as URI versioning.
    *   **Example**:
        *   `https://api.example.com/users?version=1`
        *   `https://api.example.com/users?version=2`

*   **Content Negotiation (Accept Header)**:
    *   **Description**: Use the `Accept` header to specify the desired version of the resource. This is a more RESTful approach but can be complex to implement.
    *   **Example**: `Accept: application/vnd.example.v1+json`

---

### 8. Rate Limiting Techniques

Mechanisms to control the number of requests a client can make within a specific time period to prevent abuse and ensure fair usage.

*   **Token Bucket Algorithm**:
    *   **Description**: A bucket holds tokens. Tokens are added to the bucket at a fixed rate. Each request consumes a token. If the bucket is empty, the request is rejected or queued.
    *   **Parameters**: Bucket size (maximum burst), refill rate.

*   **Leaky Bucket Algorithm**:
    *   **Description**: Requests are added to a bucket. The bucket "leaks" requests at a fixed rate. If the bucket is full, new requests are rejected.
    *   **Parameters**: Bucket size, leak rate.

*   **Fixed Window Counter**:
    *   **Description**: A counter tracks requests within a fixed time window (e.g., 60 seconds). When the window resets, the counter is reset.
    *   **Issue**: Can allow bursts at the beginning and end of consecutive windows.

*   **Sliding Window Log**:
    *   **Description**: Keeps a log of timestamps for each request. When a new request comes in, it counts the number of requests within the current time window (e.g., the last 60 seconds).
    *   **Pros**: More accurate than fixed window.
    *   **Cons**: Higher memory usage.

*   **Sliding Window Counter**:
    *   **Description**: A hybrid approach that uses a fixed window counter but also considers the previous window's activity to provide a smoother rate limit.

*   **Implementation**:
    *   **Headers**: Typically communicated to the client via response headers like `X-RateLimit-Limit`, `X-RateLimit-Remaining`, `X-RateLimit-Reset` (timestamp when the limit resets).
    *   **Response Codes**: `429 Too Many Requests` is used for rate-limited responses.

---

### 9. Pagination Methods

Techniques for dividing large datasets into smaller, manageable chunks for API responses.

*   **Offset/Limit (or Page Size)**:
    *   **Description**: The client specifies an `offset` (number of items to skip from the beginning) and a `limit` (maximum number of items to return per page).
    *   **Parameters**: `offset`, `limit` (or `page`, `pageSize`).
    *   **Example**: `GET /users?offset=20&limit=10` (returns users 21-30).
    *   **Pros**: Simple to implement and understand. Easy for clients to navigate.
    *   **Cons**: Can be inefficient for very large datasets as the server still needs to count and skip many records. Can lead to inconsistent results if data is modified between requests (e.g., items can be missed or duplicated).

*   **Cursor-Based Pagination**:
    *   **Description**: The client receives a "cursor" (an opaque identifier, often the ID or a timestamp of the last item on the previous page) and uses it in the next request to fetch the next set of items.
    *   **Parameters**: `limit`, `after` (or `before`, `next_cursor`).
    *   **Example**: `GET /users?limit=10&after=user_id_abc` (returns users after `user_id_abc`).
    *   **Pros**: More efficient for large datasets as it avoids counting. More stable when data is being added or deleted, as it's based on the position relative to a specific item.
    *   **Cons**: Less intuitive for clients to navigate directly to a specific page number. Requires stable cursors.

*   **Keyset Pagination**:
    *   **Description**: Similar to cursor-based, but uses the actual values of fields used for sorting as the cursor.
    *   **Example**: `GET /users?limit=10&sort_by=createdAt&after_created_at=2023-10-27T10:00:00Z&after_id=user_id_xyz`
    *   **Pros**: Very efficient and stable.
    *   **Cons**: Can be complex to implement and for clients to manage, especially with multiple sort criteria.

*   **Response Structure**:
    *   Pagination results typically include the list of items for the current page, along with metadata for navigation (e.g., total count, links to next/previous pages, cursors).
    *   **Example Response (Offset/Limit)**:
        ```json
        {
          "data": [ ... items ... ],
          "meta": {
            "totalItems": 150,
            "itemsPerPage": 10,
            "currentPage": 3,
            "totalPages": 15,
            "nextPage": "/users?offset=30&limit=10",
            "previousPage": "/users?offset=10&limit=10"
          }
        }
        ```
    *   **Example Response (Cursor-Based)**:
        ```json
        {
          "data": [ ... items ... ],
          "paging": {
            "cursors": {
              "after": "cursor_for_next_page",
              "before": "cursor_for_previous_page"
            },
            "has_more": true
          }
        }
        ```