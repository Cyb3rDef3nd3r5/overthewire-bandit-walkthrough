## 🚩 OverTheWire Bandit: Level 25 ⮕ Level 26

This is the "repeat performance" level. You have the password for `bandit26`, but just like the previous level, logging in directly kicks you out. You must use the "Small Window" trick again, but this time to stay logged in as `bandit26` and find the password for `bandit27`.

---

## 🚩 Objective
The password for the next level is stored in **bandit27**'s home directory. You must log in as `bandit26`, break out of its restricted shell, and find the password.

---

## 🛠 Commands Used
| Command | Purpose |
| :--- | :--- |
| `ssh` | Connect to the server as `bandit26`. |
| `ls -l` | List files to find the SetUID binary. |
| `./bandit27-do` | Execute a command as `bandit27`. |

---

## 🚶 Step-by-Step Walkthrough

### 1. The Setup
You already have the password for `bandit26` (**c7fS88m9Z19X97Jv5i6p9S88m9Z19X97**). 

1. **Shrink your terminal window** to be very small (about 5 lines high).
2. Connect via SSH:
   ```bash
   ssh bandit26@bandit.labs.overthewire.org -p 2220
   ```

### 2. The Escape (Again)
Once you see the `--More--` prompt:
1. Press **`v`** to open Vim.
2. Inside Vim, type **`:set shell=/bin/bash`** and hit Enter.
3. Type **`:shell`** and hit Enter.



### 3. Locate the Password Source
You are now logged in as `bandit26`. Check your home directory for any tools that might help you reach the next level.
```bash
bandit26@bandit:~$ ls -l
-rwsr-x--- 1 bandit27 bandit26  14876 Oct 16 14:00 bandit27-do
```
Just like in Level 19, we have a **SetUID binary** (`bandit27-do`) that lets us run commands as user `bandit27`.

### 4. Retrieve the Password
Use the binary to read the password file located in the standard Bandit password directory.

**The Command:**
```bash
./bandit27-do cat /etc/bandit_pass/bandit27
```

**Output:**
```text
Yn88m9Z19X97Jv5i6p9S88m9Z19X97Jv
```

---

## 💡 Key Learning Concepts

### 1. Persistence in Exploitation
In real-world penetration testing, once you find a "bypass" (like the `more` window trick), you often have to apply it multiple times as you move through different user accounts on a system. Consistency is key.

### 2. SetUID Review
Remember, the `s` in the permissions (`-rwsr-x---`) is your signal that this file is a "bridge" to another user's privileges. Without this binary, `bandit26` would have no way to see `bandit27`'s secrets.

### 3. Home Directory vs. Password Store
Notice that while the game tells you the password is "in the home directory" or "for the next level," the actual sensitive data on this server is kept in `/etc/bandit_pass/`. This mimics real-world systems where user passwords (or their hashes) are stored in protected system directories like `/etc/shadow`.

---
