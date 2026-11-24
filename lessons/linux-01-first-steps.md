# Linux Lesson 1: First steps (full notes)

## 1. The big picture (definitions)

| Term | Meaning |
|---|---|
| **Operating system (OS)** | Software that manages the hardware (CPU, memory, disk, network) and lets programs run. Windows, Linux and macOS are operating systems. |
| **Kernel** | The core of the OS. It talks to hardware and controls processes, memory, files and devices. **Linux is technically the kernel.** |
| **Distribution (distro)** | A complete OS built around the Linux kernel: kernel + tools + package manager. Examples: **Ubuntu**, Debian, Red Hat Enterprise Linux (RHEL), Amazon Linux. Yours is Ubuntu. |
| **Shell** | The program that reads the commands you type and asks the kernel to run them. The most common shell is **bash**. |
| **Terminal** | The window where you type. It runs a shell inside it. |

**Why DevOps and cloud work depends on Linux:** most cloud servers (EC2, Azure VMs), most containers (Docker), and most CI/CD runners run Linux. You manage them by typing commands, usually over SSH, with no mouse.

## 2. Your setup

Windows -> **WSL2** (a real Linux kernel running inside Windows) -> **Ubuntu**.
Open it from the Start menu (type "Ubuntu") or by typing `wsl` in PowerShell.

**The prompt** tells you where you are:

```
student@LAPTOP:~$
```

- `student`: your user name
- `@LAPTOP`: the computer name
- `~`: your current folder (`~` means your home folder)
- `$`: you are a normal user. (`#` would mean root, the all-powerful administrator.)

## 3. The filesystem: one tree starting at `/`

Linux has no C: or D: drives. Everything hangs from one root, `/`.

| Directory | What it holds |
|---|---|
| `/` | Root: top of everything |
| `/home` | Users' personal folders (`/home/yourname`, also written `~`) |
| `/root` | Home folder of the root user |
| `/etc` | **Configuration files** (system and services) |
| `/var` | Data that changes: logs in `/var/log`, web files, databases |
| `/usr` | Installed programs and libraries |
| `/bin`, `/sbin` | Essential commands (user and admin) |
| `/tmp` | Temporary files, cleared on reboot |
| `/opt` | Optional third-party software |
| `/dev` | Devices appear as files |
| `/proc` | Live information about processes and the kernel |
| `/mnt/c` | **In WSL only:** your Windows C: drive |

> Tip: keep your work inside your Linux home (`~`), not under `/mnt/c`. It is much faster.

## 4. Paths

- **Absolute path:** starts with `/`, works from anywhere. `/home/student/devops-lab`
- **Relative path:** starts from where you are now. `devops-lab/week1`
- Special names: `.` = current folder, `..` = parent folder, `~` = your home, `-` = the previous folder you were in.

## 5. Anatomy of a command

```
command  [options]  [arguments]
ls       -l         /etc
```

Options change behaviour (`-l` = long listing). Arguments say what to act on. Linux is **case-sensitive**: `Notes.txt` and `notes.txt` are different files.

## 6. Core commands

| Command | What it does | Examples |
|---|---|---|
| `pwd` | Print working directory: where am I? | `pwd` |
| `ls` | List files. `-l` long, `-a` include hidden (names starting with `.`), `-h` human sizes | `ls -la` |
| `cd` | Change directory | `cd /etc`, `cd ..`, `cd ~`, `cd -` |
| `mkdir` | Make a directory. `-p` creates parents too | `mkdir -p a/b/c` |
| `touch` | Create an empty file (or update its timestamp) | `touch notes.txt` |
| `cat` | Print a file's contents | `cat notes.txt` |
| `echo` | Print text. With `>` write to a file (overwrites), with `>>` append | `echo "hi" > a.txt` |
| `cp` | Copy. `-r` for folders | `cp a.txt b.txt` |
| `mv` | Move **or rename** | `mv old.txt new.txt` |
| `rm` | Remove files. `-r` folders, `-f` force | `rm a.txt` |
| `rmdir` | Remove an **empty** folder | `rmdir week1` |
| `man` | Manual page for a command (press `q` to quit) | `man ls` |
| `clear` | Clear the screen (or press Ctrl+L) | |

**Danger:** `rm` has **no recycle bin**. A deleted file is gone. Never run `rm -rf` on a path you have not double-checked.

**Keyboard help:** Tab completes names; Up arrow recalls earlier commands; Ctrl+C stops a running command; Ctrl+A / Ctrl+E jump to line start / end.

