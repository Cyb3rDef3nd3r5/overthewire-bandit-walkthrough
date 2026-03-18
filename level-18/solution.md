## 🚩 OverTheWire Bandit: Level 17 ⮕ Level 18

Level 18 is a clever lesson in how **login scripts** work. Usually, when you log into a Linux server, a script (like `.bashrc` or `.profile`) runs to set up your environment. In this level, the administrators have modified that script to kick you out the second you arrive.

---

## 🚩 Objective
The password for the next level is stored in a file called `readme` in the home directory. However, someone has modified the `.bashrc` file to log you out immediately upon a successful SSH connection.

---

## 🛠 Commands Used
| Command | Purpose |
| :--- | :--- |
| `ssh [command]` | Executing a command remotely *without* starting an interactive shell. |
| `cat` | Read the file content. |

---

## 🚶 Step-by-Step Walkthrough

### 1. Identify the Trap
Try to log in normally using the password from Level 17 (**xLYugtXunI9S88m9Z19X97Jv5i6p9S88**).

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220
```

**Output:**
```text
bandit18@bandit.labs.overthewire.org's password: 
Byebye !
Connection to bandit.labs.overthewire.org closed.
```
The "Byebye !" message proves that the login script is executing a `logout` or `exit` command as soon as you connect.

### 2. The SSH "Command" Trick
When you use SSH, you don't *have* to start an interactive terminal session. You can tell SSH to log in, run one specific command, and then disconnect. Since the malicious `.bashrc` script only triggers for **interactive shells**, running a direct command often bypasses it.



**The Command:**
```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 "cat readme"
```

### 3. Retrieve the Password
After entering your password, instead of seeing "Byebye !", the server will execute the `cat readme` command and send the text back to your screen before the connection closes.

**Output:**
```text
awhqfNnAbcbS1p9S88m9Z19X97Jv5i6p
```

---

## 💡 Key Learning Concepts

### 1. Interactive vs. Non-Interactive Shells
* **Interactive:** What you usually do. You get a prompt (like `bandit18@bandit:~$`) and type commands. This triggers scripts like `.bashrc`.
* **Non-Interactive:** You send a command via SSH. The server runs it in the background and sends you the result. This often skips user-specific profile scripts.

### 2. The `.bashrc` File
This is a hidden script in every user's home directory. It’s meant for setting aliases or changing your prompt color, but it can be abused to restrict user access, as seen in this level.

### 3. SSH Command Execution
This is a vital tool for automation. System administrators use this to check the status of 100 servers at once using a script, rather than logging into each one manually.

---
