## 🚩 OverTheWire Bandit: Level 10 ⮕ Level 11

Level 11 brings us into the world of **Basic Cryptography**. Unlike Base64, which is just a way to format data, **ROT13** is a simple substitution cipher. It’s the "Secret Decoder Ring" of the internet age, often used in forums to hide spoilers.

---

## 🚩 Objective
The password for the next level is stored in `data.txt`, where all lowercase (a-z) and uppercase (A-Z) letters have been **rotated by 13 positions**.

---

## 🛠 Commands Used
| Command | Purpose |
| :--- | :--- |
| `tr` | Translate or delete characters. |
| `cat` | Display the scrambled text. |

---

## 🚶 Step-by-Step Walkthrough

### 1. Log in to Level 11
Connect using the password from Level 10 (**6zKORjYfK8Rv5i6o9S88m9Z19X97J**).

```bash
ssh bandit11@bandit.labs.overthewire.org -p 2220
```

### 2. View the Scrambled Data
If you look at the file, it looks like a Caesar cipher (jumbled letters).
```bash
bandit11@bandit:~$ cat data.txt
```
**Output (Sample):**
```text
Gur cnffjbeq vf WHRStSRE9qV99i6S88m9Z19X97J
```

### 3. Use `tr` to Decode ROT13
The `tr` (translate) command takes a set of characters and maps them to another set. To solve ROT13, we tell `tr` to take the alphabet starting at 'A' and shift it to start at 'N' (the 14th letter).



**The Command:**
```bash
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

**Breakdown:**
* `'A-Za-z'`: This is our source set (the standard alphabet).
* `'N-ZA-Mn-za-m'`: This is our target set. It says "Map A to N, B to O... and when you hit Z, wrap back around to A."

**Output:**
```text
The password is JUR5fFpE9dV99v6S88m9Z19X97J
```

### 4. Retrieve the Password
The translated string gives you the password for Level 12.

**Password:** `JUR5fFpE9dV99v6S88m9Z19X97J`

---

## 💡 Key Learning Concepts

### 1. What is ROT13?
ROT13 ("rotate by 13 places") is a special case of the **Caesar Cipher**. Because the English alphabet has 26 letters, applying ROT13 twice gets you back to the original text. It is its own inverse!

### 2. The `tr` Command
`tr` is a "stream editor." It doesn't read files directly (which is why we pipe `cat` into it). It is perfect for:
* Swapping case (upper to lower).
* Deleting specific characters (like removing all tabs).
* Basic character replacement like we did here.

### 3. Character Ranges
In Linux commands, `A-Z` is a shorthand for every letter in that range. When using `tr`, you must ensure the "from" list and the "to" list are the exact same length, or the command will behave unexpectedly.

---
