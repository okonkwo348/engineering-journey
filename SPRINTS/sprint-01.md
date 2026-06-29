# Sprint 01 – ASCII Art Web

**Date:** June 29–30, 2026

## Sprint Goal

Understand the architecture of a Go web application before writing any code.

Instead of jumping into implementation, the focus of this sprint is to understand how requests travel through a web application and how responsibilities should be separated between different components.

---

# Objectives

* Understand HTTP request/response lifecycle
* Understand the role of a server
* Understand routing
* Understand handlers
* Understand business logic
* Design the application architecture
* Identify the functions needed before implementation

---

# What I Learned

## 1. A Server Is More Than `ListenAndServe()`

A server is an application that:

* Listens for incoming requests.
* Routes requests to the correct handler.
* Executes business logic.
* Returns responses.

`ListenAndServe()` starts the server listening, but it is only one part of the server.

---

## 2. Request Lifecycle

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

Template

↓

Browser

---

## 3. Separation of Responsibilities

Every component should have a single responsibility.

### main.go

* Register routes
* Start the server

### Handler

* Receive HTTP requests
* Validate input
* Call business logic
* Return responses

### Business Logic

* Solve the application's problem
* Generate ASCII art
* Know nothing about HTTP

### Templates

* Display data
* Render HTML

---

## 4. Reusability

Business logic should never depend on HTTP.

This allows the same logic to be reused in:

* CLI applications
* REST APIs
* Desktop applications
* Mobile applications
* Robotics software

---

## 5. Engineering Principles Learned

* Separation of Concerns
* Single Responsibility Principle
* Low Coupling
* Reusability
* Architecture Before Implementation

---

# Reflection

Today I realized that software engineering is not about writing code first.

It is about designing systems with clear responsibilities.

Writing code becomes easier once the architecture is clear.

---

# Tomorrow

* Build the project structure.
* Implement the HTTP server.
* Register routes.
* Create the first handler.
