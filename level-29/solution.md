## 🚩 OverTheWire Bandit: Level 28 ⮕ Level 29

Level 29 introduces **Git Branches**. In professional development, branches allow teams to work on different features (like "login-page" or "fix-bug") in parallel universes of the same project. If you can't find a secret in the main history, it might be hiding in a branch that was never merged.

---

## 🚩 Objective
There is a git repository at `ssh://bandit29-git@localhost/home/bandit29-git/repo`. The password for the next level is not in the `master` branch. You must find it in another **branch**.

---

## 🛠 Commands Used
| Command | Purpose |
| :--- | :--- |
| `git branch -a` | List all branches, including remote ones. |
| `git checkout [branch]` | Switch your working directory to a different branch. |
| `git log -p` | View the history of a branch with the actual text changes shown. |

---

## 🚶 Step-by-Step Walkthrough

### 1. Log in to Level 29
Connect using the password from Level 28 (**bbc3663252ff5c11732560Z99S88m9Z1**).

```bash
ssh bandit29@bandit.labs.overthewire.org -p 2220
```

### 2. Clone the Repository
Create your workspace in `/tmp` and clone the repo as usual.
```bash
mkdir /tmp/git_branch_hunt
cd /tmp/git_branch_hunt
git clone ssh://bandit29-git@localhost/home/bandit29-git/repo
cd repo
```

### 3. Check for Other Branches
By default, you are on the `master` (or `main`) branch. Let's see if there are other branches available on the server.



**The Command:**
```bash
git branch -a
```

**Output:**
```text
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/dev
  remotes/origin/master
  remotes/origin/sploit
```
Notice the branch named **`dev`** or **`sploit`**. These are prime targets for finding unfinished or sensitive work.

### 4. Switch to the Target Branch
Let's check out the `dev` branch to see what's inside.
```bash
git checkout dev
```

### 5. Retrieve the Password
Once you've switched branches, check the files again.
```bash
ls
cat README.md
```

If the password isn't in the current file, use `git log -p` to see if it was deleted in this branch's history specifically.

**Output:**
```text
The password to the next level is: tbc3663252ff5c11732560Z99S88m9Z1
```

---

## 💡 Key Learning Concepts

### 1. What are Branches?
A branch is essentially a pointer to a specific commit. Developers use them to experiment without breaking the "production" code (the master branch). In a security context, developers often leave "debug" code or hardcoded credentials in `dev` or `test` branches, assuming they won't be seen by the public.

### 2. Remote vs. Local Branches
* **Local:** Branches you have created or checked out on your machine.
* **Remote (`remotes/origin/...`):** Branches that exist on the server. You must "fetch" or "checkout" these to see their contents.

### 3. The `git checkout` Command
This command is your "portal" between different versions of the code. When you run it, Git instantly swaps the files in your folder to match the state of that branch.

---
