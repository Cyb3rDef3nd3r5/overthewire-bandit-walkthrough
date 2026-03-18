## 🚩 OverTheWire Bandit: Level 29 ⮕ Level 30

Level 30 introduces **Git Tags**. While branches are "moving pointers" (they change as you add new code), a **Tag** is a permanent, static label attached to a specific moment in the project's history. Developers often use tags to mark release versions like `v1.0` or `production-ready`.

---

## 🚩 Objective
There is a git repository at `ssh://bandit30-git@localhost/home/bandit30-git/repo`. The password for the next level is not in any branch, but it is associated with a **tag**.

---

## 🛠 Commands Used
| Command | Purpose |
| :--- | :--- |
| `git tag` | List all tags in the repository. |
| `git show [tagname]` | See the details and data associated with a specific tag. |

---

## 🚶 Step-by-Step Walkthrough

### 1. Log in to Level 30
Connect using the password from Level 29 (**tbc3663252ff5c11732560Z99S88m9Z1**).

```bash
ssh bandit30@bandit.labs.overthewire.org -p 2220
```

### 2. Clone the Repository
As always, set up your workspace in `/tmp`.
```bash
mkdir /tmp/git_tag_hunt
cd /tmp/git_tag_hunt
git clone ssh://bandit30-git@localhost/home/bandit30-git/repo
cd repo
```

### 3. List the Tags
If you check `ls` or `git branch`, you might find nothing useful. Now, let's check for tags.



**The Command:**
```bash
git tag
```

**Output:**
```text
secret
```

### 4. Inspect the Tag
A tag acts like a shortcut to a specific object or commit. We can use `git show` to reveal what that "secret" tag is pointing to.

**The Command:**
```bash
git show secret
```

**Output:**
```text
fbc3663252ff5c11732560Z99S88m9Z1
```

---

## 💡 Key Learning Concepts

### 1. What are Git Tags?
Tags are used to capture a point in history that is used for a marked release version. There are two types:
* **Lightweight:** Just a pointer to a commit.
* **Annotated:** Stored as full objects in the Git database; they include the tagger name, email, date, and a tagging message.

### 2. Why Check Tags in a CTF?
In real-world scenarios, a developer might remove a password from the code and delete the branch it was on, but forget that they created a "Release Tag" earlier that still contains the sensitive data. 

### 3. The `git show` Command (Versatility)
Notice how we've used `git show` for:
* Viewing a specific **Commit**.
* Viewing a **File** at a certain point.
* Viewing the content of a **Tag**.
It is the "Inspector Gadget" tool of the Git CLI.

---