## 7. Reading `ls -l`

```
-rw-r--r-- 1 student student 33 Nov 24 10:15 notes.txt
```

| Part | Meaning |
|---|---|
| `-` | Type: `-` file, `d` directory, `l` link |
| `rw-` | Owner permissions: read, write, no execute |
| `r--` | Group permissions |
| `r--` | Others' permissions |
| `1` | Number of hard links |
| `student` (first) | Owner |
| `student` (second) | Group |
| `33` | Size in bytes |
| `Nov 24 10:15` | Last modified |
| `notes.txt` | Name |

Permissions are numbers too: r = 4, w = 2, x = 1. So `rw-` = 6, `r--` = 4, making `644`. We'll go deep on this in the next Linux lesson.

## 8. Wildcards

- `*` matches any characters: `ls *.txt`
- `?` matches exactly one character: `ls file?.txt`

## 9. Guided session (type each line; read the output)

```
cd ~
pwd
mkdir -p devops-lab/week1
cd devops-lab/week1
touch notes.txt todo.txt
echo "Day 1 of Linux" > notes.txt
echo "Learn permissions" >> notes.txt
cat notes.txt
ls -l
cp notes.txt backup.txt
mv todo.txt tasks.txt
ls
rm backup.txt
cd ..
pwd
```

**Expected results:**
- `pwd` prints `/home/<yourname>`.
- `cat notes.txt` prints two lines: `Day 1 of Linux` and `Learn permissions`.
- `ls -l` shows `notes.txt` with size **33** bytes and `todo.txt` with size **0**.
- After `cp` and `mv`, `ls` prints `backup.txt  notes.txt  tasks.txt`.
- The last `pwd` prints `/home/<yourname>/devops-lab`.

## 10. Check your understanding (answer in your own words)

1. What is the difference between `cd ..` and `cd ~`?
2. You are in `/home/you/devops-lab`. Give the absolute path and the relative path of `week1/notes.txt`.
3. `a.txt` already contains text. What does `echo "hi" > a.txt` do? What would `>>` do instead?
4. Create the folders `projects/web/css` with **one** command.
5. Why is `rm` more dangerous in Linux than deleting a file in Windows?

## 11. Common errors

- `No such file or directory`: wrong path or spelling (remember case).
- `Permission denied`: you are not allowed. Do not just add `sudo`; understand why first.
- `command not found`: typo, or the program is not installed.
- Names with spaces need quotes: `mkdir "my folder"`.

## 12. Interview questions from this lesson (with answers)

**What is the difference between the kernel and the shell?**
The kernel is the core of the OS that controls hardware and processes. The shell is a program that takes your typed commands and passes them to the kernel.

**What is a Linux distribution?**
A complete operating system built on the Linux kernel, bundled with tools and a package manager. Ubuntu, Debian, RHEL and Amazon Linux are distributions.

**What do `/etc` and `/var/log` contain?**
`/etc` holds configuration files. `/var/log` holds log files, the first place to look when something breaks.

**What is the difference between an absolute and a relative path?**
An absolute path starts at `/` and works from anywhere. A relative path starts from the current directory.

---

## 13. Extra basics you need from day one (so nothing is missed)

**Finding out about your system**

| Command | What it shows |
|---|---|
| `whoami` | Your user name |
| `hostname` | The computer's name |
| `date` | Current date and time |
| `uname -s` | Kernel name (prints `Linux`); `uname -m` prints the architecture (`x86_64`); `uname -a` prints everything |
| `echo $HOME` | Your home path. `$HOME` is an **environment variable** |
| `which python3` | Where a program lives (`/usr/bin/python3`) |
| `history` | List of commands you typed before. Run `!!` to repeat the last one |
| `clear` or Ctrl+L | Clear the screen |
| `exit` or Ctrl+D | Leave the shell |
| `cd` (with nothing after it) | Go to your home folder |
| `cd -` | Go back to the previous folder |

**Getting help (you are never stuck in Linux):** `man ls` opens the manual (press `q` to quit, `/word` to search). `ls --help` prints a short summary. `type cd` tells you whether a command is built in.

**Chaining commands**

| Operator | Meaning | Example |
|---|---|---|
| `;` | Run the next command no matter what | `echo one; echo two` |
| `&&` | Run the next only if the previous **succeeded** | `mkdir backup && echo created` |
| `\|\|` | Run the next only if the previous **failed** | `false \|\| echo "it failed"` |

