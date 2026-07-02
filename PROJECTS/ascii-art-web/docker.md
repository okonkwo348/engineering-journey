# Project Notes — Dockerize

## Project Goal

Containerize the ASCII Art Web application using Docker and prepare it for deployment.

---

## Skills Practiced

* Writing Dockerfiles
* Understanding Docker images and containers
* Docker layer caching
* Dependency optimization
* Multi-stage builds
* GitHub Actions CI
* Production image optimization

---

## Final Dockerfile Features

* Optimized dependency caching
* Multi-stage build
* Lightweight Alpine runtime
* Runtime asset copying
* Production-ready executable

---

## Runtime Assets Required

The application requires these folders at runtime:

* templates/
* banner/
* static/

These are not compiled into the Go executable and must be copied into the runtime image.

---

## Final Outcome

Successfully created a professional multi-stage Dockerfile while understanding the engineering reasoning behind every instruction instead of copying examples.
