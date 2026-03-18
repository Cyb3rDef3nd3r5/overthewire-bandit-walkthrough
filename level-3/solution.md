🚩 OverTheWire Bandit: Level 3 ⮕ Level 4

In this level, we move from handling tricky filenames to finding hidden files. In Linux/Unix environments, files aren't hidden by a "hidden" attribute checkbox like in Windows; instead, they are identified by a naming convention.
🚩 Objective

The password for the next level is stored in a hidden file in the inhere directory.
🛠 Commands Used
Command	Purpose
cd	Change Directory (move into a folder).
ls -a	List all files, including hidden ones (the -a stands for "all").
cat	Display the contents of the file.
🚶 Step-by-Step Walkthrough
1. Log in to Level 3

Use the password found in Level 2 (UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK).
Bash

ssh bandit3@bandit.labs.overthewire.org -p 2220

2. Enter the Target Directory

List the files to see what's there, then move into the inhere folder.
Bash

bandit3@bandit:~$ ls
inhere
bandit3@bandit:~$ cd inhere

3. Reveal the Hidden File

If you run a standard ls, the directory will appear empty. This is because any file starting with a dot (.) is hidden by default.

To see it, use the -a flag:
Bash

bandit3@bandit:~/inhere$ ls -a
.  ..  .hidden

    . refers to the current directory.

    .. refers to the parent directory.

    .hidden is our target file.

4. Retrieve the Password

Now, simply use cat to read the hidden file.
Bash

bandit3@bandit:~/inhere$ cat .hidden

Output:
Plaintext

2ExS0SNC9ZpEsnvsiY79tjv7atvS7ayC

💡 Key Learning Concepts
1. Hidden Files (Dotfiles)

In Linux, any file or folder starting with a period (.) is considered "hidden." These are often configuration files (like .bashrc or .ssh) that you don't need to see during your normal daily workflow.
2. The ls Flags

    ls: Shows standard files.

    ls -a: Shows all files (including those starting with .).

    ls -al: A common "power user" command that shows all files in a long list format, revealing permissions, sizes, and owners.

3. Navigation (.) and (..)

Every Linux directory contains these two special "link" files:

    . (Single dot): A shortcut for "right here."

    .. (Double dot): A shortcut for "the folder above this one."
