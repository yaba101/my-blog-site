---
external: false
title: 'Status Code and Error'
description: 'When a client sends a request to a server, the server sends a response back to the client. This response includes two important pieces of information: a statusCode` and an Error.'
date: 2023-04-03
---

#

When a client sends a request to a server, the server sends a response back to the client. This response includes two important pieces of information: a `statusCode` and an `Error`.

- **`statusCode`**: This is a 3-digit number that indicates the status of the response. It is a standardized code that provides information about the success or failure of the request. Status codes are grouped into five classes: informational responses, successful responses, redirects, client errors, and server errors.

For example, if a client sends a request to a server to retrieve a web page that does not exist, the server will respond with a `404` status code, indicating that the requested resource was not found.

- **`Error`**: This is a message that provides more information about why the request failed. It can include details about the specific error that occurred, such as a missing resource, invalid input, or authentication failure. The error message is often included in the response body.

For example, if a client sends an invalid input to a server, the server might respond with a `400` status code (indicating a client error) and an error message in the response body explaining the specific problem with the input.

In summary, the `statusCode` is a standardized code that provides a high-level status of the response, while the `Error` provides more specific information about the reason for the failure. Both pieces of information are important for understanding the server's response to a client request.
