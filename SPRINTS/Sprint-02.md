# Sprint 02 – Building the Core of ASCII Art Web

**Date:** June 30, 2026

## Sprint Goal

Transform the architectural design into a working Go web application by implementing the server, routing, handlers, templates, and business logic integration.

---

# What Was Built

* Initialized the Go module.
* Created the project folder structure.
* Started the HTTP server.
* Registered application routes.
* Implemented `HomeHandler`.
* Implemented `AsciiHandler`.
* Connected HTML forms to backend handlers.
* Reused the ASCII generator from the previous CLI project.
* Successfully generated ASCII art through the web application.

---

# Major Bug Encountered

## Problem

The application failed to generate ASCII art and returned:

"Couldn't generate ascii-art"

## Root Cause

The `banner` directory was missing from the project, causing `os.ReadFile()` to fail when attempting to load the selected banner file.

## Solution

* Created the `banner/` directory.
* Added `standard.txt`, `shadow.txt`, and `thinkertoy.txt`.
* Verified the working directory.
* Confirmed the banner file path before loading.

---

# Engineering Lessons

* File paths are resolved relative to the program's working directory.
* Error messages should be investigated instead of ignored.
* Reusing business logic is better than rewriting working code.
* Verify one layer of the application before building the next.

---

# Improvements Made

* Added proper HTTP status codes.
* Improved validation.
* Fixed template rendering.
* Corrected HTML form issues.
* Improved variable naming (`bannerFile` → `bannerPath`).

---

# Reflection

Today I understood that debugging is a process of collecting evidence instead of making random changes.

A single log message can narrow a problem down to one specific line of code.

This project also reinforced the importance of separating HTTP handling from business logic.
