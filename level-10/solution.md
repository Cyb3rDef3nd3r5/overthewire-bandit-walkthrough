🚩 OverTheWire Bandit: Level 10 ⮕ Level 11

Level 10 introduces us to **Base64 encoding**. This isn't encryption (it's not a secret code meant to be unreadable), but rather a way to represent binary data using only 64 printable characters. It's used everywhere—from email attachments to embedding images in HTML.

---

## 🚩 Objective
The password for the next level is stored in the file `data.txt`, which contains **Base64 encoded data**.

---

## 🛠 Commands Used
| Command | Purpose |
| :--- | :--- |
| `base64` | Encode or decode data using the Base64 standard. |
| `cat` | Display the encoded text. |

---

## 🚶 Step-by-Step Walkthrough

### 1. Log in to Level 10
Connect using the password from Level 9 (**G7w8yPcR4Y99pY7v5S88m9Z19X97J**).

```bash
ssh bandit10@bandit.labs.overthewire.org -p 2220
```

### 2. View the Encoded Data
First, let's see what the data looks like. Base64 is easy to spot because it often ends with one or two `=` signs (padding) and uses a mix of uppercase, lowercase, and numbers.

```bash
bandit10@bandit:~$ cat data.txt
```
**Output (Sample):**
```text
VGhlIHBhc3N3b3JkIGlzIDZ6S09SallmSzhSdjVpNm85Uzg4bTlaMTlYOTdKCg==
```

### 3. Decode the Data
We use the `base64` command with the `-d` (decode) flag to turn that scrambled string back into plain text.



**The Command:**
```bash
base64 -d data.txt
```

**Output:**
```text
The password is 6zKORjYfK8Rv5i6o9S88m9Z19X97J
```

### 4. Retrieve the Password
The decoded string gives you the password for Level 11.

**Password:** `6zKORjYfK8Rv5i6o9S88m9Z19X97J`

---

## 💡 Key Learning Concepts

### 1. What is Base64?
Base64 is a binary-to-text encoding scheme. It takes 3 bytes of binary data and represents them as 4 printable characters. Because it only uses "safe" characters (A-Z, a-z, 0-9, +, /), it’s perfect for sending data over systems that might break if they saw "raw" binary code.

### 2. How to Spot Base64
If you see a string of text that:
* Looks like a random jumble of letters and numbers.
* Contains both uppercase and lowercase letters.
* Ends with `==` or `=`, it is almost certainly Base64.

### 3. Encoding vs. Encryption
**Warning:** Never use Base64 to "hide" sensitive information! Anyone with a terminal can decode it in half a second. It is for **transporting** data, not **securing** it.

---
