# Day 16 – Shell Scripting Basics

## Overview (What I learned today)

Today I learned the basics of Shell Scripting.
Shell scripting helps us run Linux commands in an automatic way. Instead of typing commands again and again, we can write a script and run it anytime.

---

## Why we use Shell Scripting

We use shell scripting because:
- It saves time
- It reduces manual work
- It automates repeated tasks
- It is very useful in DevOps and system administration

---

## How Shell Script Works

- We write commands in a file with `.sh` extension
- The system reads and runs line by line
- The `#!/bin/bash` tells which shell should run the script

---

## Task 1: First Script

### hello.sh

```bash
#!/bin/bash
echo "Hello, DevOps!"
```

### How to run

```bash
chmod +x hello.sh
./hello.sh
```

### Output

```
Hello, DevOps!
```

### What I learned

If we remove `#!/bin/bash`, the script may not run correctly because system may not know which shell to use.

---

## Task 2: Variables

### variables.sh

```bash
#!/bin/bash
NAME="Suraj"
ROLE="DevOps Engineer"
echo "Hello, I am $NAME and I am a $ROLE"
```

### Output

```
Hello, I am Suraj and I am a DevOps Engineer
```

### What I learned

- Variables store data
- Double quotes allow variable values
- Single quotes do not replace variables

---

## Task 3: User Input

### greet.sh

```bash
#!/bin/bash
echo "Enter your name:"
read name
echo "Enter your favourite tool:"
read tool
echo "Hello $name, your favourite tool is $tool"
```

### Output

```
Enter your name:
Suraj
Enter your favourite tool:
Docker
Hello Suraj, your favourite tool is Docker
```

### What I learned

We can take input from users using `read`.

---

## Task 4: If-Else Conditions

### check_number.sh

```bash
#!/bin/bash
echo "Enter a number:"
read num
if [ $num -gt 0 ]; then
    echo "Positive number"
elif [ $num -lt 0 ]; then
    echo "Negative number"
else
    echo "Zero"
fi
```

### file_check.sh

```bash
#!/bin/bash
echo "Enter filename:"
read file
if [ -f "$file" ]; then
    echo "File exists"
else
    echo "File does not exist"
fi
```

### What I learned

- `if-else` helps to make decisions in scripts
- We can check numbers and files

---

## Task 5: Server Check Script

### server_check.sh

```bash
#!/bin/bash
service="nginx"
echo "Do you want to check service status? (y/n)"
read choice
if [ "$choice" = "y" ]; then
    if systemctl is-active --quiet "$service"; then
        echo "$service is RUNNING"
    else
        echo "$service is NOT RUNNING"
    fi
else
    echo "Skipped"
fi
```

### What I learned

We can check system services using scripts and automate server monitoring.

---

## Final Learning (Why this is useful)

- Shell scripting is very useful in Linux and DevOps
- It helps automate daily tasks
- It reduces human error
- It makes work faster and easier
