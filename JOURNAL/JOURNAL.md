***This is the most valuable document.***

**Every Sunday we add something like:**

Week 1

What I learned

Mistakes

Wins

Questions

Next week's goals


**You'll literally have a book describing your growth.**

## June 30, 2026

Today was my first real backend development session in Delivery Mode.

I completed the core implementation of the `ascii-art-web` project, including the HTTP server, routing, handlers, template rendering, form handling, and integration of the ASCII generator from my previous CLI project.

The biggest challenge was debugging why the banner files could not be loaded. Instead of rewriting code, I learned to follow the error messages, inspect the working directory, and verify the application's environment. This reinforced the idea that debugging is about gathering evidence, not guessing.

Another important lesson was understanding the difference between client errors (4xx) and server errors (5xx). Not every error should return a 500 status code.

Today's work gave me confidence that I can build and debug a complete Go web application while keeping the business logic separate from the HTTP layer.


## 2/7/2026

# Learning Journey

Today marks the completion of my first Docker project.

Lessons learned:

- Docker packages applications.
- Docker builds images from Dockerfiles.
- Every instruction creates a layer.
- Layer order affects build speed.
- Docker caches previous work.
- GitHub Actions can build Docker images remotely.
- Docker and GitHub Actions are different technologies.

Major milestone:

I successfully built my Docker image through GitHub Actions without Docker installed locally.

Reflection

Today I moved beyond writing Go programs.

I learned how professional software engineers package and automate applications for deployment.

Next destination

ASCII Art Export File

Long-term roadmap

Go Backend

↓

Docker

↓

Deployment

↓

Groupie Tracker

↓

REST APIs

↓

Databases

↓

Concurrency

↓

AI Engineering


# Journey Update

Today's lesson completely changed how I view Docker.

Previously, I thought Docker was mainly about running applications inside containers. I now understand that Docker first builds an image and only later runs a container from that image.

The biggest breakthrough was distinguishing between build time and runtime. During the build, Docker creates the image, downloads dependencies, compiles the application, and prepares everything required for execution. The application itself does not run until a container is created.

I also learned that multi-stage builds are an engineering design pattern rather than a Docker trick. The builder stage is simply a temporary workshop used to compile the application, while the runtime stage is a clean production environment containing only what is necessary to execute the program.

Another important realization was the difference between compiled code and runtime assets. Go source files become part of the executable, but templates, banner files, and static assets remain external and must be copied into the final image.

The most valuable lesson from today was learning to think about software systems in terms of build environments and runtime environments instead of focusing only on Docker commands.

> Great software engineering is about separating responsibilities. Build environments create software; runtime environments execute software.
