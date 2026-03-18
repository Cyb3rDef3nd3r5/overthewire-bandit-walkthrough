🚩 OverTheWire Bandit: Level 6 ⮕ Level 7
Level 6 is a classic "needle in a haystack" challenge. Instead of searching a single folder, we are searching the **entire file system**. This requires a deeper understanding of how Linux handles permissions and ownership.

---

## 🚩 Objective
The password for the next level is stored **somewhere on the server** and has all of the following properties:
* **Owned by user `bandit7`**
* **Owned by group `bandit6`**
* **33 bytes in size**

---

## 🛠 Commands Used
| Command | Purpose |
| :--- | :--- |
| `find` | Search for files based on ownership and size. |
| `2>/dev/null` | Redirect error messages to "the void" so they don't clutter your screen. |
| `cat` | Display the file content. |

---

## 🚶 Step-by-Step Walkthrough

### 1. Log in to Level 6
Connect using the password from Level 5 (**HWasnPhtq9AVKe0dmk45nTEyS2S2scS9**).

```bash
ssh bandit6@bandit.labs.overthewire.org -p 2220
```

### 2. Search the Entire System
Since we don't know which directory the file is in, we start our search from the **root directory** (`/`). 

We will use `find` with three specific flags:
* `-user bandit7`: Filter by owner.
* `-group bandit6`: Filter by group.
* `-size 33c`: Filter by exact byte count.

**The Command:**
```bash
find / -user bandit7 -group bandit6 -size 33c
```

### 3. Dealing with "Permission Denied"
When you run the command above, your screen will be flooded with "Permission denied" errors. This happens because `find` is trying to look into system folders (like `/root` or `/etc`) that your low-level user isn't allowed to see.

To clean this up, we use **stderr redirection**: `2>/dev/null`.



**The Refined Command:**
```bash
find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
```

**Output:**
```text
/var/lib/dpkg/info/bandit7.password
```

### 4. Retrieve the Password
Now that you have the path, simply read the file:
```bash
cat /var/lib/dpkg/info/bandit7.password
```

**Output:**
```text
m6P0W3M69reY99v5i6p9S88m9Z19X97J
```

---

## 💡 Key Learning Concepts

### 1. The Root Directory (`/`)
In Linux, the file system is a tree. While `.` represents where you are now, `/` represents the very bottom (the root) of the entire system. Searching from `/` covers every single file on the machine.

### 2. Ownership and Groups
Every file in Linux is owned by a **User** and assigned to a **Group**. This is how the system determines who can read, write, or execute a file. The `find` command is excellent for auditing files based on these owners.

### 3. Redirection (`2>/dev/null`)
* `1` is **stdout** (standard output/normal results).
* `2` is **stderr** (standard error/error messages).
* `/dev/null` is a special virtual device that deletes everything sent to it (the "black hole" of Linux).
* By adding `2>/dev/null`, you are telling the terminal: "Show me the successful matches, but throw the error messages in the trash."

---
