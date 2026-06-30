# Debugging Notes

## Rule 1

Never guess.

Collect evidence.

---

## Debugging Process

Observe the error.

↓

Read the log.

↓

Locate where the error originated.

↓

Inspect that component.

↓

Fix only the identified cause.

---

## Example

Error:

"Couldn't generate ascii-art"

Investigation:

The application log revealed:

"couldn't load file"

This immediately identified the problem as file loading rather than the ASCII generation algorithm.

The actual issue was a missing `banner/` directory.

---

## Engineering Principle

The first error message is usually more valuable than the last one shown to the user.

Always inspect logs before changing code.

---

## Useful Debugging Tools

* `log.Println()`
* `fmt.Println()`
* `os.Getwd()`
* `pwd`
* `ls`

These tools help verify assumptions before modifying the implementation.
