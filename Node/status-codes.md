## HTTP status code:

frequently used HTTP status codes with explanations:

**2xx: Success**

* **`200 OK`**: The standard response for successful HTTP requests.
* **`201 Created`**: The request has succeeded and a new resource has been created.
* **`204 No Content`**: The server successfully processed the request but is not returning any content.

**3xx: Redirection**

* **`301 Moved Permanently`**: The requested resource has been permanently moved to a new URL.
* **`302 Found`**: The requested resource has been temporarily moved to a different URL.
* **`304 Not Modified`**: Indicates that the client's cached version of the resource is still valid.

**4xx: Client Error**

* **`400 Bad Request`**: The server could not understand the request due to invalid syntax.
* **`401 Unauthorized`**: Authentication is required and has failed or has not yet been provided.
* **`403 Forbidden`**: The server understood the request but refuses to authorize it.
* **`404 Not Found`**: The server could not find the requested resource.
* **`405 Method Not Allowed`**: The HTTP method used in the request is not supported by the resource.
* **`429 Too Many Requests`**: The user has sent too many requests in a given amount of time (rate limiting).

**5xx: Server Error**

* **`500 Internal Server Error`**: A generic error occurred on the server.
* **`502 Bad Gateway`**: The server, acting as a gateway, received an invalid response from an upstream server.
* **`503 Service Unavailable`**: The server is temporarily unable to handle the request.
* **`504 Gateway Timeout`**: The server, acting as a gateway, did not receive a timely response from an upstream server.

## Detailed list

**1xx: Informational**

* **`100 Continue`**: Server received headers, client should send body.
* **`101 Switching Protocols`**: Server is switching to a different protocol.
* **`102 Processing`**: Server is processing the request, no response yet.
* **`103 Early Hints`**: Server is sending preliminary headers to help the client.

**2xx: Success**

* **`200 OK`**: Request succeeded, response contains the requested data.
* **`201 Created`**: Request succeeded, and a new resource was created.
* **`202 Accepted`**: Request accepted for processing, but not yet completed.
* **`203 Non-Authoritative Information`**: Response from a proxy, not the origin server.
* **`204 No Content`**: Request successful, but no response body to send.
* **`205 Reset Content`**: Request successful, client should reset the document view.
* **`206 Partial Content`**: Server fulfilled a partial GET request.
* **`207 Multi-Status`**: Multiple status codes for multiple sub-requests (WebDAV).
* **`208 Already Reported`**: Members of a binding already enumerated (WebDAV).
* **`226 IM Used`**: Server fulfilled a request for instance manipulations.

**3xx: Redirection**

* **`300 Multiple Choices`**: Multiple options for the requested resource are available.
* **`301 Moved Permanently`**: Requested resource has a new permanent URI.
* **`302 Found`**: Requested resource temporarily resides under a different URI.
* **`303 See Other`**: Response can be found under a different URI (use GET).
* **`304 Not Modified`**: Resource hasn't changed since the last request.
* **`307 Temporary Redirect`**: Requested resource temporarily under a different URI (don't change method).
* **`308 Permanent Redirect`**: Requested resource has a new permanent URI (don't change method).

**4xx: Client Error**

* **`400 Bad Request`**: Server cannot process the request due to client error.
* **`401 Unauthorized`**: Authentication is required and has failed or not yet been provided.
* **`402 Payment Required`**: Payment is needed to access the resource (reserved).
* **`403 Forbidden`**: Server refuses to authorize the request.
* **`404 Not Found`**: Server cannot find the requested resource.
* **`405 Method Not Allowed`**: Request method is not supported for this resource.
* **`406 Not Acceptable`**: Server cannot produce a response matching the client's Accept headers.
* **`407 Proxy Authentication Required`**: Client must authenticate with the proxy first.
* **`408 Request Timeout`**: Server timed out waiting for the client's request.
* **`409 Conflict`**: Request could not be completed due to a conflict.
* **`410 Gone`**: Requested resource is permanently unavailable.
* **`411 Length Required`**: Server requires a Content-Length header.
* **`412 Precondition Failed`**: One or more preconditions in the request headers were false.
* **`413 Payload Too Large`**: Request body is larger than the server can handle.
* **`414 URI Too Long`**: Request URI is too long for the server to process.
* **`415 Unsupported Media Type`**: Request entity has an unsupported format.
* **`416 Range Not Satisfiable`**: Requested byte range is invalid or unavailable.
* **`417 Expectation Failed`**: Server cannot meet the requirements of the Expect header.
* **`418 I'm a teapot`**: Server refuses to brew coffee (humorous).
* **`421 Misdirected Request`**: Request directed to a server that can't handle it.
* **`422 Unprocessable Entity`**: Request entity is syntactically correct but semantically invalid.
* **`423 Locked`**: Resource is locked (WebDAV).
* **`424 Failed Dependency`**: Request failed because a dependency failed (WebDAV).
* **`425 Too Early`**: Server unwilling to process a potentially replayed request.
* **`426 Upgrade Required`**: Client should switch to a different protocol.
* **`428 Precondition Required`**: Server requires conditional request headers.
* **`429 Too Many Requests`**: Client has sent too many requests.
* **`431 Request Header Fields Too Large`**: Request header fields are too large.
* **`451 Unavailable For Legal Reasons`**: Access to the resource is denied due to legal constraints.
* **`499 Client Closed Request`**: Client closed the connection before receiving a response.

**5xx: Server Error**

* **`500 Internal Server Error`**: Server encountered an unexpected error.
* **`501 Not Implemented`**: Server does not support the requested functionality.
* **`502 Bad Gateway`**: Server received an invalid response from an upstream server.
* **`503 Service Unavailable`**: Server is temporarily overloaded or under maintenance.
* **`504 Gateway Timeout`**: Server didn't receive a timely response from an upstream server.
* **`505 HTTP Version Not Supported`**: Server doesn't support the HTTP version used.
* **`506 Variant Also Negotiates`**: Internal server error due to misconfiguration.
* **`507 Insufficient Storage`**: Server lacks storage to complete the request (WebDAV).
* **`508 Loop Detected`**: Infinite loop detected while processing the request (WebDAV).
* **`510 Not Extended`**: Server needs further extensions to fulfill the request.
* **`511 Network Authentication Required`**: Client needs to authenticate to access the network.
* **`520 Web Server Returned an Unknown Error`**: Cloudflare-specific: origin server returned an invalid response.
* **`521 Web Server Is Down`**: Cloudflare-specific: origin server refused the connection.
* **`522 Connection Timed Out`**: Cloudflare-specific: connection to origin server timed out.
* **`523 Origin Is Unreachable`**: Cloudflare-specific: could not reach the origin server.
* **`524 A Timeout Occurred`**: Cloudflare-specific: origin server took too long to respond.
* **`525 SSL Handshake Failed`**: Cloudflare-specific: SSL handshake with origin failed.
* **`526 Invalid SSL Certificate`**: Cloudflare-specific: SSL certificate on origin is invalid.