Verified in your Ubuntu: `true && echo A` prints `A`. `false && echo B` prints nothing. `false || echo C` prints `C`. Running `mkdir x1` twice succeeds the first time and fails the second time, because the folder already exists.

**Brace expansion (create many things at once):** `mkdir -p project/{src,docs,logs}` creates three folders. `ls project` then prints `docs logs src`.

**Hidden files:** any name that starts with `.` is hidden. `ls` skips them, `ls -a` shows them, along with `.` (here) and `..` (parent). Configuration files such as `.bashrc` are hidden files.

**Overwrite vs append (verified):** `echo a > f.txt` then `echo b > f.txt` leaves only `b`. After `echo c >> f.txt`, the file has `b` and `c`. `wc -l < f.txt` counts lines and prints `2`.

**Note:** in a terminal, `ls` prints names in columns. When its output goes through a pipe, it prints one name per line.

## 14. Questions that repeat in technical interviews (Linux basics)

> Honest note: these are the most common fresher questions for system engineer, cloud support and infrastructure roles. Companies don't publish their question sets, so treat this as a strong pattern, not a leaked list. **Practise saying each answer aloud in 30 to 45 seconds**, then read the model answer.

1. **What is Linux? Is it Unix?**
   Linux is an open-source kernel created by Linus Torvalds in 1991. Together with GNU tools and a package manager it forms a distribution such as Ubuntu or RHEL. It is *Unix-like* but not Unix. It is free to use under the GPL licence.
2. **What are the differences between Linux and Windows?** (a favourite)
   Linux is open source, mostly command-line driven, case-sensitive, has one filesystem tree, strong file permissions and runs most of the world's servers. Windows is proprietary, GUI-first, has drive letters, and is mainly used on desktops.
3. **What is the root user? What does `sudo` do?**
   root is the superuser (UID 0) with full control. `sudo` runs **one command** with root privileges, which is safer than staying logged in as root.
4. **Explain the Linux directory structure.**
   Everything is under `/`. `/home` holds user files, `/etc` configuration, `/var/log` logs, `/usr` programs, `/tmp` temporary files, `/bin` and `/sbin` essential commands, `/dev` devices and `/proc` live process information.
5. **What is the difference between an absolute and a relative path?**
   Absolute starts at `/` and works from anywhere. Relative starts from the current directory.
6. **What is the difference between `>` and `>>`?**
   `>` overwrites the file. `>>` appends to it.
7. **How do you see hidden files?** `ls -a`.
8. **What is the difference between `rm` and `rmdir`?**
   `rm` deletes files (and folders with `-r`). `rmdir` deletes only **empty** folders.
9. **What is the difference between `cp` and `mv`?**
   `cp` makes a copy and keeps the original. `mv` moves or renames, so the original is gone.
10. **What does `ls -l` show? Explain `drwxr-xr-x`.**
    A long listing. The first character is the type (`d` = directory). The next three groups are owner, group and others: `rwx` = read, write, execute; `r-x` = read and execute.
11. **What is a shell? Name some shells.**
    A program that reads commands and passes them to the kernel. bash, sh, zsh and fish are shells.
12. **What is the difference between `&&` and `;`?**
    `;` always runs the next command. `&&` runs it only if the previous one succeeded.
13. **What do `/`, `~` and `/root` mean?** (the classic confusion)
    `/` is the root **directory** (top of the tree). `~` is your **home** directory. `/root` is the home directory of the **root user**. "root" has three different meanings, so listen carefully to the question.
14. **What does `cd` do with no arguments?** It goes to your home directory.
15. **Why is Linux preferred for servers?**
    Stable, secure, free, lightweight, easy to automate from the command line, and the standard for cloud and containers.

## 15. Expected hands-on practice (type these in your Ubuntu)

1. Create `company/{hr,tech,sales}` with **one** command and list it.
2. Inside `tech`, create `team1.txt` containing "DevOps team", then add the line "Cloud team" without erasing the first one. Show the file.
3. Copy `team1.txt` into `hr` as `copy.txt`, then rename `copy.txt` to `staff.txt`.
4. Show the hidden files in your home directory. Name two that you see.
5. Print your user name, hostname and kernel name in **one line** using `;`.
6. Delete the empty `sales` folder safely.
7. Use `&&` to create a folder `backup` and, only if that succeeded, create `a.txt` inside it.
8. Without using `cd`, list `/etc` in long format.
9. From any folder, go home, and then jump back to the previous folder, with one command each.
10. What does the command `cd` do when you type nothing after it? Prove it.

**Answers and solutions:** I'll explain after you try them, and add them here.
