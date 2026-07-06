# Knowledge Base

## Project: ASCII Art Web Export

### HTTP Headers

HTTP headers provide metadata about an HTTP request or response.

Headers are sent before the response body.

The browser reads the headers first before deciding how to process the response body.

---

## Content-Type

Purpose:

Tells the browser what type of data the response body contains.

Examples:

* text/plain
* text/html
* application/json

Example:

```
Content-Type: text/plain
```

This tells the browser to interpret the body as plain text.

---

## Content-Disposition

Purpose:

Tells the browser how the response body should be handled.

Common values:

* inline
* attachment

Example:

```
Content-Disposition: attachment; filename="ascii-art.txt"
```

This instructs the browser to download the response instead of displaying it.

---

## Content-Length

Purpose:

Specifies the size of the response body in bytes.

Example:

```
Content-Length: 512
```

The browser uses this information to know when the download is complete.

---

## Query Parameters

Query parameters are values appended to the URL after a question mark (?).

Example:

```
/export?text=Hello&banner=shadow
```

They are used to transfer small amounts of information between requests.

In Go:

```go
text := r.URL.Query().Get("text")
banner := r.URL.Query().Get("banner")
```

---

## ResponseWriter

`http.ResponseWriter` is responsible for sending the HTTP response back to the client.

It is used to:

* Set HTTP headers
* Set the HTTP status code
* Write the response body

Example:

```go
w.Header().Set("Content-Type", "text/plain")
fmt.Fprint(w, generate)
```

---

## Template Rendering vs File Download

Rendering HTML:

```
Template
        ↓
HTML Page
        ↓
Browser
```

Downloading a File:

```
Generate ASCII
        ↓
Set Headers
        ↓
Write Text
        ↓
Browser Downloads File
```

These are two different kinds of HTTP responses.

---

## Request Independence

Each HTTP request is independent.

A handler does not remember local variables from previous requests.

To transfer information between requests, the browser must send it again.

In this project, the browser sends:

* text
* banner

through query parameters.

The export handler regenerates the ASCII art instead of trying to reuse previous variables.

---

## Engineering Principle

Do not send derived data.

Instead of sending the generated ASCII art through the URL, send only:

* Text
* Banner

The server can regenerate the ASCII art whenever necessary.

This reduces network traffic and keeps URLs small.




# Project Notes

## ASCII Art Web Export

### Objective

Allow users to export generated ASCII art as a downloadable text file.

---

## Request Flow

```
Browser

POST /ascii-art
        ↓
AsciiHandler
        ↓
Generate ASCII
        ↓
Render HTML
        ↓
Export Button

GET /export?text=Hello&banner=shadow
        ↓
ExportHandler
        ↓
Generate ASCII Again
        ↓
Set HTTP Headers
        ↓
Write Response
        ↓
Browser Downloads File
```

---

## Files Modified

* main.go
* handler.go
* templates/index.html

---

## New Features

* Export endpoint
* Query parameter handling
* File download
* HTTP response headers

---

## Engineering Decisions

* Pass original inputs instead of generated ASCII art.
* Reuse the ASCII generation logic.
* Use a struct to pass data to the template.
* Return plain text instead of rendering HTML.



# Project Notes

## ASCII Art Web Export

### Objective

Allow users to download generated ASCII art as a text file.

---

## Architecture

```
Browser

POST /ascii-art
        │
        ▼
AsciiHandler
        │
        ▼
prepareASCII()
        │
        ▼
Render Template

──────────────────────────

GET /export
        │
        ▼
ExportHandler
        │
        ▼
prepareASCII()
        │
        ▼
Set Headers
        │
        ▼
Write ASCII
        │
        ▼
Browser Downloads File
```

---

## Helper Function

Shared business logic:

```
prepareASCII(text, banner)
```

Responsibilities:

* Validate input
* Validate banner
* Build banner path
* Generate ASCII art
* Return generated result or error

Handlers remain responsible only for HTTP-related tasks.

---

## Engineering Decisions

* Query parameters are used to transfer the original inputs.
* The ASCII art is regenerated instead of being sent through the URL.
* Shared business logic is extracted into a helper.
* Errors are returned instead of printed.
* Export responses write directly to the response body without rendering templates.

