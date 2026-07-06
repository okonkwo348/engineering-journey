# Questions & Answers

## Why doesn't the export handler have access to variables from AsciiHandler?

Because each HTTP request is independent.

When AsciiHandler finishes executing, all of its local variables are destroyed.

The browser sends the required information again using query parameters.

---

## Why send text and banner instead of the generated ASCII art?

Because the ASCII art is derived data.

The server already knows how to regenerate it.

Sending only the original inputs makes the URL shorter, cleaner, and easier to maintain.

---

## Why use a struct for the template?

The template requires multiple values:

* Result
* Text
* Banner

A struct groups related data together and is the idiomatic Go approach.

---

## Why doesn't ExportHandler execute a template?

Because the goal is not to render an HTML page.

The goal is to return a downloadable file.

---

## Why is fmt.Fprint used?

`fmt.Fprint` writes the generated ASCII art directly into the HTTP response body through `http.ResponseWriter`.

---

## What is the purpose of each required header?

### Content-Type

Describes the type of the response body.

### Content-Disposition

Instructs the browser to download the response as a file.

### Content-Length

Specifies the size of the response body in bytes.



## 2/7/2026

# Engineering Thinking Questions

## Question 1

Why is this Dockerfile preferred?

COPY go.mod ./

RUN go mod download

COPY . .

instead of

COPY . .

RUN go mod download

Explain using Docker layers and caching.

---

## Question 2

What is the difference between:

Image

and

Container?

---

## Question 3

What is the difference between:

RUN

and

CMD

---

## Question 4

Why is EXPOSE optional?

If it is optional, why do many developers still include it?

---

## Question 5

Explain why Docker itself does not understand Go projects.

What does Docker actually compare while building images?

---

## Challenge

Design a production Dockerfile that builds quickly even if the source code changes every day.
