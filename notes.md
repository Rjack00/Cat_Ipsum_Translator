````md
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

## 🔹 Core Commands (used here)

### echo
```bash
echo "text"
echo -e "\ntext"
```
- `-e` → enable escapes

---

### read
```bash
read VAR
read VAR < file.txt
```
⚠️ pipe runs in subshell → variable won’t persist

---

### cat
```bash
cat file.txt
cat < file.txt
echo "hi" | cat
```

---

### wc (word count)
```bash
wc file.txt
```

flags:
```bash
wc -l   # lines
wc -w   # words
wc -m   # characters
```

---

### grep (search)
```bash
grep 'pattern' file.txt
```

flags:
```bash
--color   # highlight matches
-n        # line numbers
-c        # count matching lines
-o        # each match on its own line
-E        # extended regex
```

examples:
```bash
grep -n 'meow' file.txt
grep -o 'meow[a-z]*' file.txt
grep -E 'dog|woof'
```

count matches:
```bash
grep -o 'pattern' file.txt | wc -l
```

---

### sed (stream editor / replace)
```bash
sed 's/pattern/replacement/' file.txt
```

flags:
```bash
g   # global (all matches per line)
i   # ignore case
-E  # extended regex
```

examples:
```bash
sed 's/cat/dog/g'
sed -E 's/meow|meowzer/woof/g'
sed -E 's/([0-9]+).*/\1/'
```

multiple replacements:
```bash
sed 's/a/b/g; s/c/d/g'
```

---

### diff (compare files)
```bash
diff file1 file2
diff --color file1 file2
```

---

## 🔹 Regex (used here)

```bash
[a-z]*    # 0+ letters
[a-z]+    # 1+ letters
[0-9]+    # numbers
.*        # everything
a|b       # OR (needs -E)
```

---

## 🔹 Common patterns

count matches:
```bash
grep -o 'pattern' file | wc -l
```

get line numbers only:
```bash
grep -n 'pattern' file | sed -E 's/([0-9]+).*/\1/'
```

append text:
```bash
echo -e "\ntext" >> file.txt
```

---

## 🔹 Script basics

```bash
#!/bin/bash
cat "$1"
```

make executable:
```bash
chmod +x script.sh
```

run:
```bash
./script.sh file.txt
```

---

## 🔹 Translation script pattern

```bash
cat "$1" | sed -E '
s/catnip/dogchow/g;
s/cat/dog/g;
s/meow|meowzer/woof/g
'
```

test:
```bash
./script.sh file.txt | grep -E --color 'dog[a-z]*|woof[a-z]*'
```

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

pipe vs redirect:

```bash
grep pattern < script.sh      # reads file (NOT execution)
./script.sh | grep pattern   # correct
```

---

missing extended regex:

```bash
grep 'a|b'      # ❌
grep -E 'a|b'   # ✅
```

---

sed not global:

```bash
sed 's/a/b/'    # first match only
sed 's/a/b/g'   # all matches
```

---

subshell variable issue:

```bash
echo "RJ" | read NAME   # variable lost
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
````
