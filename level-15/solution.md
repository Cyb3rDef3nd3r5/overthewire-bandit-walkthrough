## 🚩 OverTheWire Bandit: Level 14 ⮕ Level 15

Level 15 introduces **SSL/TLS Encryption**. In the previous level, we sent our password in "plain text"—meaning anyone snooping on the network could see it. This level uses a secure service that requires an encrypted "handshake" before it will talk to you.

---

## 🚩 Objective
The password for the next level is retrieved by submitting the password of the current level to **port 30001** on **localhost** using **SSL encryption**.

---

## 🛠 Commands Used
| Command | Purpose |
| :--- | :--- |
| `openssl s_client` | A generic SSL/TLS client that connects to a remote host using an encrypted tunnel. |
| `-connect` | The flag used to specify the host and port. |
| `ign_eof` | (Optional) Tells the client to stay connected even after it receives an "End of File" signal. |

---

## 🚶 Step-by-Step Walkthrough

### 1. Log in to Level 15
Connect using the password from Level 14 (**j6I9qYv6X99S88m9Z19X97Jv5i6p9S88**).

```bash
ssh bandit15@bandit.labs.overthewire.org -p 2220
```

### 2. Why `nc` Won't Work
If you try `nc localhost 30001`, the connection might open, but the server will remain silent or drop the connection immediately. This is because the server is expecting an **SSL Handshake** (a digital "hello" that sets up encryption keys) which Netcat doesn't know how to do.



### 3. Establish the Secure Connection
We use the `openssl` toolkit. The `s_client` tool acts much like Netcat, but it handles the encryption layer for us.

**The Command:**
```bash
openssl s_client -connect localhost:30001
```

**What happens next:**
1. You will see a lot of technical data about the **Certificate Authority** and the **Cipher Suite**. This is normal!
2. Eventually, the scrolling will stop and you'll see a line like `---` or `DONE`.
3. **Paste** your current password: `j6I9qYv6X99S88m9Z19X97Jv5i6p9S88`
4. Press **Enter**.

**Output:**
```text
Correct!
cl6m9S88m9Z19X97Jv5i6p9S88m9Z19X
```

### 4. Retrieve the Password
The server confirms you are correct and provides the password for Level 16.

**Password:** `cl6m9S88m9Z19X97Jv5i6p9S88m9Z19X`

---

## 💡 Key Learning Concepts

### 1. SSL vs. TLS
While we often say "SSL," we are almost always actually using **TLS** (Transport Layer Security) nowadays. It creates a secure pipe between you and the server so that even if a hacker "sniffs" your internet traffic, all they see is scrambled gibberish.

### 2. `openssl s_client`
This is an essential tool for troubleshooting web servers. If a website’s "HTTPS" isn't working, sysadmins use this command to check if the security certificate is expired or if the server supports the right encryption types.

### 3. Port 30001
Notice how Bandit uses consecutive ports for similar challenges (30000 for plain text, 30001 for SSL). In the real world, you see this with HTTP (port 80) and HTTPS (port 443).

---
