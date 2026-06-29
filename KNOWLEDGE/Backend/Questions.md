# Backend Questions

## Q1. Is a server just `ListenAndServe()`?

No.

A server is the complete application responsible for:

* Listening for requests.
* Routing requests.
* Calling handlers.
* Returning responses.

`ListenAndServe()` only starts the server listening.

---

## Q2. What is a handler?

A handler is responsible for communicating with the outside world.

Responsibilities:

* Receive HTTP requests.
* Validate user input.
* Call business logic.
* Return HTTP responses.

Handlers should not contain business logic.

---

## Q3. What is business logic?

Business logic solves the application's core problem.

It operates on data rather than HTTP.

Example:

Input:

Text + Banner

↓

ASCII Generator

↓

ASCII Art

The same business logic can be reused by:

* CLI
* Web
* API
* Mobile
* Robotics software

---

## Q4. Should the handler know how ASCII generation works?

No.

The handler only coordinates the request.

The business logic performs the transformation.

---

## Q5. Why separate handlers from business logic?

To make the application reusable, maintainable, and easier to test.

Different interfaces can share the same business logic without duplication.

---

## Q6. What is Low Coupling?

Low coupling means components depend on each other as little as possible.

Example:

Handler

↓

Business Logic

Good.

Business Logic

↓

HTTP Request

Bad.

The business logic should not depend on HTTP.

---

## Q7. What is Separation of Concerns?

Each component should have one responsibility.

Server → Receive requests

Router → Match URLs

Handler → Coordinate

Business Logic → Solve the problem

Template → Display results
