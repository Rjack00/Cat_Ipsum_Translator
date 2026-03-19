# Bash I/O & Redirection Notes

## Core idea
Every command uses 3 streams:
- stdin (0)  → input
- stdout (1) → normal output
- stderr (2) → errors

---

## Input (stdin)
- default: keyboard
- can redirect from file:

```bash
command < file.txt
```

---

## Output (stdout)

* default: terminal

overwrite file:

```bash
command > file.txt
```

append to file:

```bash
command >> file.txt
```

---

## Errors (stderr)

* separate from normal output

redirect errors:

```bash
command 2> error.txt
```

---

## stdout vs stderr

```bash
command > out.txt      # stdout
command 2> err.txt     # stderr
```

both:

```bash
command > all.txt 2>&1
```

(2>&1 = send errors to same place as output)

---

## Pipes

send output of one command → input of another:

```bash
command1 | command2
```

example:

```bash
echo "hello" | grep "h"
```

---

## Useful patterns

ignore errors:

```bash
command 2> /dev/null
```

save + display:

```bash
command | tee file.txt
```

append everything:

```bash
command >> file.txt 2>&1
```

---

## File descriptors

* 0 = stdin
* 1 = stdout
* 2 = stderr

---

## Gotchas

overwrites file:

```bash
> file.txt   # wipes it
```

append instead:

```bash
>> file.txt
```

---

errors not saved here:

```bash
command > file.txt
```

---

order matters:

```bash
command 2>&1 > file.txt   # wrong
command > file.txt 2>&1   # correct
```

---

## Mental shortcuts

* `>`  → send output
* `>>` → append output
* `<`  → input from file
* `|`  → connect commands

---

## One-line summary

Control where input comes from and where output/errors go.

```
```
