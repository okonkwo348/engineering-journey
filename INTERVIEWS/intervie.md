# Engineering Thinking Questions – Dockerize

These questions are designed to test understanding rather than memorization. The goal is to explain *why* Docker behaves the way it does.

---

# Question 1

Consider these two Dockerfiles.

### Dockerfile A

```dockerfile
COPY . .

RUN go mod download
```

### Dockerfile B

```dockerfile
COPY go.mod ./

RUN go mod download

COPY . .
```

### Answer

Dockerfile B is preferred because it maximizes Docker's layer caching.

Docker builds an image layer by layer and caches each completed layer. By copying only `go.mod` first and then running `go mod download`, Docker creates a cache for the dependency installation.

If I later modify `main.go`, the `go.mod` file has not changed, so Docker reuses the cached `COPY go.mod` and `RUN go mod download` layers. Only the `COPY . .` layer and the layers after it are rebuilt.

This significantly reduces build time because Docker avoids downloading dependencies every time the application source code changes.

---

# Question 2

Explain the difference between an Image and a Container.

### Answer

Docker is a platform that packages an application and everything it needs into an image. When the image is executed, Docker creates a container.

An **Image** is a read-only blueprint that contains the application, dependencies, libraries, configuration, and everything required to run the application.

A **Container** is a running instance of an image.

### Analogy

Think of an image as a **recipe book**.

The recipe book contains all the instructions but does nothing by itself.

A container is the **prepared meal** created by following the recipe.

One recipe can produce many meals, just as one image can create many containers.

---

# Question 3

Both `RUN` and `CMD` execute commands.

Why are both needed?

### Answer

Although both execute commands, they are executed at different stages of Docker's lifecycle.

`RUN` executes commands during **image build time**.

Example:

```dockerfile
RUN go mod download
```

This downloads the application's dependencies while Docker is building the image. Once completed, the result becomes part of the image and is stored in Docker's cache.

`CMD` executes during **container run time**.

Example:

```dockerfile
CMD ["go", "run", "."]
```

This command starts the application whenever a new container is created from the image.

### Key Difference

* `RUN` executes while building the image.
* `CMD` executes every time a container starts.

Separating these responsibilities allows Docker to prepare the environment once and reuse it many times.

---

# Question 4

Why is `EXPOSE` optional?

If it is optional, why do many developers still include it?

### Answer

`EXPOSE` is optional because Docker does not require it to run a container.

Its primary purpose is documentation. It tells other developers and Docker-related tools which port the application is expected to listen on.

Including `EXPOSE` improves readability and communicates the intended network configuration of the container.

---

# Question 5

Docker does not understand Go projects.

What does Docker actually compare while building images?

### Answer

Docker does not understand programming languages, dependencies, or project structure.

Instead, Docker compares the **inputs to each instruction** in the Dockerfile.

If the files used by an instruction change, Docker invalidates that layer and rebuilds it along with every layer that follows.

This file-based comparison is what enables Docker's caching mechanism.

---

# Question 6

Suppose you have the following Dockerfile.

```dockerfile
FROM golang:1.24

WORKDIR /app

COPY . .

RUN go mod download

CMD ["go", "run", "."]
```

Every time you change one line in `main.go`, rebuilding the image takes a long time.

### Answer

The rebuild is slow because `COPY . .` copies the entire source code before downloading dependencies.

Whenever `main.go` changes, the `COPY . .` layer changes, causing Docker to invalidate its cache. Since `RUN go mod download` comes after that layer, Docker executes it again even though `go.mod` has not changed.

To fix this, reorder the Dockerfile:

```dockerfile
WORKDIR /app

COPY go.mod ./

RUN go mod download

COPY . .
```

This works because Docker reuses the cached `COPY go.mod` and `RUN go mod download` layers whenever `go.mod` remains unchanged.

If only the application source code changes, Docker rebuilds only the `COPY . .` layer and the layers after it, significantly reducing build time.

---

# Final Engineering Principle

> Docker is **file-aware**, not **language-aware**.

Docker does not know Go, Python, Java, or Node.js.

It simply executes instructions, compares file changes, and reuses cached layers whenever possible.

Understanding this principle allows engineers to design Dockerfiles that build efficiently and scale well in real-world projects.
