Bandit Level 0 → 1
🎯 Goal

The objective of this level is to find the password for the next user (bandit1). The password is saved in a plain text file named readme, which is located directly inside the home directory of the current user (bandit0).
🧠 Concepts Covered

    Command Line Interface (CLI): Interacting with the Linux operating system using text commands rather than a graphical user interface (GUI).

    Directory Listing: Viewing the contents of the current folder you are operating in.

    File Reading: Outputting the contents of a text file directly to your terminal screen.

    Case Sensitivity: Understanding that Linux treats uppercase and lowercase letters differently (e.g., readme is not the same as Readme).

🛠 Commands Used
Bash

ls
cat readme

🔍 Step-by-Step Explanation

Step 1: Verify your surroundings

    Why the command was chosen: Before reading a file, it is good practice to confirm the file actually exists in your current location.

    What it does: The ls (list) command displays the files and folders in your current working directory.

    What output is expected: You will see the word readme printed on the screen, confirming the file is right in front of you.

    How it leads to the solution: This verifies the target file name and ensures you do not need to navigate to other directories to find it.

Step 2: Read the file contents

    Why the command was chosen: Since readme is a standard text file, we need a command that prints text file contents to the terminal.

    What it does: The cat (concatenate) command reads data from the file and outputs its entire contents to the standard output (your screen).

    What output is expected: A long string of random alphanumeric characters will be printed. This is the password for the next level.

    How it leads to the solution: By displaying the contents of the file, you directly uncover the password needed to log into bandit1 via SSH.

⚠️ Common Mistakes

    Case Sensitivity Errors: Typing cat Readme or cat README. In Linux, file names are strictly case-sensitive. You must type exactly readme.

    Trying to execute the file: Beginners sometimes just type readme into the terminal and hit Enter. This tells Linux to run the file as a program, which will result in a "command not found" or "permission denied" error. You must explicitly tell the system what to do with the file (in this case, read it with cat).

    Using a text editor unnecessarily: While commands like nano or vim will also open the file, they are full text editors and overkill for simply viewing a single line of text.

🔐 Security Relevance

In real-world cybersecurity, this level represents the danger of Cleartext Credential Storage.

Developers and system administrators sometimes carelessly leave sensitive information—like API keys, database passwords, or backup credentials—in plain text files (e.g., passwords.txt, config.json, or readme.md) on a server. During an initial compromise, one of the first things a penetration tester or malicious actor will do is use basic commands like ls and cat to search for these "low-hanging fruit" files to escalate their privileges.
💡 Key Takeaways

    The ls command is your primary tool for seeing what files are available in a directory.

    The cat command is the quickest and most fundamental way to read the contents of a file in Linux.

    Linux is completely case-sensitive; precision in typing file names is strictly required.
