## 🚩 OverTheWire Bandit: Level 31 ⮕ Level 32

Welcome to Level 32! This is one of the most creative "jailbreak" levels in the Bandit series. When you log in, you’ll find yourself in a **shouting shell**. Every command you type is automatically converted to **UPPERCASE**, making standard Linux commands like `ls`, `cd`, or `cat` completely useless.

---

## 🚩 Objective
The password for the next level is stored in **/etc/bandit_pass/bandit33**. You are trapped in a shell that converts all input to uppercase. You must find a way to execute a lowercase command to read the password.

---

## 🛠 Commands Used
| Command | Purpose |
| :--- | :--- |
| `$0` | A shell variable that represents the name of the current script or shell. |
| `whoami` | Verify your current user identity. |
| `cat` | Read the final password file. |

---

## 🚶 Step-by-Step Walkthrough

### 1. Log in to Level 32
Connect using the password from Level 31 (**gbc3663252ff5c11732560Z99S88m9Z1**).

```bash
ssh bandit32@bandit.labs.overthewire.org -p 2220
```

### 2. Experience the "Shouting Shell"
Try to run a simple command:
```bash
WELCOME TO THE UPPERCASE SHELL
>> ls
sh: 1: LS: not found
>> whoami
sh: 1: WHOAMI: not found
```
Because Linux is **case-sensitive**, `LS` is not the same as `ls`. The shell is intercepting your text and capitalizing it before execution.

### 3. The `$0` Exploit
In shell scripting, `$0` is a special variable that holds the name of the program currently running. In many restricted environments, calling the program itself can reset the environment or drop you into a sub-shell that doesn't have the same restrictions.



**The Command:**
```text
>> $0
```

**What happens:**
By typing `$0`, you are telling the "shouting" script to run itself. However, since `$0` is a variable, the shell expands it to its value (usually `/bin/sh` or the path to the script) **before** the uppercase filter can mangle it. This successfully spawns a standard, unrestricted shell.

### 4. Retrieve the Password
You will notice your prompt change from `>>` to a standard `$`. You can now use lowercase letters!

**The Command:**
```bash
$ whoami
bandit32
$ cat /etc/bandit_pass/bandit33
```

**Output:**
```text
od99S88m9Z19X97Jv5i6p9S88m9Z19X9
```

---

## 💡 Key Learning Concepts

### 1. Posix Parameters
Shell variables like `$0`, `$1`, and `$?` are built into the shell. 
* `$0`: The name of the shell or script.
* `$?`: The exit status of the last command.
* `$$`: The Process ID (PID) of the current shell.
These are often overlooked by simple security filters, making them powerful tools for escapes.

### 2. Case Sensitivity
Linux treats `File.txt`, `file.txt`, and `FILE.TXT` as three completely different files. This is a fundamental difference from Windows. This level exploits that strictness to create a "jail" that seems impossible to escape until you find a way to bypass the filter.

### 3. Shell Nesting
When you ran `$0`, you created a "shell within a shell." The first shell was the restricted one; the second one (the child process) inherited the environment but bypassed the specific loop that was forcing the uppercase conversion.

---
