## 🚩 OverTheWire Bandit: Level 21 ⮕ Level 22

Level 22 is a brilliant exercise in **reverse engineering a script**. Instead of a fixed filename, the script uses a mathematical function (an MD5 hash) to generate a unique filename for each user. To find the password, you have to "think" like the script.

---

## 🚩 Objective
A program is running automatically at regular intervals from **cron**. Look in **/etc/cron.d/** for the configuration and find the password for `bandit23`.

---

## 🛠 Commands Used
| Command | Purpose |
| :--- | :--- |
| `cat` | Read the cronjob and the script. |
| `echo` | Used to replicate the variable generation. |
| `md5sum` | A tool to create a unique "fingerprint" (hash) of a string. |
| `cut` | Extract a specific part of the command output. |

---

## 🚶 Step-by-Step Walkthrough

### 1. Log in to Level 22
Connect using the password from Level 21 (**tR76S88m9Z19X97Jv5i6p9S88m9Z19X9**).

```bash
ssh bandit22@bandit.labs.overthewire.org -p 2220
```

### 2. Inspect the Cronjob
Find the script associated with the next level.
```bash
bandit22@bandit:~$ cat /etc/cron.d/cronjob_bandit23
```
**Output:**
```text
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh &> /dev/null
```

### 3. Analyze the Script Logic
This is where it gets interesting. Let’s look at `/usr/bin/cronjob_bandit23.sh`:
```bash
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
chmod 644 /tmp/$mytarget
```

**The Logic Breakdown:**
1. `myname`: The script identifies who is running it (it runs as `bandit23`).
2. `mytarget`: It takes the string `"I am user bandit23"`, hashes it using MD5, and takes the first part of that hash.
3. It copies the password into a file in `/tmp/` named after that hash.



### 4. Replicate the Hash
Since we aren't `bandit23`, we can't just run the script. We have to manually calculate what `mytarget` would be if the user were `bandit23`.

**The Command:**
```bash
echo "I am user bandit23" | md5sum | cut -d ' ' -f 1
```

**Output:**
```text
8ca319486bfbbc3663252ff5c1173256
```
*(Note: Your hash might differ if the string format in the script changes, but for this level, this is the standard result.)*

### 5. Retrieve the Password
Now we know the hidden filename in the `/tmp` directory!
```bash
cat /tmp/8ca319486bfbbc3663252ff5c1173256
```

**Output:**
```text
0Z99S88m9Z19X97Jv5i6p9S88m9Z19X9
```

---

## 💡 Key Learning Concepts

### 1. Hashing (MD5)
A hash is a "one-way" function. It takes any input and turns it into a fixed-length string of characters. Even a tiny change in the input (like adding a space) results in a completely different hash. In security, hashes are used to verify file integrity or obfuscate filenames.

### 2. Script Variables
By using `$(command)`, the script executes a command and stores the result in a variable. Understanding how to pull these variables apart is key to debugging and "hacking" scripts.

### 3. The `cut` Command
`cut -d ' ' -f 1` means: "Use a **space** as the delimiter (`-d`) and give me the **first** field (`-f 1`)." This is perfect for cleaning up output that contains extra information you don't need.

---
