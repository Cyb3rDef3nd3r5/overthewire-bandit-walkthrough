## 🚩 OverTheWire Bandit: Level 18 ⮕ Level 19

Level 19 introduces one of the most important concepts in Linux security: **SETUID (Set User ID)**. This is a special file permission that allows a user to run an executable with the permissions of the file's owner rather than the user's own permissions.

---

## 🚩 Objective
The password for the next level is stored in **/etc/bandit_pass/bandit20**. You are currently `bandit19`, and you do not have permission to read that file. However, there is a **setuid binary** in the home directory that can help you.

---

## 🛠 Commands Used
| Command | Purpose |
| :--- | :--- |
| `./bandit20-do` | The setuid binary provided in the home directory. |
| `ls -l` | View file permissions and identify the setuid bit. |
| `whoami` | Check which user is currently executing a command. |

---

## 🚶 Step-by-Step Walkthrough

### 1. Log in to Level 19
Connect using the password from Level 18 (**awhqfNnAbcbS1p9S88m9Z19X97Jv5i6p**).

```bash
ssh bandit19@bandit.labs.overthewire.org -p 2220
```

### 2. Inspect the Binary
Look at the file in your home directory. Notice the permissions string:
```bash
bandit19@bandit:~$ ls -l
-rwsr-x--- 1 bandit20 bandit19  14876 Oct 16 14:00 bandit20-do
```


* The owner is **bandit20**.
* The group is **bandit19**.
* The **`s`** in `-rws` indicates the **setuid** bit is active. This means when you run this file, it runs as `bandit20`.

### 3. Test the "Do" Command
The binary `bandit20-do` is designed to execute whatever command you pass to it as arguments, but it will do so with `bandit20`'s privileges.

To prove this, try running `whoami` normally, and then through the binary:
```bash
bandit19@bandit:~$ whoami
bandit19

bandit19@bandit:~$ ./bandit20-do whoami
bandit20
```

### 4. Retrieve the Password
Since the binary lets us "be" `bandit20` for a split second, we can use it to read the password file that only `bandit20` can see.

**The Command:**
```bash
./bandit20-do cat /etc/bandit_pass/bandit20
```

**Output:**
```text
0qP9S88m9Z19X97Jv5i6p9S88m9Z19X9
```

---

## 💡 Key Learning Concepts

### 1. What is SETUID?
Normally, when you run a program (like `ls`), it runs with *your* permissions. If a file has the **setuid** bit (represented by an `s` in the owner's execute position), the program runs with the permissions of the **owner** of the file. 

Common real-world examples include the `passwd` command, which needs root access to change your password in `/etc/shadow`, even when a normal user runs it.

### 2. Privilege Escalation
In cybersecurity, setuid binaries are high-value targets. If a setuid binary is poorly programmed (for example, if it allows you to run shell commands like this one does), a "low-privilege" user can use it to act as a "high-privilege" user.

### 3. Permissions Breakdown
* `rwx`: Read, Write, Execute.
* `rws`: Read, Write, and **SetUID** (Execute is implied).

---
