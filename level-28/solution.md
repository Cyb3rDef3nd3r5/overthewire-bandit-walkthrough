## 🚩 OverTheWire Bandit: Level 27 ⮕ Level 28

Level 28 is a classic lesson in **Git Forensics**. In the real world, a developer might realize they accidentally committed a password, "delete" it in a new update, and think it's gone. However, Git remembers *everything* unless the history is specifically scrubbed.

---

## 🚩 Objective
There is a git repository at `ssh://bandit28-git@localhost/home/bandit28-git/repo`. The password for the next level was previously in the repository but has since been changed or "deleted." You need to find it in the **commit history**.

---

## 🛠 Commands Used
| Command | Purpose |
| :--- | :--- |
| `git log` | View the history of commits (changes) made to the repository. |
| `git show [hash]` | See the specific changes made in a particular commit. |
| `git checkout [hash]` | Switch the entire working directory to a previous state in time. |

---

## 🚶 Step-by-Step Walkthrough

### 1. Log in to Level 28
Connect using the password from Level 27 (**0Z99S88m9Z19X97Jv5i6p9S88m9Z19X9**).

```bash
ssh bandit28@bandit.labs.overthewire.org -p 2220
```

### 2. Clone the Repository
Create your workspace in `/tmp` and clone the repo as we did before.
```bash
mkdir /tmp/git_recovery
cd /tmp/git_recovery
git clone ssh://bandit28-git@localhost/home/bandit28-git/repo
cd repo
```

### 3. Inspect the Current State
If you look at the `README.md` now, the password is obscured:
```bash
cat README.md
# Output: The password is: xxxxxxxxxxxxxxxx
```

### 4. Travel Back in Time
We need to see who changed this file and what it looked like before. Use `git log` to see the list of "Checkpoints" (commits).



**The Command:**
```bash
git log
```

**Output (Example):**
```text
commit d1a2b3c4e5f6...
Author: Bandit <bandit@overthewire.org>
Date:   Thu Mar 19 2026

    fix: remove password from readme

commit a7b8c9d0e1f2...
Author: Bandit <bandit@overthewire.org>
Date:   Wed Mar 18 2026

    add password to readme
```

### 5. View the Secret Commit
The second commit looks promising ("add password"). We can use `git show` with the **Commit Hash** (that long string of letters and numbers) to see exactly what was added.

**The Command:**
```bash
git show a7b8c9d0e1f2
```

**Output:**
```diff
- The password is: [OLD_STUFF]
+ The password is: b988m9Z19X97Jv5i6p9S88m9Z19X97Jv
```

---

## 💡 Key Learning Concepts

### 1. Git Immortality
In Git, "Deleting" a file or a line of text only hides it from the *current* view. Every previous version is stored in the `.git` directory. This is why "Secret Scanning" is so important in DevOps—hackers specifically look for deleted keys in old commits.

### 2. Commits and Hashes
Every time a developer saves their work in Git, it creates a **Commit**. This commit is identified by a unique **SHA-1 Hash**. If you have the hash, you can see exactly what the project looked like at that exact second in history.

### 3. The `git show` Command
This is the most direct way to see a "diff" of a past change. It shows you what lines were removed (marked with `-` in red) and what lines were added (marked with `+` in green).

---
