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