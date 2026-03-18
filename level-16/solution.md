## 🚩 OverTheWire Bandit: Level 15 ⮕ Level 16

Level 16 is a major step up. It combines **port scanning**, **service identification**, and **SSL communication**. Instead of being told exactly where to go, you have to "scout" the server to find the right door.

---

## 🚩 Objective
The password for the next level is retrieved by submitting the password of the current level to a port on **localhost** in the range **31000 to 32000**. 

First, you must find which ports are listening. Then, find which one of those speaks **SSL** and which one returns the password (one will simply echo back whatever you type).

---

## 🛠 Commands Used
| Command | Purpose |
| :--- | :--- |
| `nmap` | Network Mapper. Used to discover hosts and services on a computer network. |
| `openssl s_client` | Connect to the identified SSL service. |

---

## 🚶 Step-by-Step Walkthrough

### 1. Log in to Level 16
Connect using the password from Level 15 (**cl6m9S88m9Z19X97Jv5i6p9S88m9Z19X**).

```bash
ssh bandit16@bandit.labs.overthewire.org -p 2220
```

### 2. Scan the Range with Nmap
We need to scan ports **31000 through 32000**. We'll use the `-p` flag for the range and `-sV` to "Service Version" detect, which helps us see what is running on those ports.



**The Command:**
```bash
nmap -p 31000-32000 localhost -sV
```

**Output (Simplified):**
You will likely see several ports. Look for the ones that say `ssl/unknown` or `ssl/echo`.
* Port **31518** (Example): Might be an echo server (useless).
* Port **31790** (Example): Might be the one we need.

### 3. Connect to the Correct SSL Port
Try the ports you found using the `openssl` command from the previous level. 

**The Command:**
```bash
openssl s_client -connect localhost:31790
```

**What happens next:**
1. The SSL handshake completes.
2. **Paste** your current password: `cl6m9S88m9Z19X97Jv5i6p9S88m9Z19X`
3. Press **Enter**.

### 4. Retrieve the RSA Key
Instead of a simple password string, the server will output an **RSA Private Key**. It looks like this:
```text
-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEA75p...
... (many lines of text) ...
-----END RSA PRIVATE KEY-----
```

**CRITICAL STEP:** This is not the password; it's the **Private Key** for Level 17 (just like we used in Level 13). 
1. Copy the **entire block** (including the BEGIN and END lines).
2. Create a file in `/tmp/mykey` and paste the content there.
3. Set the permissions to `600` (or SSH will reject it): `chmod 600 /tmp/mykey`

---

## 💡 Key Learning Concepts

### 1. Nmap (Network Mapper)
Nmap is the gold standard for network discovery. Hackers and sysadmins use it to:
* See which ports are "Open" (listening for connections).
* Identify what software is running (e.g., Apache, OpenSSH).
* Determine the operating system of a remote machine.

### 2. Service Fingerprinting (`-sV`)
By sending specific packets and analyzing the response, `nmap` can guess what a service is. In this level, it helps us distinguish between a "dumb" echo server and a "smart" Bandit server.

### 3. Handling Private Keys
Private keys are raw cryptographic data. When a server gives you one, you must treat it like a physical key. Saving it with the correct permissions (`chmod 600`) is the most common hurdle for beginners.

---
