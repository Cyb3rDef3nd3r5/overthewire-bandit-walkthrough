## 🚩 OverTheWire Bandit: Level 3 ⮕ Level 4

This is where things start to get a bit more "detective-like." In Level 4, we aren't just looking for a file with a specific name; we have to identify which file contains **human-readable** data among a sea of binary "garbage" data.

---

## 🚩 Objective
The password for the next level is stored in the only **human-readable** file in the `inhere` directory.

---

## 🛠 Commands Used
| Command | Purpose |
| :--- | :--- |
| `file` | Determine the type of data contained in a file. |
| `ls -la` | List all files with detailed information. |
| `cat` | Display file contents. |
| `*` (Wildcard) | A symbol representing "everything" in a directory. |

---

## 🚶 Step-by-Step Walkthrough

### 1. Log in to Level 4
Connect using the password from Level 3 (**2ExS0SNC9ZpEsnvsiY79tjv7atvS7ayC**).

```bash
ssh bandit4@bandit.labs.overthewire.org -p 2220
```

### 2. Explore the Directory
Navigate into the `inhere` folder and look at the contents.
```bash
bandit4@bandit:~$ cd inhere
bandit4@bandit:~/inhere$ ls
-file00  -file02  -file04  -file06  -file08
-file01  -file03  -file05  -file07  -file09
```
There are 10 files here, all starting with a hyphen `-`. If you try to `cat` them all, most will output weird symbols and potentially mess up your terminal display because they are **binary** files.

### 3. Identify the Human-Readable File
The `file` command is your best friend here. It looks at the header of a file to tell you what it actually is (e.g., an image, a program, or plain text).

We can use the **wildcard `*`** to check every file in the directory at once. Remember to use `./` so the shell doesn't mistake the filenames starting with `-` for command flags!



**Command:**
```bash
file ./*
```

**Output:**
```text
./-file00: data
./-file01: data
./-file02: data
./-file03: data
./-file04: data
./-file05: data
./-file06: data
./-file07: ASCII text
./-file08: data
./-file09: data
```
Notice that `-file07` is the only one labeled **ASCII text**.

### 4. Retrieve the Password
Now that we know which file is the winner, read it:
```bash
cat ./-file07
```

**Output:**
```text
4oWL31ot9Tl1ptp9Yv89S69Y5ne796BS
```

---

## 💡 Key Learning Concepts

### 1. The `file` Command
In Linux, file extensions (like `.txt` or `.exe`) are mostly for the user's convenience; the system doesn't strictly rely on them. The `file` command actually "peeks" inside the file to see what kind of data it holds.

### 2. ASCII vs. Data (Binary)
* **ASCII text:** Plain text that humans can read (letters, numbers, symbols).
* **Data:** Usually refers to compiled code or binary data. If you try to `cat` this, your terminal might start beepng or showing gibberish because it's trying to interpret machine code as text.

### 3. Wildcards (`*`)
The asterisk `*` is a powerful tool. In this context, `file ./*` tells the computer: "Run the file command on **every** item inside the current directory."

---
