# Linux

## Introductory Terms and Concepts

Before we begin our journey through the wonderful world of Linux Basics for Hackers, let's introduce a few terms that will clarify some concepts discussed later in this chapter.

**Binaries:** This term refers to files that can be executed, similar to executables in Windows. Binaries generally reside in the /usr/bin or usr/sbin directory and include utilities such as `ps`, `cat`, `ls`, and `ifconfig` (we'll touch on all four of these in this chapter) as well as applications such as the wireless hacking tool `aircrack-ng` and the intrusion detection system (IDS) `Snort`.

**Case sensitivity:** Unlike Windows, the Linux filesystem is case sensitive. This means that `Desktop` is different from `desktop`, which is different from `DeskTop`. Each of these would represent a different file or directory name. Many people coming from a Windows environment can find this frustrating. If you get the error message "file or directory not found" and you are sure the file or directory exists, you probably need to check your case.

**Directory:** This is the same as a folder in Windows. A directory provides a way of organizing files, usually in a hierarchical manner.

**Home:** Each user has their own `/home` directory, and this is generally where files you create will be saved by default.

**Kali:** Kali Linux is a distribution of Linux specifically designed for penetration testing. It has hundreds of tools preinstalled, saving you the hours it would take to download and install them yourself. I will be using the latest version of Kali at the time of this writing: Kali 2018.2, first released in April 2018.

**root:** Like nearly every operating system, Linux has an administrator or superuser account, designed for use by a trusted person who can do nearly anything on the system. This would include such things as reconfiguring the system, adding users, and changing passwords. In Linux, that account is called root. As a hacker or pentester, you will often use the root account to give yourself control over the system. In fact, many hacker tools require that you use the root account.

**Script:** This is a series of commands run in an interpretive environment that converts each line to source code. Many hacking tools are simply scripts. Scripts can be run with the bash interpreter or any of the other scripting language interpreters, such as Python, Perl, or Ruby. Python is currently the most popular interpreter among hackers.

**Shell:** This is an environment and interpreter for running commands in Linux. The most widely used shell is bash, which stands for Bourne-again shell, but other popular shells include the C shell and Z shell. I will be using the bash shell exclusively in this book.

**Terminal:** This is a command line interface (CLI).

## The Linux Filesystem

The Linux filesystem structure differs somewhat from that of Windows. Instead of having a physical drive like the C: drive at the base of the filesystem, Linux uses a logical filesystem. At the top of the filesystem structure is the / directory, often referred to as the root of the filesystem, resembling an upside-down tree (see Figure 1-4). It's important to note that this is different from the root user. Although these terms may initially be confusing, they will become easier to distinguish as you become more familiar with Linux.

<a href="https://imgbb.com/"><img src="https://i.ibb.co/SRNkKKT/Untitled.png" alt="Untitled" border="0"></a>

The root **(/)** of the filesystem is at the top of the tree, and the following are the most important subdirectories to know:

- **/root**: The home directory of the all-powerful root user
- **/etc**: Generally contains the Linux configuration files—files that control when and how programs start up
- **/home**: The user's home directory
- **/mnt**: Where other filesystems are attached or mounted to the filesystem
- **/media**: Where CDs and USB devices are usually attached or mounted to the filesystem
- **/bin**: Where application binaries (the equivalent of executables in Microsoft Windows or applications in macOS) reside
- **/lib**: Where you'll find libraries (shared programs that are similar to Windows DLLs)

**Finding Yourself with pwd**

To find your current directory in Linux, you can use the `pwd` command. Enter `pwd` in your terminal to see your current location within the directory structure:

```
kali > pwd
/root

```

In this case, the output shows that you are in the root user's directory. Since you logged in as root when you started Linux, you have administrative privileges and can access and modify system files and directories.

**Checking Your Login with whoami**

If you've forgotten whether you're logged in as root or another user, you can use the `whoami` command to see which user you're logged in as:

```bash
kali > whoami
root

```

If I had been logged in as another user, such as my personal account, `whoami` would have returned my username instead, as shown here:

```bash
kali > whoami
OTW

```

**Navigating the Linux Filesystem**

Changing Directories with `cd`

To change directories from the terminal, use the change directory command, `cd`. For example, here’s how to change to the `/etc` directory used to store configuration files:

```bash
kali > cd /etc
kali:/etc >

```

The prompt changes to `root@kali:/etc`, indicating that we’re in the `/etc` directory. We can confirm this by entering `pwd`:

```bash
kali:/etc > pwd
/etc

```

To move up one level in the file structure (toward the root of the file structure, or `/`), we use `cd` followed by double dots (`..`), as shown here:

```bash
kali:/etc > cd ..
kali > pwd
/
kali >

```

This moves us up one level from `/etc` to the root directory, but you can move up as many levels as you need. Just use the same number of double-dot pairs as the number of levels you want to move:

- You would use `..` to move up one level.
- You would use `../..` to move up two levels.
- You would use `../../..` to move up three levels, and so on.

So, for example, to move up two levels, enter `cd` followed by two sets of double dots with a forward slash in between:

```bash
kali > cd ../..

```

You can also move up to the root level in the file structure from anywhere by entering `cd /`, where `/` represents the root of the filesystem.

**Listing the Contents of a Directory with ls**

To see the contents of a directory (the files and subdirectories), we can use the `ls` (list) command. This is very similar to the `dir` command in Windows.

```bash
kali > ls
bin  initrd.img  media  run  var
boot  initrd.img.old  mnt  sbin  vmlinuz
dev  lib  opt  srv  vmlinuz.old
etc  lib64  proc  tmp
home  lost+found  root  usr

```

This command lists both the files and directories contained in the directory. You can also use this command on any particular directory, not just the one you are currently in, by listing the directory name after the command; for example, `ls /etc` shows what’s in the `/etc` directory.

To get more information about the files and directories, such as their permissions, owner, size, and when they were last modified, you can add the `-l` switch after `ls` (the `l` stands for long). This is often referred to as long listing. Let’s try it here:

```bash
kali > ls -l
total 84
drw-r--r-- 1 root root 4096 Dec 5 11:15 bin
drw-r--r-- 2 root root 4096 Dec 5 11:15 boot
drw-r--r-- 3 root root 4096 Dec 9 13:10 dev
drw-r--r-- 18 root root 4096 Dec 9 13:43 etc
--snip--
drw-r--r-- 1 root root 4096 Dec 5 11:15 var

```

As you can see, `ls -l` provides us with significantly more information, such as whether an object is a file or directory, the number of links, the owner, the group, its size, when it was created or modified, and its name.

Some files in Linux are hidden and won’t be revealed by a simple `ls` or `ls -l` command. To show hidden files, add a lowercase `-a` switch, like so:

```bash
kali > ls -la

```

If you aren’t seeing a file you expect to see, it’s worth trying `ls` with the `-a` flag. When using multiple flags, you can combine them into one, as we’ve done here with `-la` instead of `-l -a`.
