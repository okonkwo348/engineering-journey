# Backend Questions

## What is a server?

A server is an application that listens for requests, routes them to the correct handler, executes business logic, and returns a response.

---

## Is ListenAndServe the server?

No.

ListenAndServe starts the server listening for requests.

The server also includes routing, handlers, business logic, and response generation.

---

## Difference between a handler and business logic?

Handler:
- Deals with HTTP.

Business Logic:
- Solves the actual problem.