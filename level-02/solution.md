This walkthrough covers **Level 2** of the OverTheWire Bandit wargame. At this stage, you’ve already mastered basic navigation and handling special characters like the hyphen. Now, we face a common real-world nuisance: **spaces in filenames**.

---

## 🚩 Objective
The password for the next level is stored in a file called `spaces in this filename` located in the home directory. You must log in as `bandit2` using the password obtained from Level 1 to retrieve it.

---

## 🛠 Commands Used
| Command | Purpose |
| :--- | :--- |
| `ls` | List directory contents. |
| `cat` | Concatenate and display the content of files. |
| `ssh` | Connect to the remote server. |

---

## 🚶 Step-by-Step Walkthrough

### 1. Log in to Level 2
Open your terminal and connect to the Bandit server using the password you found in Level 1 (**CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9**).

```bash
ssh bandit2@bandit.labs.overthewire.org -p 2220
```

### 2. Locate the File
Once logged in, check the contents of your current directory.
```bash
bandit2@bandit:~$ ls
```
**Output:**
```text
spaces in this filename
```

### 3. Read the File (The Challenge)
In Linux, spaces are used to separate commands and arguments. If you simply type `cat spaces in this filename`, the system thinks you are trying to read four separate files: `spaces`, `in`, `this`, and `filename`.

To fix this, you have two primary methods:

#### Option A: Using Quotes (Recommended)
Wrapping the name in double or single quotes tells the shell to treat the entire string as one single filename.
```bash
cat "spaces in this filename"
```

#### Option B: Using Backslashes (Escaping)
You can "escape" each space by putting a backslash `\` before it. This tells the shell, "The next character is part of the name, not a separator."
```bash
cat spaces\ in\ this\ filename
```

### 4. Retrieve the Password
**Command:**
```bash
cat "spaces in this filename"
```
**Output:**
```text
UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK
```

---

## 💡 Key Learning Concepts

### 1. Argument Separation
By default, the Bash shell uses **whitespace** (spaces, tabs, newlines) to determine where one argument ends and the next begins. Understanding how to override this behavior is crucial for managing files on Linux systems.

### 2. Quoting vs. Escaping
* **Quoting (`""` or `''`)**: Good for long names or names with many special characters.
* **Escaping (`\`)**: Useful for a single special character or space.

### 3. Pro-Tip: Tab Completion
Always try hitting the **Tab** key after typing the first few letters of a filename (e.g., `cat spa[TAB]`). Linux will automatically fill in the name and handle the escaping or quoting for you. It’s faster and prevents typos!

---
