🚩 OverTheWire Bandit: Level 5 ⮕ Level 6
Level 5 is a step up in complexity. Instead of one folder, you are presented with 20 directories, each containing multiple files. Manually checking each one with file would be exhausting.

This level teaches you how to use the find command—one of the most powerful tools in a Linux administrator's arsenal.
🚩 Objective

The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties:

    Human-readable

    1033 bytes in size

    Not executable

🛠 Commands Used
Command	Purpose
find	Search for files in a directory hierarchy based on specific criteria.
cat	Display the contents of the file.
🚶 Step-by-Step Walkthrough
1. Log in to Level 5

Connect using the password from Level 4 (4oWL31ot9Tl1ptp9Yv89S69Y5ne796BS).
Bash

ssh bandit5@bandit.labs.overthewire.org -p 2220

2. Inspect the Structure

If you look inside inhere, you'll see a mess of folders:
Bash

bandit5@bandit:~$ cd inhere
bandit5@bandit:~/inhere$ ls
maybehere00  maybehere04  maybehere08  maybehere12  maybehere16
maybehere01  maybehere05  maybehere09  maybehere13  maybehere17
... (and so on)

3. Use find to Filter Results

Instead of checking every folder, we will tell the system exactly what we are looking for using the find command.

    .: Search starting from the current directory.

    -type f: Look for files (not directories).

    -size 1033c: Look for exactly 1033 bytes (c stands for bytes).

    ! -executable: The ! means "NOT." We want files that are not executable.

Command:
Bash

find . -type f -size 1033c ! -executable

Output:
Plaintext

./maybehere07/.file2

4. Retrieve the Password

The find command did all the heavy lifting and pointed us directly to the path. Now just cat that specific file:
Bash

cat ./maybehere07/.file2

Output:
Plaintext

HWasnPhtq9AVKe0dmk45nTEyS2S2scS9

💡 Key Learning Concepts
1. The Power of find

The find command doesn't just look for names; it looks for metadata (data about the data). It can filter by:

    Size: -size 10k (kilobytes), -size +1M (greater than 1 megabyte).

    Time: -mtime -1 (modified in the last 24 hours).

    Permissions: -perm 777 (world readable/writable/executable).

2. Size Units in find

Be careful with the suffix!

    c = bytes

    k = kilobytes

    M = megabytes

3. Logical Operators

The ! (NOT) operator is used to exclude results. You can also use -and and -or to build very specific queries, like finding a file that is "this size AND that type."
