## 🚩 OverTheWire Bandit: Level 19 ⮕ Level 20

Level 20 is a fantastic challenge because it combines **SetUID binaries** (from Level 19) with **Network Sockets** (from Level 14). This time, the program won't just give you the password; it wants to "talk" to another process on the server to verify you.

---

## 🚩 Objective
There is a setuid binary called `suconnect` in the home directory. It connects to a port on **localhost** that you specify as an argument. It then reads a line of text from that port and compares it to the password in the current level (`bandit20`). If they match, it will output the password for the next level (`bandit21`).

---

## 🛠 Commands Used
| Command | Purpose |
| :--- | :--- |
| `nc -l -p [port]` | Use Netcat to **listen** on a specific port. |
| `&` | Run a command in the **background**. |
| `./suconnect` | The setuid binary that performs the connection. |
| `jobs` / `fg` | Manage background processes. |

---

## 🚶 Step-by-Step Walkthrough

### 1. Log in to Level 20
Connect using the password from Level 19 (**0qP9S88m9Z19X97Jv5i6p9S88m9Z19X9**).

```bash
ssh bandit20@bandit.labs.overthewire.org -p 2220
```

### 2. The Strategy
We need to be both the **server** and the **client** at the same time.
1. We will set up a listener (`nc`) on a random port (e.g., 1234).
2. We will tell that listener to "send" our current password as soon as someone connects.
3. We will run `./suconnect 1234` so it connects to our listener.



### 3. Create the Listener (Background)
We use the `&` symbol to keep the listener running while we type our next command.

**The Command:**
```bash
echo "0qP9S88m9Z19X97Jv5i6p9S88m9Z19X9" | nc -l -p 1234 &
```
* `echo "..."`: Prepares your current password.
* `| nc -l -p 1234`: Pipes that password into a Netcat listener on port 1234.
* `&`: Sends this process to the background so you get your prompt back.

### 4. Run the Binary
Now, run the setuid program and tell it to connect to your "fake" server on port 1234.

**The Command:**
```bash
./suconnect 1234
```

**What happens:**
1. `suconnect` connects to port 1234.
2. `nc` sends the password we gave it.
3. `suconnect` sees the password matches what it expects.
4. It prints the password for Level 21.

**Output:**
```text
Read: 0qP9S88m9Z19X97Jv5i6p9S88m9Z19X9
Password matches, sending next password
E0663S88m9Z19X97Jv5i6p9S88m9Z19X
```

### 5. Retrieve the Password
The final string is your password for Level 21.

**Password:** `E0663S88m9Z19X97Jv5i6p9S88m9Z19X`

---

## 💡 Key Learning Concepts

### 1. Listening vs. Connecting
* **Connecting (Client):** You reach out to a server (like `nc google.com 80`).
* **Listening (Server):** You open a port and wait for others to reach out to you (`nc -l`). In this level, you acted as the server to "trick" the binary.

### 2. Background Jobs (`&`)
In Linux, you don't need multiple terminal windows to do two things at once. Adding `&` to the end of a command lets it run "behind the scenes." You can see your background tasks by typing `jobs`.

### 3. Inter-Process Communication (IPC)
This level demonstrates a simple form of IPC. Two different programs (the setuid binary and your netcat listener) are talking to each other through a network socket to exchange information.

---
