## 🚩 OverTheWire Bandit: Level 12 ⮕ Level 13

Level 13 introduces a fundamental concept in cybersecurity and server administration: **Key-Based Authentication**. Instead of typing a password, you will use a "Private Key" (a special file) to prove your identity to the server.

---

## 🚩 Objective
The password for the next level is stored in **/etc/bandit_pass/bandit14**, but it can only be read by user `bandit14`. For this level, you do not have the password for `bandit14`, but you do have their **SSH private key** stored in a file named `sshkey.private` in your home directory.

---

## 🛠 Commands Used
| Command | Purpose |
| :--- | :--- |
| `ssh -i` | Connect to a server using a specific identity file (private key). |
| `localhost` | The hostname representing the machine you are currently logged into. |
| `cat` | Read the password file once you've logged in as the next user. |

---

## 🚶 Step-by-Step Walkthrough

### 1. Log in to Level 13
Connect using the password from Level 12 (**WB3W6M69reY99v5i6p9S88m9Z19X97J**).

```bash
ssh bandit13@bandit.labs.overthewire.org -p 2220
```

### 2. Locate the Private Key
Check your home directory. You'll see a file that acts as a "digital skeleton key" for the next user.
```bash
bandit13@bandit:~$ ls
sshkey.private
```

### 3. SSH into "Localhost"
Normally, you SSH from your computer to a remote server. Here, you are already on the Bandit server, so you will SSH **from the Bandit server to itself** as the next user (`bandit14`).

The `-i` flag stands for "identity." It tells SSH to use the file `sshkey.private` instead of asking for a password.



**The Command:**
```bash
ssh -i sshkey.private bandit14@localhost -p 2220
```
* **Note:** If prompted "Are you sure you want to continue connecting?", type **yes**.

### 4. Retrieve the Password
Now that you are logged in as `bandit14`, you have the permissions to read the password file for Level 14.
```bash
bandit14@bandit:~$ cat /etc/bandit_pass/bandit14
```

**Output:**
```text
fGrHPx402xGC7U7rSqkAt9Dc9u1p61px
```

---

## 💡 Key Learning Concepts

### 1. SSH Private Keys (`id_rsa`)
Think of a Private Key like a physical key and a Public Key like a lock. 
* The **Public Key** is stored on the server you want to access.
* The **Private Key** stays with you.
* When you connect, the server checks if your private key matches the "lock" it has on file. If it matches, you're in!

### 2. Localhost (`127.0.0.1`)
In networking, `localhost` always refers to the machine you are currently sitting at. It’s a loopback address. Using `ssh bandit14@localhost` is essentially opening a new door into the same house you are already standing in.

### 3. File Permissions and Keys
SSH keys are extremely sensitive. In a real-world scenario, if your private key file is "too readable" (e.g., other users on your PC can see it), SSH will refuse to use it for security reasons. Usually, these files must have `600` permissions (`-rw-------`).

---
