# Knowledge – Dockerize (Day 1)

## Objective

Learn how Docker packages an application so it can run consistently on any computer.

---

# 1. What is Docker?

Docker is a platform that packages an application together with everything it needs to run.

Benefits:

- Consistent environments
- Easy deployment
- Isolation
- Portability

---

# 2. Docker Concepts

## Image

A blueprint for creating containers.

Example:

golang:1.24

An image is read-only.

---

## Container

A running instance of an image.

Relationship:

Image
↓

Container

---

# 3. Dockerfile

A Dockerfile is a text file containing instructions for Docker to build an image.

Example:

FROM
WORKDIR
COPY
RUN
EXPOSE
CMD

Docker executes these instructions from top to bottom.

---

# 4. FROM

Syntax

FROM golang:1.24

Purpose

Selects the base image.

Every Dockerfile must begin with a FROM instruction.

---

# 5. WORKDIR

Syntax

WORKDIR /app

Equivalent to:

cd /app

All following commands execute inside this directory.

---

# 6. COPY

Syntax

COPY source destination

Examples

COPY go.mod ./

COPY . .

Purpose

Copies files from the host machine into the container.

---

# 7. RUN

Syntax

RUN go mod download

Purpose

Executes commands while building the image.

RUN creates a new Docker layer.

---

# 8. CMD

Syntax

CMD ["go","run","."]

Purpose

Specifies the default command executed when a container starts.

Only one CMD should exist.

---

# 9. EXPOSE

Syntax

EXPOSE 8080

Purpose

Documents which port the application listens on.

Important:

EXPOSE does NOT publish the port.

It is metadata.

---

# 10. Docker Layers

Each Docker instruction creates a layer.

Example

FROM

↓

WORKDIR

↓

COPY

↓

RUN

↓

COPY

↓

CMD

Layers allow Docker to cache work and speed up future builds.

---

# 11. Docker Cache

Docker checks every instruction.

If nothing changes,
it reuses the cached layer.

Rule

If one layer changes,

↓

Every layer after it must rebuild.

---

# 12. Optimizing Docker Cache

Preferred

COPY go.mod ./

RUN go mod download

COPY . .

Reason

Changing source code does not force Docker to redownload dependencies.

---

# 13. .dockerignore

Purpose

Prevent unnecessary files from being copied into the image.

Example

.git

README.md

.gitignore

Benefits

- Smaller images
- Faster builds
- Cleaner containers

---

# 14. GitHub Actions

GitHub Actions is NOT Docker.

Purpose

Automates tasks such as:

- Testing
- Docker builds
- Deployment

Workflow file

.github/workflows/docker.yml

---

# 15. CI (Continuous Integration)

CI automatically verifies code whenever changes are pushed.

Example

Push code

↓

GitHub Action starts

↓

Docker image builds

↓

Build passes

---

# 16. Single-stage Build

Contains:

- Go compiler
- Source code
- Dependencies

Simple but larger.

---

# 17. Multi-stage Build

Uses one stage to build.

Uses another stage to run.

Benefits

- Smaller images
- Better security
- Faster deployment

---

# Engineering Principles

✔ Docker does not understand Go.

Docker only compares files and instructions.

✔ Docker caches layers.

✔ Arrange Dockerfile to maximize cache usage.

✔ Build environment and runtime environment can be separated.



# Personal Notes

Things I now understand

✔ Docker is not a virtual machine.

✔ Docker packages applications.

✔ Images are blueprints.

✔ Containers are running images.

✔ Docker layers improve performance.

✔ Docker cache depends on file changes.

✔ GitHub Actions is separate from Docker.

✔ docker.yml belongs to GitHub Actions.

Important realization

The order of Dockerfile instructions affects build performance.

One sentence to remember

"Optimize the Dockerfile so that expensive operations happen before frequently changing files are copied."

Next Topic

Multi-stage Docker builds





# Docker Knowledge Base

**Date:** July 2, 2026

---

# Core Concepts

## Docker

Docker is a platform that packages an application and everything it needs into an image. The image can then be executed as one or more containers, ensuring the application behaves consistently across different environments.

---

## Image

An image is a read-only blueprint containing:

* Operating system
* Application
* Dependencies
* Configuration
* Runtime instructions

Images are used to create containers.

---

## Container

A container is a running instance of an image.

One image can create multiple independent containers.

---

# Docker Lifecycle

```
Source Code
      ↓
docker build
      ↓
Docker Image
      ↓
docker run
      ↓
Container
```

---

# Dockerfile

A Dockerfile is a list of instructions Docker follows to build an image.

Common instructions:

* FROM
* WORKDIR
* COPY
* RUN
* EXPOSE
* CMD

---

# Build Time vs Runtime

## Build Time

Occurs during:

```
docker build
```

Examples:

* Download dependencies
* Install packages
* Compile application

---

## Runtime

