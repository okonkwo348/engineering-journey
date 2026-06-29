# ASCII Art Web - Architecture

## Goal

Create a web application that converts text into ASCII art.

---

## Main Components

### HTTP Server

Responsibilities:

- Start the server
- Register routes
- Receive requests
- Return responses

---

### Handler

Responsibilities:

- Read form data
- Validate input
- Call the ASCII generator
- Render templates

---

### ASCII Generator

Responsibilities:

- Load banner files
- Convert text to ASCII
- Return the generated result

---

### Templates

Responsibilities:

- Display the form
- Display the generated ASCII art
- Display error messages

---

## Routes

GET /

Display the home page.

POST /ascii-art

Receive the submitted form and generate ASCII art.

---

## Request Flow

Browser

↓

GET /

↓

Home Page

↓

User enters text

↓

POST /ascii-art

↓

Handler

↓

Validation

↓

ASCII Generator

↓

Template

↓

Browser