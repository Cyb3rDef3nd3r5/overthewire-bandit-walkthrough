🚩 OverTheWire Bandit: Level 7 ⮕ Level 8

Level 7 moves us away from searching for a *file* and into searching **inside** a file. Imagine a text file with thousands of lines; scrolling through it manually is impossible. This is where `grep` becomes your most powerful ally.

---

## 🚩 Objective
The password for the next level is stored in the file `data.txt` next to the word **"millionth"**.

---

## 🛠 Commands Used
| Command | Purpose |
| :--- | :--- |
| `grep` | Search for a specific pattern (text) within a file. |
| `ls -lh` | List files with human-readable sizes (to see how big `data.txt` is). |
| `wc -l` | Count the number of lines in a file (optional, to see the scale). |

---

## 🚶 Step-by-Step Walkthrough

### 1. Log in to Level 7
Connect using the password from Level 6 (**m6P0W3M69reY99v5i6p9S88m9Z19X97J**).

```bash
ssh bandit7@bandit.labs.overthewire.org -p 2220
```

### 2. Check the File Size
Before trying to read a file, it’s good practice to see how large it is.
```bash
bandit7@bandit:~$ ls -lh
-rw-r----- 1 bandit8 bandit7 4.0M Oct 16 14:00 data.txt
```
At **4.0MB**, this is a huge text file. If you try to `cat` it, your terminal will scroll for minutes!

### 3. Use `grep` to Find the Needle
We know the password is right next to the word **"millionth"**. We can use `grep` to "filter" the file and only show lines that contain that specific word.



**The Command:**
```bash
grep "millionth" data.txt
```

**Output:**
```text
millionth   TESKfsRrqvE7XnB9WvST9X96TMY9R914
```

### 4. Retrieve the Password
The string on the right is your password for Level 8.

**Password:** `TESKfsRrqvE7XnB9WvST9X96TMY9R914`

---

## 💡 Key Learning Concepts

### 1. What is `grep`?
`grep` stands for **G**lobal **R**egular **E**xpression **P**rint. It is the industry-standard tool for searching through logs, code, and data. Instead of reading the whole file, you ask the computer to "find me the line that looks like X."

### 2. Pipes and Filtering (Preview)
While we used `grep word file` here, you will soon learn to "pipe" commands together using the `|` symbol. For example, `cat data.txt | grep "millionth"` does the exact same thing—it takes the output of one command and shoves it into the next.

### 3. Case Sensitivity
By default, `grep` is case-sensitive. If you searched for "Millionth" (capital M), it might not find anything. If you aren't sure about the casing, you can use the `-i` flag:
`grep -i "millionth" data.txt`

---

**Ready for Level 8? Things get even more interesting—we'll be looking for a line of text that occurs exactly once in a file full of duplicates! Shall I continue?**
