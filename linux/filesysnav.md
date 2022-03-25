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
```bash
touch {am,sam,bam}/someCrazyStuff.py
```
- You can also make use of ranges within these expansion braces as in:
```bash
touch {a..f}{1..3}
```

## Listing Files and Directories:
- Something I'm not very used to in the Mac shell is the different colors of different file types in the file lists printed with the **`ls`** command.
- When running **`ls -l`** where the option**`-l`** refers to long, a verbose listing of the directory's contents, you'd get a lot of information about the files listed, something like:
```
total 20
-rw-rw-r-- 1 am   am       0 Jan 21 15:22 b.html
-rwxrwxr-x 1 am   am      52 Jan 25 11:25 d
drwxrwxr-x 2 am   am    4096 Feb  7 02:38 w
lrwxrwxrwx 1 root root    25 Mar 19 04:31 web -> /var/www/html/hometwitter
``` 
- The top line **`total 20`** shows the total amount of disk space used by the current directory which is 20 kilobytes here. This might not necessarily reflect the actual size of the directory's contents! It actually represents the size of the file containing information about the directory and its content!!!!
- The first column to the left of each file's description describes the type and user permissions to the file. The first character of that string denotes the type of the file where:

| First character of file description | Meaning |
| --- | --- |
| **`-`** | A regular file |
| **`d`** | A directory |
| **`l`** | A symbolic link |
| **`b`** | A block device (_whatever this is!_) |
| **`c`** | A character device |
| **`s`** | A socket |
| **`p`** | A named pipe |

- The other 9 characters in that first column denote if the file is executable, readable or writeable by different types of users.
- The second column shows the number of hard links to the file. Column 3 shows the owner of the file and column 4 the group to which the owner belongs. 
- Column 5 shows the file size in bytes. 
- Column 6 shows the date when the file was last modified. This might appear in different formats based on the language and distribution.
- Column 7 is obviously for the name of the files. Notice that symbolic links also list the files being linked to.  

### Important `ls` options:
- The **`-a`** options lists hidden files as well. These configuration files or files required by the system.
- The **`-F`** option can also be useful and make navigating the filesystem easier. It appends a slash **`/`** to the end of directories, an asterisk to executables and an arobasque **`@`** to the end of symbolic link.
- The **`-t`** option lists files in the order in which they were most recently modified. 
- The **`--hide=option`** allows you to not list certain files. **`ls --hide=g*`** prevents the listing of all files that start with **`g`**.
- The **`-d`** option list information about the current directory instead of listing files in it.
- **`-R`** for recursive, lists all files in the current directory and files within subdirectories and their subdirectories. 
- **`-S`** list files by size.

### More Special Info Given in the Column 1:
- The first column can show more information for certain files such as:
	- Instead of showing an an **`x`** for an executable file, an **`s`** is shown. This means a regular user can run the command but the process run by the user is owned by the owning group or user. This is called a **`set GID`** or a **`set UID`** program. A special such program is the **`mount`** command. Any user can use `mount` to list mounted filesystem, but only the user can actually use it to mount the system!! ***I see the point but I need more explanation!! This is something like different levels of executability on the same file!!***
	- A **`t`** can appear at the end of a directory instead of **`x`** as in **`drwxrwxr-t`**. This means the so-called **`sticky bit`** is set. That means users can add files to the directory, but they cannot delete each other's files (***Can they modify each other's files?!***).
	- If **`set GID`** is assigned to a directory, any file created in that directory is assigned to the group of the directory.
	- If you see, a capital **`S`** or **`T`** at the end of column one in a directory, this means on the directory, this means it is assigned GID or the sticky bit, the execute bit hasn't been turned on. 
	- If you see a plus (**`-rw-rw- r--+`**) at the end of the permission bits, this means that the file has extra controls like **access controls lists** or whatever.

## File Permissions and Ownership:
- The first column in a list generated by **`ls -l`** contains 10 characters. The first character denotes the type of the file, while the other 9 characters are for so-called **permission bits**. There are sets of these permission bits, the first set is for the owner of the file, the 2nd set for the group the file is assigned to, and the third set is for everybody else. 
- Each set of permission bit is usually one of three characters: **`r`** for read permission, **`w`** for write permission, and **`x`** for execute permissions. a hyphen **`-`** as mentioned before means the permission is turned off.
- The permission bits means different things for different types of files. The following table explains these differences between regular files and directory files:

| Permission | File | Directory |
| --- | --- | --- |
| Read | See file content. | See files and subdirectories in directory. |
| Write | Modify, rename and delete file. | Add/remove files, subdirectories to directory. |
| Execute | Run the file as a program. | "Change to the directory as the current directory". Search the directory and execute programs from within it. See file metadata in that directory. |

