# Navigating the Filesystem:
## Table of Contents:

## Basics:
- I keep seeing the Unix/Linux directory hierarchy all over the place and I kinda feel like certain files and things need to be located in certain parts of this hierarchy, but I'm just not sure!! 
- I am familiar with the basic usage of most commands used to navigate the file system so I'll skip most of these and maybe focus on just focus on less common topics.
- Let's start with how the typical vanilla Linux filesystem is organized (There might be extra directories based on the distribution and the availability of a Desktop). The diagram is generate by the popular **`tree`** command (that's not really part of Linux, I believe. You gotta install it):
```bash
/
├── bin
├── boot
├── dev
├── etc
├── home
├── lib
├── media
├── misc
├── mnt
├── opt
├── proc
├── root
├── sbin
├── tmp
├── usr
└── var
```
- It shows that a typical Linux filesystem is organized into an upside-down tree. The root of the tree is actually called the **root directory** of the filesystem and is denoted by a slash **`/`**. Under the root are a dozen subdirectories, some of which are found in most Linux/Unix systems and store specific  data and serve specific uses.
- The **`/home`** directory for example contains directories for system users except for the root user which you cannot cd into unless you are root. For some reason you cannot use `sudo` with `cd`.
- The following table shows some linux directories under the root directory and what they are good for:

| Directory | Usage |
| --- | --- |
| **`/bin`** | Contains common Linux user commands such as `ls`, `chmod`, and `sort`. |
| **`/boot`** | It has stuff that have to do with the kernel and booting and all that. |
| **`/dev`** | Contains device files such as terminal devices (**`tty*`**), floppy disks (**`fd*`**), hard disks (**`hd*`** or **`sd*`**), and RAM (**`ram*`**). |
| **`/etc`** | Contains administrative configuration files. These are usually plaintext files and can be edited by any user with administrative privileges. |
| **`/home`** | A user with login has a subdirectory whose name is the user's username in this directory. The root user is an exception. Its home directory is `/root`. |
| **`/lib`** | Contains shared libraries needed by programs in `/bin` and `/sbin` to boot the system. |
| **`/media`** | Used for automounting devices especially removable ones like USB sticks. If you connect a USB stick called `usb1` to the system, a file called `/media/usb1` will be used to mount it. |
| **`/misc`** | Can be used to automount filesystem upon requests. |
| **`/mnt`** | Another mounting location, but seems to have been largely replaced by `/media`. People still use it to mount remote or local filesystems. |
| **`/opt`** | Used to store add-on application software. |
| **`/proc`** | Contains information about system resources (abbr seems to stand for process). |
| **`/root`** | This is root user's home directory. For security reasons, it's not placed under the `/home` directory. |
| **`/sbin`** | Contains administrative commands and process daemons. |
| **`/tmp`** | Contains temporary files used by applications. |
| **`/usr`** | "Contains user documentation, games, graphical files (`X11`), libraries (`lib`), and a variety of other commands and files that are not needed during the boot process. The `/usr` directory is meant for files that don’t change after installation (in theory, `/usr` could be mounted read-only)." |
| **`/var`** | Contains data used by various (hence `/var`) applications. For example, `/var/ftp` contains files used by an ftp server, `/var/www` files used by a web server, `/var/log` contains all system log files. This directory is meant to be used for files that change frequently. On server computers, `/var` can be made a separate filesystem based on a filesystem type that can be easily expanded. |

## Basic Filesystem Navigation Notions:
- Only the root directory start with a slash. Imagine your home directory has the following files:
```
web/			files/ 		books/
```
- If you type `cd web`, you will sed into the `web/` (`/home/yourUsername/web`) directory. If you type `cd /web`, you will cd into a directory `/web` under the root directory if `/web` exists. 
- basically, when you use a directory/file starting with a slash, you mean you wanna use its full path. Not starting with a slash signifies that you want to use a relative path to file. 
- Tilde **`~`** denotes the current user's home directory, it is basically equivalent to `/home/<currentUser>`.

 
## Metacharacters and Operators:
- The shell has special characters called **metacharacters**. These allow you to move, copy and remove not just one file at a time, but as a group of files that share certain patterns in their names. These are something like regex but with limited capabilities. 

### File-matching Metacharacters:
- The basic **file-matching metacharacters** are:

| Metacharacter | Matching behavior |
| --- | --- |
| **`*`** | Matches any number of any characters. |
| **`?`** | Matches any one character. |
| **`[...]`** | Matches one of the characters between brackets including hyphenated ranges of characters or numbers such as `[a-z]`. |

- You might need to differentiate between **`*`** which matches zero or more any characters and **`?`** which matches exactly one any character. **`f*`** matches `f`, `fa`, `f.py`, `fpoo`, etc., but `f?` exactly matches a two-character word, something like `f1`, `fa`, `fz`, but nott `f`, `fat`, `fnp`.

### File-redirection Metacharacters:
- **File-redirection metacharacters** allow you redirect standard IO to and from files (similarly and as opposed to pipes which redirect the standard output of command to the input of another command). These include:

| Metacharacter | Action |
| --- | --- |
| **`<`** | Makes the contents of a file the input of a command. Most commands that take input can already handle this so there is no need for input redirection. |
| **`>`** | Instead of printing to stdout, prints to a named file. |
| **`2>`** | Outputs error to file instead of standard error. |
| **`&>`** | Directs both stout and stderror to the named file. |
| **`>>`** | Appends output to the file content instead of overwriting it. |

### Here Text (Here Document):
- **Here text** (also called **here document**) is a commonly used pattern/convention among Unix heads and can be great for UI as part of shell scripts and shell programs. I haven't tested this yet, but it might allow the user of a shell script to pass input to the script using conditionals and what not.
- To be honest I don't know what exactly happens in a *here text*, but I kinda understand how it works in certain situations as the following one shows. The user is asked to enter a value. That value is used as a part of a script within the file and then is given back in some form as a part of the output (Oh the humanity!!):
```bash
#!/bin/sh
echo -n 'what is the value? '
read value
cat <<EOF
The value is $value
EOF
```
- A *here text* also allows you to start a command and then input stuff to it from stdin (I don't know how useful this is):
```sh
sed -n 's/foo/bar/g p' <<EOF
foo foo
foo fooo
fooo
baba
foo
EOF
``` 
- The useless script above selects words containing `foo` and replaces each `foo` with `bar`.

### Brace-expansion Characters:
- This is another cool shell construct. It is self-explanatory but also extremely powerful as the following example shows:
```bash
touch {1,2,3,4}.jpg # Create files 1.jpg, 2.jpg, 3.jpg, and 4.jpg
```
- You can also create files across multiple directories and using complex patterns as in:
```sh
touch {am,sam,bam}/someCrazyStuff.py
```




## Listing Files and Directories:
-

## File Permissions and Ownership:
-

## Moving, Copying and Removing Files:
-
