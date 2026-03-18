## 🚩 OverTheWire Bandit: Level 8 ⮕ Level 9

Level 9 is a perfect example of how to "snoop" inside files that weren't meant to be read by humans. Up until now, we’ve been dealing with text files. But what happens when the password is hidden inside a **binary file** (like a program or a compressed image)?

If you try to `cat` a binary file, your terminal will explode with gibberish and weird beeping. We need a tool that ignores the "computer code" and only extracts the "human words."

---

## 🚩 Objective
The password for the next level is stored in the file `data.txt` in one of the few **human-readable strings**, preceded by several **'='** characters.

---

## 🛠 Commands Used
| Command | Purpose |
| :--- | :--- |
| `strings` | Extract printable character sequences from a binary file. |
| `grep` | Search for a specific pattern (in this case, the `=` signs). |
| `|` (Pipe) | Connect the output of `strings` to the input of `grep`. |

---

## 🚶 Step-by-Step Walkthrough

### 1. Log in to Level 9
Connect using the password from Level 8 (**EN63pZubrSIOV9b3Y9f598v6S8p1O9Yp**).

```bash
ssh bandit9@bandit.labs.overthewire.org -p 2220
```

### 2. Identify the Problem
Try to see what `data.txt` looks like using the `file` command:
```bash
bandit9@bandit:~$ file data.txt
data.txt: data
```
As we learned in previous levels, `data` means it's a binary file. If you `cat` it, you'll see a mess. We need to filter out the binary junk.

### 3. Extract the Strings
The `strings` command scans a file and prints any sequence of printable characters that is at least 4 characters long.



Since the hint says the password is preceded by several `=` characters, we can pipe the output of `strings` into `grep`.

**The Command:**
```bash
strings data.txt | grep "=="
```

**Output:**
```text
========== the
========== password
========== is
========== G7w8yPcR4Y99pY7v5S88m9Z19X97J
```

### 4. Retrieve the Password
The final line gives you the clear password for Level 10.

**Password:** `G7w8yPcR4Y99pY7v5S88m9Z19X97J`

---

## 💡 Key Learning Concepts

### 1. The `strings` Command
This is a standard forensic and reverse-engineering tool. It is incredibly useful for:
* Finding hardcoded passwords or API keys in compiled software.
* Identifying the version of a program.
* Seeing error messages hidden inside a binary.

### 2. Binary vs. Text
Most files on a computer are binary (images, videos, executables). They are composed of bytes that represent data other than just simple letters. `strings` acts as a sieve, letting the "junk" fall through while catching the "readable" bits.

### 3. Combining Patterns
By using `grep "=="`, we filtered the thousands of strings found in the file down to just the 4 or 5 lines that actually mattered. This is the core of Linux efficiency: **Filter, don't search manually.**

---
