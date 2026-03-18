## 🚩 OverTheWire Bandit: Level 10 ⮕ Level 11

Level 12 is widely considered the "boss fight" of the early Bandit levels. It tests your ability to identify file types, decompress data repeatedly, and keep your workspace organized. You aren't just running one command; you are performing **reverse engineering**.

---

## 🚩 Objective
The password for the next level is stored in `data.txt`, which is a **hexdump** of a file that has been compressed multiple times.

---

## 🛠 Commands Used
| Command | Purpose |
| :--- | :--- |
| `xxd -r` | Reverse a hexdump back into a binary file. |
| `file` | Determine the type of compression used. |
| `mv` | Rename files to give them the correct extension. |
| `gzip / gunzip` | Decompress `.gz` files. |
| `bzip2 -d` | Decompress `.bz2` files. |
| `tar -xf` | Extract files from a `.tar` archive. |

---

## 🚶 Step-by-Step Walkthrough

### 1. Setup a Workspace
Since we will be creating many temporary files, it’s best to work in a temporary directory where we have write permissions.

```bash
mkdir /tmp/mywork12
cp data.txt /tmp/mywork12
cd /tmp/mywork12
```

### 2. Reverse the Hexdump
The file `data.txt` is a text representation of binary code. We use `xxd` to turn it back into a real file.
```bash
xxd -r data.txt > datafile
```

### 3. The Decompression Loop
From here, the game is a loop: **Check the file type ⮕ Rename with extension ⮕ Decompress.**



#### Round 1: gzip
```bash
file datafile 
# Output: gzip compressed data
mv datafile datafile.gz
gunzip datafile.gz
```

#### Round 2: bzip2
```bash
file datafile
# Output: bzip2 compressed data
bzip2 -d datafile
# This creates 'datafile.out'
```

#### Round 3: gzip (again)
```bash
file datafile.out
# Output: gzip compressed data
mv datafile.out datafile.gz
gunzip datafile.gz
```

#### Round 4: tar
```bash
file datafile
# Output: POSIX tar archive
tar -xf datafile
# This extracts a new file, usually 'data5.bin'
```

#### Round 5: tar
```bash
file data5.bin
# Output: POSIX tar archive
tar -xf data5.bin
# Extracts 'data6.bin'
```

#### Round 6: bzip2
```bash
file data6.bin
# Output: bzip2 compressed data
bzip2 -d data6.bin
```

#### Round 7: tar
```bash
file data6.bin.out
# Output: POSIX tar archive
tar -xf data6.bin.out
# Extracts 'data8.bin'
```

#### Round 8: gzip (The Final Stretch!)
```bash
file data8.bin
# Output: gzip compressed data
mv data8.bin data8.gz
gunzip data8.gz
```

### 4. Retrieve the Password
Finally, check the last file.
```bash
file data8
# Output: ASCII text
cat data8
```

**Output:**
```text
The password is WB3W6M69reY99v5i6p9S88m9Z19X97J
```

---

## 💡 Key Learning Concepts

### 1. Hexdumps (`xxd`)
Hexdumps are used to view the raw bytes of a file in a text format. Reversing them (`-r`) is a common task in CTFs (Capture The Flag) when you are given a text snippet of a binary.

### 2. File Signatures (Magic Bytes)
The `file` command works by looking at the first few bytes of a file (the "magic bytes"). Even if you name a file `picture.jpg`, the `file` command will know if it’s actually a `zip` file.

### 3. Nested Compression
In the real world, installers and firmware updates are often "nested"—a compressed archive inside a compressed archive. Mastering this workflow is essential for malware analysis and system administration.

---
