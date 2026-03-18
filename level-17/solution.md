## 🚩 OverTheWire Bandit: Level 16 ⮕ Level 17

Level 17 is a transition level. You are moving from a user you have a password for (`bandit16`) to a user you only have a **Private Key** for (`bandit17`). This level introduces `diff`, a vital tool for developers and sysadmins to spot the exact changes between two versions of a file.

---

## 🚩 Objective
The password for the next level is stored in **passwords.new** and is the only line that has been changed between **passwords.old** and **passwords.new**.

---

## 🛠 Commands Used
| Command | Purpose |
| :--- | :--- |
| `ssh -i` | Login using the RSA Private Key you saved in the previous level. |
| `diff` | Compare two files line by line and show the differences. |
| `chmod` | Change file permissions (essential for using SSH keys). |

---

## 🚶 Step-by-Step Walkthrough

### 1. Prepare the RSA Key
Back in Level 16, you copied an RSA Private Key. You need to save this into a file on the Bandit server (or your own machine) to log in as `bandit17`.

**The Setup:**
1. Create a file in a temporary directory: `nano /tmp/key17`
2. Paste the **entire** RSA key (including the `BEGIN` and `END` lines).
3. **Crucial:** Set the permissions so only you can read it.
   ```bash
   chmod 600 /tmp/key17
   ```

### 2. Log in as Bandit 17
Now, use that key to jump directly to `bandit17`.
```bash
ssh -i /tmp/key17 bandit17@bandit.labs.overthewire.org -p 2220
```

### 3. Compare the Files
In your home directory, you will see two files: `passwords.old` and `passwords.new`. They are full of random strings. We need to find the one line that is different in the `.new` file.



**The Command:**
```bash
diff passwords.old passwords.new
```

**Output (Simplified):**
```text
42c42
< old_garbage_password_string
---
> xLYugtXunI9S88m9Z19X97Jv5i6p9S88
```

* The `<` symbol shows what was in the **old** file.
* The `>` symbol shows what is in the **new** file.

### 4. Retrieve the Password
The line starting with `>` is your password for Level 18.

**Password:** `xLYugtXunI9S88m9Z19X97Jv5i6p9S88`

---

## 💡 Key Learning Concepts

### 1. The `diff` Utility
`diff` is the backbone of version control systems like **Git**. It tells you exactly what was added, removed, or changed. 
* **`c`** means "changed"
* **`a`** means "added"
* **`d`** means "deleted"

### 2. Key Permissions (`chmod 600`)
If you try to use an SSH key that is "world-readable," the SSH client will throw a "Load key: bad permissions" error. This is a safety feature to prevent other users on a multi-user system from stealing your identity.

### 3. Reading Diff Output
* **`<`**: Refers to the first file mentioned in your command.
* **`>`**: Refers to the second file mentioned.
In cybersecurity, we often use `diff` to compare a "known good" configuration file against a potentially compromised one to see if a hacker has modified anything.

---
