## 🚩 OverTheWire Bandit: Level 30 ⮕ Level 31

Level 31 flips the script. Instead of just pulling data down, you have to **push** data back to the server. This simulates a real-world "CI/CD" (Continuous Integration/Continuous Deployment) pipeline, where pushing a specific file to a repository triggers an automated script to run on the server.

---

## 🚩 Objective
There is a git repository at `ssh://bandit31-git@localhost/home/bandit31-git/repo`. You need to push a file named **key.txt** containing the text **"May I come in?"** to the remote repository to receive the next password.

---

## 🛠 Commands Used
| Command | Purpose |
| :--- | :--- |
| `git add` | Track a new file in the repository. |
| `git commit` | Save the changes locally with a message. |
| `git push` | Send your local changes to the remote server. |
| `touch` / `echo` | Create the required file. |

---

## 🚶 Step-by-Step Walkthrough

### 1. Log in to Level 31
Connect using the password from Level 30 (**fbc3663252ff5c11732560Z99S88m9Z1**).

```bash
ssh bandit31@bandit.labs.overthewire.org -p 2220
```

### 2. Clone the Repository
As usual, set up your workspace in `/tmp`.
```bash
mkdir /tmp/git_push_test
cd /tmp/git_push_test
git clone ssh://bandit31-git@localhost/home/bandit31-git/repo
cd repo
```

### 3. Create the Required File
The instructions are very specific. You need a file named `key.txt` with exact content.
```bash
echo "May I come in?" > key.txt
```

### 4. The Git Workflow (Add, Commit, Push)
To send this file to the server, you must follow the three-step Git process:



1. **Add**: Tell Git to track this new file.
   ```bash
   git add key.txt
   ```
   *Note: You might see a warning about `.gitignore`. If `key.txt` is being ignored, you can force it with `git add -f key.txt`.*

2. **Commit**: Save the snapshot of your work.
   ```bash
   git commit -m "Adding the key file"
   ```

3. **Push**: Send the data to the master branch on the server.
   ```bash
   git push origin master
   ```

### 5. Retrieve the Password
Once the push is successful, the server's "Post-Receive Hook" (an automated script) will trigger, read your file, and print the password to your terminal screen.

**Output:**
```text
remote: Correct!
remote: The password to the next level is: gbc3663252ff5c11732560Z99S88m9Z1
```

---

## 💡 Key Learning Concepts

### 1. The `.gitignore` File
This is a hidden file in a repo that tells Git which files to **ignore** (like temporary log files or API keys). If you try to add a file that matches a rule in `.gitignore`, Git will stop you unless you use the `-f` (force) flag.

### 2. Git Hooks
Hooks are scripts that Git executes before or after events like `commit` or `push`. In cybersecurity, "Post-Commit" hooks are often used to automatically run security scans or deploy code to a web server.

### 3. Push Access
In this level, you were given "Write" access to the repository. In the real world, controlling who can `push` to a master branch is the most critical part of repository security, as a malicious push could introduce a "backdoor" into the software.

---
