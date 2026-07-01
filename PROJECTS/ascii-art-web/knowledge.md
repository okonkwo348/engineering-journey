# Knowledge – ASCII Art Web Stylize (Day 4)

## Goal
Learn how to improve a web application's appearance using CSS while keeping the backend logic unchanged.

---

# 1. Separation of Concerns

The business logic should remain unchanged.

Changes for Stylize are limited to:
- static/
- templates/

The backend continues handling:
- Routing
- Validation
- ASCII generation
- HTTP responses

---

# 2. Static Files

Purpose:
Store frontend assets.

Example:

static/
    css/
        style.css
    images/

Serve using:

http.Handle("/static/",
    http.StripPrefix("/static/",
        http.FileServer(http.Dir("static"))))

---

# 3. CSS Box Model

Every HTML element consists of:

Content
Padding
Border
Margin

box-sizing: border-box;

ensures the width includes padding and border.

---

# 4. Responsive Design

Instead of:

width: 900px;

use

width: 90%;
max-width: 900px;

Benefits:
- Works on phones
- Works on tablets
- Works on desktops

---

# 5. Media Queries

Syntax

@media (max-width:768px){

}

Meaning:

Apply enclosed styles only when the screen width is 768px or less.

Equivalent Go logic:

if screenWidth <= 768 {
    // Apply styles
}

---

# 6. CSS Selectors

Element selector

h1

Descendant selector

header h1

Attribute selector

input[type="submit"]

More specific selectors prevent unintended styling.

---

# 7. Typography

font-family

Uses fallback fonts.

Example:

Arial, Helvetica, sans-serif

line-height

Improves readability.

font-size

Prefer rem over px for accessibility.

---

# 8. Forms

textarea

width:100%

resize:vertical

Allows users to expand vertically without breaking layout.

---

# 9. User Experience

cursor:pointer

Shows an element is clickable.

transition

Creates smooth animations.

:hover

Changes appearance during mouse hover.

:focus

Highlights active input fields.

---

# 10. ASCII Output

Use

font-family: monospace;

Reason:

ASCII art depends on equal-width characters.

overflow-x:auto;

Adds horizontal scrolling only when necessary.

---

# 11. Design Principles

Every CSS property should answer one question:

What problem does this solve?

Examples

margin
→ spacing

padding
→ breathing room

border-radius
→ modern appearance

background
→ visual separation

media query
→ responsiveness

overflow
→ prevents layout breakage

---

# 12. Engineering Mindset

Never memorize CSS.

Instead ask:

Why does this property exist?

What happens if I remove it?
