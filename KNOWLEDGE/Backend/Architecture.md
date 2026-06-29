# Backend Architecture Notes

## Fundamental Principle

A good application is divided into components.

Each component has one responsibility.

---

## Typical Web Architecture

Browser

↓

HTTP Server

↓

Router

↓

Handler

↓

Business Logic

↓

Response

↓

Browser

---

## Component Responsibilities

### Server

Responsibilities:

* Listen for requests.
* Accept connections.
* Return responses.

---

### Router

Responsibilities:

* Match URLs.
* Send requests to the correct handler.

Example:

GET /

↓

HomeHandler

POST /ascii-art

↓

AsciiHandler

---

### Handler

Responsibilities:

* Read requests.
* Validate user input.
* Call business logic.
* Return responses.

Handlers communicate with the outside world.

---

### Business Logic

Responsibilities:

* Solve the application's problem.
* Operate on data.
* Stay independent of HTTP.

Business logic should be reusable across multiple interfaces.

---

## Engineering Rule

Never mix HTTP code with business logic.

Instead of:

Handler

↓

Generate ASCII

↓

Write HTML

Keep the responsibilities separated.

---

## Goal

Design software where each component can be replaced without rewriting the entire application.
