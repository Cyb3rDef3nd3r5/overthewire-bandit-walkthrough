## 🚩 OverTheWire Bandit: Level 13 ⮕ Level 14

Level 14 introduces you to **Network Sockets**. In the real world, services (like web servers, databases, or mail servers) "listen" on specific ports. To get the next password, you have to behave like a client and send data to a service running on the server.

---

## 🚩 Objective
The password for the next level is retrieved by submitting the password of the current level to **port 30000** on **localhost**.

---

## 🛠 Commands Used
| Command | Purpose |
| :--- | :--- |
| `nc` (netcat) | The "Swiss Army Knife" of networking. Used to read from and write to network connections. |
| `telnet` | An older tool used to communicate with another binary or text-based service (alternative to nc). |

---

## 🚶 Step-by-Step Walkthrough

### 1. Log in to Level 14
Connect using the password from Level 13 (**fGrHPx402xGC7U7rSqkAt9Dc9u1p61px**).

```bash
ssh bandit14@bandit.labs.overthewire.org -p 2220
```

### 2. Understand the Network Port
Think of the server's IP address like a street address for a large apartment building. The **Port Number** (30000) is like the specific apartment number you need to knock on. 

We need to "talk" to apartment 30000 and give it our current password.



### 3. Connect and Submit
We use `nc` (netcat) to open a connection to `localhost` on port `30000`. Once the connection is open, the cursor will just blink—it's waiting for you to provide the "secret word" (your current password).

**The Command:**
```bash
nc localhost 30000
```

**What happens next:**
1. The terminal will pause.
2. **Paste** your current password: `fGrHPx402xGC7U7rSqkAt9Dc9u1p61px`
3. Press **Enter**.

**Output:**
```text
Correct!
j6I9qYv6X99S88m9Z19X97Jv5i6p9S88
```

### 4. Retrieve the Password
The second line of the output is your password for Level 15.

**Password:** `j6I9qYv6X99S88m9Z19X97Jv5i6p9S88`

---

## 💡 Key Learning Concepts

### 1. What is Netcat (`nc`)?
Netcat is a featured-packed networking utility which will read and write data across network connections, using the TCP or UDP protocol. It is used by sysadmins for:
* Port scanning.
* Transferring files.
* Port forwarding.
* Testing if a remote service is "alive."

### 2. Ports and Services
* **Standard Ports:** Web (80/443), SSH (22), FTP (21).
* **High Ports:** Often used for custom applications or "obfuscation" (hiding a service on a non-obvious port). Bandit uses port 30000 for this specific challenge.

### 3. TCP Handshaking
When you run `nc`, a "Three-Way Handshake" happens behind the scenes between your command and the service listening on port 30000. Once that connection is established, you are essentially in a "chat room" with that service.

---
