## 🚩 OverTheWire Bandit: Level 7 ⮕ Level 8

Level 8 introduces us to **data manipulation**. While Level 7 taught us how to find a specific word, Level 8 asks us to find a "unique" needle in a haystack of duplicates. This requires combining multiple commands into a **pipeline**.

---

## 🚩 Objective
The password for the next level is stored in the file `data.txt` and is the **only line of text that occurs exactly once**.

---

## 🛠 Commands Used
| Command | Purpose |
| :--- | :--- |
| `sort` | Sort lines of text alphabetically/numerically. |
| `uniq` | Report or omit repeated lines (requires sorted input). |
| `|` (Pipe) | Pass the output of one command as input to the next. |

---

## 🚶 Step-by-Step Walkthrough

### 1. Log in to Level 8
Connect using the password from Level 7 (**TESKfsRrqvE7XnB9WvST9X96TMY9R914**).

```bash
ssh bandit8@bandit.labs.overthewire.org -p 2220
```

### 2. Understand the Problem
If you `cat data.txt`, you will see hundreds of lines, many of which look identical. We need the one line that isn't repeated.

The command `uniq` is designed for this, but there is a catch: **`uniq` only works on sorted data.** It compares a line to the one immediately below it. If the duplicates are scattered throughout the file, `uniq` won't "see" them.

### 3. Create a Command Pipeline
We need to perform three steps:
1. Read the file (`cat`).
2. Sort the lines so duplicates are touching (`sort`).
3. Find the line that appears only once (`uniq -u`).



**The Command:**
```bash
cat data.txt | sort | uniq -u
```

**Breakdown:**
* `cat data.txt`: Dumps the text.
* `|`: The "Pipe" sends that text to the next command.
* `sort`: Groups all identical lines together.
* `uniq -u`: The `-u` flag stands for "unique"—it tells the system to *only* show lines that occur once.

**Output:**
```text
EN63pZubrSIOV9b3Y9f598v6S8p1O9Yp
```

### 4. Retrieve the Password
The single line returned by the command is your password for Level 9.

**Password:** `EN63pZubrSIOV9b3Y9f598v6S8p1O9Yp`

---

## 💡 Key Learning Concepts

### 1. The Pipe (`|`)
The pipe is the "secret sauce" of Linux. It allows you to chain small, simple tools together to perform complex tasks. Instead of one giant program that does everything, Linux gives you many small tools that you can link together like LEGO bricks.

### 2. Sorting Data
Computers process sorted data much faster and more accurately. Many Linux utilities (like `uniq`, `comm`, and `join`) require the input to be alphabetically or numerically ordered to function correctly.

### 3. `uniq` Flags
* `uniq`: Removes adjacent duplicate lines.
* `uniq -c`: Counts how many times each line appears.
* `uniq -u`: Only prints lines that are truly unique (appear exactly once).
* `uniq -d`: Only prints the duplicate lines.

---
