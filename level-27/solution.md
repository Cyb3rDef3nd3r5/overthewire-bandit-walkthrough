## 🚩 OverTheWire Bandit: Level 26 ⮕ Level 27

Level 27 introduces you to **Git**, the industry-standard version control system. In the real world, developers use Git to manage code, but they sometimes accidentally "commit" sensitive information like passwords, API keys, or configuration files into the repository.

---

## 🚩 Objective
There is a git repository at `ssh://bandit27-git@localhost/home/bandit27-git/repo`. The password for the next level is stored in a file in that repository.

---

## 🛠 Commands Used
| Command | Purpose |
| :--- | :--- |
| `mkdir` | Create a temporary directory to work in. |
| `git clone` | Copy a remote repository onto your local machine. |
| `ls -la` | List files in the cloned repository. |
| `cat` | Read the password file. |

---

## 🚶 Step-by-Step Walkthrough

### 1. Log in to Level 27
Connect using the password from Level 26 (**Yn88m9Z19X97Jv5i6p9S88m9Z19X97Jv**).

```bash
ssh bandit27@bandit.labs.overthewire.org -p 2220
```

### 2. Create a Workspace
Since you cannot write files in the home directory, you must create a workspace in `/tmp`.

```bash
mkdir /tmp/myrepo_27
cd /tmp/myrepo_27
```

### 3. Clone the Repository
A "clone" is a full copy of the project, including its history. You will be asked for the password for `bandit27-git`, which is the same as your current password (**Yn88m9Z19X97Jv5i6p9S88m9Z19X97Jv**).



**The Command:**
```bash
git clone ssh://bandit27-git@localhost/home/bandit27-git/repo
```

### 4. Explore the Files
Navigate into the newly created `repo` folder and see what's inside.
```bash
cd repo
ls -la
```

**Output:**
```text
.  ..  .git  README
```

### 5. Retrieve the Password
Check the `README` file; it usually contains instructions or the data itself.
```bash
cat README
```

**Output:**
```text
The password to the next level is: 0Z99S88m9Z19X97Jv5i6p9S88m9Z19X9
```

---

## 💡 Key Learning Concepts

### 1. What is Git?
Git is a distributed version control system. It allows multiple people to work on the same code without overwriting each other's changes. 
* **Repository (Repo):** A folder managed by Git.
* **Clone:** Downloading a repo from a server to your local machine.

### 2. Information Leakage in Repos
One of the most common security vulnerabilities today is "Secret Exposure." Developers often push code to sites like GitHub forgetting that they left a password inside a file. Tools like `truffleHog` or `git-secrets` are used by security pros to find these leaks.

### 3. The `.git` Folder
When you ran `ls -la`, you saw a hidden `.git` folder. This is the "brain" of the repository. It contains the entire history of every change ever made to the code. Even if a password is deleted in the *current* version, it might still be hiding in the history of the `.git` folder!

---