Occurs during:

```
docker run
```

Examples:

* Start application
* Serve requests
* Read templates
* Read configuration files

---

# RUN vs CMD

RUN executes during image creation.

CMD executes every time a container starts.

RUN prepares the image.

CMD starts the application.

---

# Docker Layers

Every Docker instruction creates a new layer.

Docker caches completed layers.

If an instruction changes, Docker rebuilds that layer and every layer after it.

---

# Docker Cache

Docker compares the input of each instruction.

If the input has not changed, Docker reuses the cached layer instead of rebuilding it.

Efficient Dockerfiles maximize cache reuse.

---

# Optimized Dependency Download

Instead of:

```
COPY . .

RUN go mod download
```

Use:

```
COPY go.mod ./

RUN go mod download

COPY . .
```

This prevents unnecessary dependency downloads whenever only source code changes.

---

# Multi-Stage Builds

A multi-stage build separates:

## Builder Stage

Contains:

* Go compiler
* Dependencies
* Source code

Produces:

* Executable

---

## Runtime Stage

Contains only:

* Executable
* Templates
* Banner files
* Static assets

The Go compiler is left behind.

---

# AS builder

```
FROM golang:1.24 AS builder
```

Names the first build stage so later stages can copy files from it.

---

# COPY --from

Example:

```
COPY --from=builder /app/ascii-art-web .
```

Meaning:

Copy the executable from the stage named **builder** into the current stage.

---

# Runtime Assets

Go source code becomes part of the executable.

These remain external files:

* templates/
* banner/
* static/

They must be copied into the runtime image because the application reads them while running.

---

# Alpine Linux

Alpine is a minimal Linux distribution commonly used for production containers.

Benefits:

* Small image size
* Faster downloads
* Lower storage usage
* Smaller attack surface

---

# Engineering Principles Learned

* Optimize Docker layer caching.
* Separate build and runtime environments.
* Keep production images minimal.
* Copy only runtime requirements into the final image.
* Understand why a solution works instead of memorizing commands.


# Additional Knowledge — Build Time, Runtime & Multi-Stage Builds

## Build Time

Build time begins when Docker executes:

```bash
docker build -t app .
```

During build time, Docker reads the Dockerfile from top to bottom and executes every instruction required to create an image.

Instructions processed during build time include:

* FROM
* WORKDIR
* COPY
* RUN
* EXPOSE (stored as image metadata)

At build time, the application is **not running**. Docker is only assembling the image.

---

## Runtime

Runtime begins when Docker executes:

```bash
docker run app
```

Docker creates a container from the previously built image and starts the application.

During runtime, the `CMD` instruction is executed.

Example:

```dockerfile
CMD ["./ascii-art-web"]
```

This starts the Go application, which then begins listening for HTTP requests.

---

## Build Time vs Runtime

| Build Time                   | Runtime                          |
| ---------------------------- | -------------------------------- |
| Creates an image             | Creates a container              |
| Executed by `docker build`   | Executed by `docker run`         |
| Downloads dependencies       | Starts the application           |
| Compiles the executable      | Serves HTTP requests             |
| Copies project files         | Reads templates and banner files |
| Does not run the application | Application is running           |

---

## Multi-Stage Build

A multi-stage build separates building the application from running the application.

### Stage 1 — Builder

Purpose:

* Download dependencies
* Compile the application
* Produce the executable

Uses:

```dockerfile
FROM golang:1.24 AS builder
```

This stage is temporary and exists only during image creation.

---

### Stage 2 — Runtime

Purpose:

* Contain only what is required to run the application.

Uses:

```dockerfile
FROM alpine
```

Docker starts a completely new stage from Alpine Linux.

The previous stage is **not** continued or merged.

Only explicitly copied files are transferred using:

```dockerfile
COPY --from=builder ...
```

---

## COPY --from

Example:

```dockerfile
COPY --from=builder /app/ascii-art-web .
```

Meaning:

* Go to the stage named `builder`.
* Find `/app/ascii-art-web`.
* Copy it into the current working directory of the current stage.

---

## Builder Stage vs Final Image

The builder stage is **not** the final image.

It is a temporary build environment containing:

* Go compiler
* Go tools
* Source code
* Dependencies

Its purpose is to produce the executable.

The final image is an Alpine-based image containing only:

* Executable
* templates/
* banner/
* static/

The Go compiler, source code, and build tools are left behind.

---

## Runtime Assets

Compiled Go code becomes part of the executable.

External resources remain separate files.

Examples:

* templates/
* banner/
* static/
* configuration files
* images

These must exist inside the runtime container because the application reads them while running.

---

## Engineering Principles Learned

* Separate build environments from runtime environments.
* Keep production images as small as possible.
* Copy only what the application needs at runtime.
* Docker stages are independent.
* Files move between stages only through `COPY --from`.
* Think in terms of build architecture rather than Docker commands.
