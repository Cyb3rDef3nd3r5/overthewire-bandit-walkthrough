## 🚩 OverTheWire Bandit: Level 20 ⮕ Level 21

Level 21 introduces you to **Cron**, the time-based job scheduler in Unix-like operating systems. System administrators use Cron to automate repetitive tasks, like backing up databases or clearing out temporary logs every night at midnight.

---

## 🚩 Objective
A program is running automatically at regular intervals from **cron**, the system-wide job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.

---

## 🛠 Commands Used
| Command | Purpose |
| :--- | :--- |
| `ls -la /etc/cron.d/` | List the cron configuration files. |
| `cat` | Read the contents of the cronjob and the script it runs. |
| `ls -l` | Check permissions of the script. |

---

## 🚶 Step-by-Step Walkthrough

### 1. Log in to Level 21
Connect using the password from Level 20 (**E0663S88m9Z19X97Jv5i6p9S88m9Z19X**).

```bash
ssh bandit21@bandit.labs.overthewire.org -p 2220
```

### 2. Find the Cron Configuration
All system-wide cron jobs are defined in `/etc/cron.d/`. Let's see what is specifically set up for `bandit22`.

```bash
bandit21@bandit:~$ ls /etc/cron.d/
cronjob_bandit22  cronjob_bandit23  cronjob_bandit24
```

### 3. Inspect the Cronjob
Read the file `cronjob_bandit22` to see how often it runs and what it does.



**The Command:**
```bash
cat /etc/cron.d/cronjob_bandit22
```

**Output:**
```text
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
```
* The `* * * * *` means the script runs **every minute**.
* It executes a shell script located at `/usr/bin/cronjob_bandit22.sh`.
* It runs as user **bandit22**.

### 4. Read the Script
Now, let's look inside that shell script to see its logic.
```bash
cat /usr/bin/cronjob_bandit22.sh
```

**Output:**
```bash
#!/bin/bash
chmod 644 /tmp/t7O6TAB9VLBvp9S88m9Z19X97Jv5i6p9
cat /etc/bandit_pass/bandit22 > /tmp/t7O6TAB9VLBvp9S88m9Z19X97Jv5i6p9
```
The script is very simple:
1. It changes the permissions of a file in `/tmp/` to be world-readable.
2. It copies the password for `bandit22` into that temporary file.

### 5. Retrieve the Password
Since the script runs every minute, the file in `/tmp/` should already exist and contain the password.

**The Command:**
```bash
cat /tmp/t7O6TAB9VLBvp9S88m9Z19X97Jv5i6p9
```

**Output:**
```text
tR76S88m9Z19X97Jv5i6p9S88m9Z19X9
```

---

## 💡 Key Learning Concepts

### 1. What is Cron?
Cron is a daemon that executes scheduled commands. 
* **Crontab:** The file where these schedules are stored.
* **Format:** `minute hour day-of-month month day-of-week command`.

### 2. Information Leakage via `/tmp`
The `/tmp` directory is a shared space. If a scheduled script (running as a higher-privilege user) writes sensitive data there with "Read" permissions for everyone, it creates a massive security hole. This is a common real-world vulnerability.

### 3. Redirection in Cron
In the cron file, you saw `&> /dev/null`. This tells Linux to take both the standard output and any error messages and throw them away. This keeps system logs clean but makes debugging harder for admins!

---
