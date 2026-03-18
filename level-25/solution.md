## 🚩 OverTheWire Bandit: Level 24 ⮕ Level 25

Level 25 is a "Jailbreak" challenge. You have the password for `bandit25`, but there is a catch: when you log in, you aren't given a normal Bash shell. Instead, a custom script runs, displays some text, and immediately disconnects you. 

To win, you have to interrupt the script before it finishes and "force" the terminal to give you a shell.

---

## 🚩 Objective
The password for the next level is stored in **bandit26**'s home directory. You have the password for `bandit25`, but their default shell is a restricted script. You must break out of this script to gain a command prompt.

---

## 🛠 Commands Used
| Command | Purpose |
| :--- | :--- |
| `ssh` | Connect to the server. |
| `v` | Inside the `more` interface, opens the **Vim** editor. |
| `:set shell=/bin/bash` | A Vim command to define which shell to use. |
| `:shell` | A Vim command to drop into the defined shell. |

---

## 🚶 Step-by-Step Walkthrough

### 1. Log in to Level 25
Connect using the password from Level 24 (**i869S88m9Z19X97Jv5i6p9S88m9Z19X9**).

```bash
ssh bandit25@bandit.labs.overthewire.org -p 2220
```

**What happens:**
You see a text-art "Bandit" logo, and then the connection immediately closes with a "Goodbye!" message. This is because the user's shell is set to a script that simply shows the logo and exits.

### 2. The "Small Window" Trick
The script uses the `more` command to display the text. In Linux, `more` only stays open if the text is **larger** than the window. If the window is huge, `more` finishes instantly and closes the connection.

**The Fix:**
1. **Shrink your terminal window** until it is very short (only 4 or 5 lines high).
2. Log in again.
3. Because the window is tiny, `more` will pause and wait for you to scroll. You will see a `--More--` prompt at the bottom. **Do not press Enter!**



### 3. Escape to Vim
While the `more` prompt is active, you can press keys to trigger special functions.
1. Press **`v`**. This tells `more` to open the current text in the **Vim editor**.
2. Now you are inside Vim, but you still don't have a shell.

### 4. Break out to Bash
Inside Vim, we can override the environment and force a shell to open.
1. Type **`:set shell=/bin/bash`** and press **Enter**. (This tells Vim: "If I ask for a shell, use Bash").
2. Type **`:shell`** and press **Enter**.



### 5. Retrieve the Password
You are now "inside" as `bandit25` with a working prompt! However, the objective says the password for the next level is in **bandit26**'s home directory.

**The Command:**
```bash
bandit25@bandit:~$ ls /home/bandit26
bandit26-do  bandit27-do  (etc)
bandit25@bandit:~$ cat /home/bandit26/bandit26-do
```
Wait—that doesn't work! You'll need to use the **bandit26-do** setuid binary (just like in Level 19) to read the password.

**The Final Command:**
```bash
./bandit26-do cat /etc/bandit_pass/bandit26
```

**Output:**
```text
c7fS88m9Z19X97Jv5i6p9S88m9Z19X97
```

---

## 💡 Key Learning Concepts

### 1. Restricted Shells
Administrators sometimes limit users to "Restricted Shells" (like `rbash`) or custom scripts to prevent them from running dangerous commands. "Jailbreaking" is the art of finding a program that was allowed (like `more` or `vi`) and using its internal features to launch a full shell.

### 2. The `more` Command
`more` is a "pager." Its job is to let you read text one screen at a time. Because it allows you to launch an editor (`v`), it is a common escape route in restricted environments.

### 3. Vim Escapes
Vim is incredibly powerful. If you can get into Vim, you can usually get to a shell. This is why security-conscious admins are very careful about which users are allowed to run text editors with `sudo` or as a login shell.

---