### Changing File Permissions with **`chmod <numbers>`**:
- If you own a file you can change its permission as you please using the **`chmod`** (change mode) command. You can do this with letters or numbers. Let's start with numbers. Each permission is assigned a number as the following table:

| Permission | Value |
| --- | --- |
| **`r`** | 4 |
| **`w`** | 2 |
| **`x`** | 1 |

- The permissions a user has is the sum of these numbers. If a group has read and write permission, its number would be 6. A file with the permission number 777 can be read, execute and written by everyone. A user can have a permission value between 0 and 7. You got the gist:
```sh
chmod 755 someFile.py
```
- The command above results in the permissions string **`rwxr-xr-x`**. To remember things easily, `rwx` can be pronounced like *rawax*  or *rawcks*.
- `chmod` can be used with the **`-R`** option to recursively change permission on all subdirectories and files.
- The problem with this numerical approach though is that it changes all permissions to one unified level. What if you just want to add some permissions to all files but without changing other things. Let's say I want to make every file in a directory executable but keep the write and read permissions intact. I can't do this with numerical `chmod`. This is why we also have a letter `chmod`.

### Changing File Permissions with **`chmod <letters>`**:
- This approach allows for a more granular control of chmodding. It also preserves permissions that were already there.
- We add permissions using the plus sign **`+`** and remove them with the minus sign **`-`**. We change permissions for different types of users which can be represented by the following letters:

| Permission letter | Stands for |
| --- | --- |
| **`u`** | User (owner) |
| **`g`** | Group |
| **`o`** | Other |
| **`a`** | All |

- The chmodding is done as follows:
```sh
chmod ug+x someFile		# Make executable by owner and group
chmod a+x someFile      # Make executable for all users.
chmod go-w someFile		# strip off writing permissions for group and others
```
- This approach is obviously superior to the numerical one mainly because it allows for adding or removing permissions to/form existing permissions. It doesn't just overwrite everything. This approach is preferred when changing many files at once.

### Setting Default File Permissions with **`umask`**:
- When you create a file or directory different default permissions are provided based on the type of the file you create and type of user you carry yourself as, as the following table shows:

|  | Regular File | Directory |
| --- | --- | --- |
| Regular user | `rw-rw-r--` |`rwxrwxr-x` |
| Root user | `rw-r--r--` | `rwxr-xr-x` |

- These defaults values are determined by the **`umask`** value which is also set by default to **`0002`**. You can check this with typing:
```
$ umask
0002
```
- A regular file with the permissions value `666` has fully opened permissions, while a folder with permissions number `777` is considered to have fully opened. These fully opened permissions get masked by the `umask` value resulting in 775 for directories and 664 for files. Masking is equivalent to the bitwise operation **`fullyOpenPermissions & (~umask)`**.
- The `umask` value can be changed by running the `umask` command with the new value as an argument as in:
```sh
umask 000
```
- To have the new `umask` permanently in your system add it to your `.bashrc` file.  

### Changing File Ownership:
- You need to be a root user to be able to change the ownership of a file. This is the job of the **`chown`** command:
```sh
chown Simsim someFile
```
- The command above only changes the user, but you can change both the user and group with the following:
```sh
chown Simsim:Simsim someFile
```
- Our dear friend, the **`-R`** option can also be used with `chown` to change the contents of a directory recursively.

## Moving, Copying and Removing Files:
- You can move a file or a directory with the **`mv`** command. This command also allows you to  rename a file. Basically, this doesn't change the location of content but just give it a new name that the file system recognizes.
- The `mv` command has the the option **`-i`** which prompts the user if the destination name already exists. Some systems alias `mv` as `mv -i`. Something that seems very useful and can prevent disasters.
- **`cp`** command is for copying. To copy directories you must supply the **`-r`** option (to do so recursively). Another important option is **`-a`** which when used, the metadata of the copied file such as timestamp and permissions are maintained. Without it, new timestamps and different permissions are given to the copy. The permissions are determined by the `umask`. The **`-i`** option does the same thing it does with `mv`.
- **`rm`** removes files and directories. Directories can be removed when the **`-r`** option is present. It can removes files and multiple levels of subdirectories (it might prompt you when exiting subdirectories). There is another Directory removal command **`rmdir`** but this can only remove empty directories. `rm` doesn't remove dot files or directories unless, as mentioned before, `-r` is present. 
- **`-f`** option removes anything in its way without prompting. Running the following command will wipe your whole system:
```sh
rm -rf /
```
- If any of the commands above is aliased and resulting in many prompts, you can:
	- Use the `-f` force option.
	- Precede the command with a backslash as in `\mv` to deactivate the aliasing.
	- Use the `-b` option so the system creates a backup of the old file before the new file is placed in its place. 