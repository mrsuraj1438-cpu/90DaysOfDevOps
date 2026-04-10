# Day 16 – Shell Scripting Basics

## Task
Start your shell scripting journey — learn the fundamentals every script needs.

You will:
- Understand **shebang** (`#!/bin/bash`) and why it matters
- Work with **variables**, **echo**, and **read**
- Write basic **if-else** conditions

---

## Expected Output
- A markdown file: `day-16-shell-scripting.md`
- All scripts you write during the tasks

---

## Challenge Tasks

### Task 1: Your First Script
1. Create a file `hello.sh`
2. Add the shebang line `#!/bin/bash` at the top
3. Print `Hello, DevOps!` using `echo`
4. Make it executable and run it

```bash
chmod +x hello.sh
./hello.sh
```

**Document:** What happens if you remove the shebang line?
**Ans:**If I remove the shebang (#!/bin/bash), the script may still work, but not always. 
           Without the shebang, the system does not know which shell to use.
           So it may use a default shell (like sh), and some advanced bash commands may fail.
           Without shebang, bash hello.sh works, but ./hello.sh may fail or give errors.
     - The script can run in two ways:
     1. Explicit method: bash hello.sh
       - It works because we are directly using bash.
     2. Direct execution: ./hello.sh
       - It may fail or run with a different shell if the shebang is not present.

---

### Task 2: Variables
1. Create `variables.sh` with:
   - A variable for your `NAME`
   - A variable for your `ROLE` (e.g., "DevOps Engineer")
   - Print: `Hello, I am <NAME> and I am a <ROLE>`
2. Try using single quotes vs double quotes — what's the difference?
   - Single quotes ' ' do NOT allow variable expansion: Because single quotes treat everything as plain text.
   - Double quotes " " Variables work ($NAME, $ROLE): Because double quotes allow variables to expand (replace with value).

---

### Task 3: User Input with read
1. Create `greet.sh` that:
   - Asks the user for their name using `read`
   - Asks for their favourite tool
   - Prints: `Hello <name>, your favourite tool is <tool>`

---

### Task 4: If-Else Conditions
1. Create `check_number.sh` that:
   - Takes a number using `read`
   - Prints whether it is **positive**, **negative**, or **zero**

2. Create `file_check.sh` that:
   - Asks for a filename
   - Checks if the file **exists** using `-f`
   - Prints appropriate message

---

### Task 5: Combine It All
Create `server_check.sh` that:
1. Stores a service name in a variable (e.g., `nginx`, `sshd`)
2. Asks the user: "Do you want to check the status? (y/n)"
3. If `y` — runs `systemctl status <service>` and prints whether it's **active** or **not**
4. If `n` — prints "Skipped."

---

## Hints
- Shebang: `#!/bin/bash` tells the system which interpreter to use
- Variables: `NAME="Shubham"` (no spaces around `=`)
- Read: `read -p "Enter name: " NAME`
- If syntax: `if [ condition ]; then ... elif ... else ... fi`
- File check: `if [ -f filename ]; then`

---

## Documentation

Create `day-16-shell-scripting.md` with:
- Each script's code and output
- What you learned (3 key points)

---

## Submission
1. Add your scripts and `day-16-shell-scripting.md` to `2026/day-16/`
2. Commit and push to your fork

---

## Learn in Public

Share your first shell scripts on LinkedIn.

`#90DaysOfDevOps` `#DevOpsKaJosh` `#TrainWithShubham`

Happy Learning!
**TrainWithShubham**
