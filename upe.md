# THE UNIX PROGRAMMING ENVIRONMENT

- This is an old book but it's held in high esteem by many. Seeing how many linux commands resemble those of unix and having enjoyed the C book, co-written by Kernighan, I thought I'd skim over this one and see what it still has to offer.
- These are my notes on what I understand of the book and what I manage to read of it.
- I will use macOS 10.x to test the scripts provided in the book and probably report if they are still relevant.
- I might skip the few commands and things I already know about how to use the terminal.

- [Preface/Trivia](#preface/trivia)
- [Chapter 1: Unix for Beginners](#chapter-1-unix-for-beginners)
- [Chapter 2: The File System](#chapter-2-the-file-system)
- [Chapter 3: ](#chapter-3-)
- [Chapter 4: Filters](#chapter-4-filters)
- [Chapter 5: ](#chapter-5-)
- [Chapter 6: ](#chapter-6-)
- [Chapter 7: ](#chapter-7-)
- [Chapter 8: ](#chapter-8-)
- [Chapter 9: ](#chapter-9-)

## PREFACE/TRIVIA:
- Unix was created in 1969 in Bell Labs and rewritten in C in 1973. In 1974, it was licensed to universities (the so called BSD).
- Unix is successful because of its portability, the fact that it's written in C, a high level language. 
- The **Unix philosophy** states that the relationship among different programs is what makes a great system. Even trivial programs can be combined to create great software, if they work well together.
- An important theme of the book, is how programs work together and how they are used to create other programs, and how they fit in the environment where they run.
- Use existing unix tools instead of writing programs that the same task clumsily. 

## CHAPTER 1: UNIX FOR BEGINNERS
- Unix is a time sharing operating system _kernel_. A **kernel** is program that let users run their programs, controls resources and provides a file system. Time sharing means that the the use of the CPU is rapidly switched among different users and programs that seem to be running simultaneously. 
- Unix can also refer to the kernel along with other system tools such as editors, compilers, command languages.. etc.

### 1.1 Getting started:
- Unix is _full duplex_, meaning when you type on the keyboard, characters are sent to the system which _echoes_ them (send them back) to the screen. This echoing functionality is turned off when you type a password, for example. 
- Most keyboard characters are printing characters, but there are also control characters such as the **RETURN** key which signifies the end of input line. Control characters are invisible characters that control some aspect on the terminal.
- Return has a key of its own, but most other control characters are typed by holding the control (CTL/CTRL/ctl) character and some other character. RETURN, for example, is is composed of ctl-m, backspace is ctl-h..etc (I don't know if these). Other important control keys include, BREAK and DELETE, which stop running programs immediately. This is the familiar ctl-c.
- A few useful commands in a typical session in 1984 would include the following commands:
	* `date`: shows date and time.
	* `mail`: shows mail.
	* `mail ahmed`: sends mail to specified recipient.
	* `d`: delete mail inside mail
	* `ctl-d`: exit mail inside mail.
	* `login`: prompt you to login using the credentials you provide.
	* 'logout': can also be done using `ctl-d`.
	* `who`: shows who is logged into the system. `tty...` stands for the teletype (old word for terminal). It's the system name for a certain connection (user).
	* `who am i`: tells you which user you are.
	* `ctl-u`: kills a line (very useful, INDEED).
	* WHat the fuck are you talking about.
- **Type ahead:** you can type while the terminal is outputting text. Your input gets mixed up with the output, but the system still interprets it correctly. COOOL!!!
- To stop a program, I use mainly `ctl-c` and sometimes `ctl-z`. I still don't know the difference as of right now!
- Chatting with other users of the same system can be done with e.g. `write Ahmed`. To exit chat, type `ctl-d`.
- **The manual**: The book doesn't say much more than what I already know `man`. I need to work on this on my own.

### 1.2 Files and common commands:
- Information is stored in files which have names, content, locations and metadata such as who owns them.
- **ed 101:** ed is an old text editor, much older than both vi and emacs. Basic commands of ed:
	* `a`: start adding text.
	* `.`: By itself on a line. Stop adding text.
	* `w <filename>`: save to named file
	* `q`: quit ed.
	* `1,$p`: prints to screen contents of file from line 1 to last line
	* `g/<regular-expression>/p`: same as grep.
- Weird thing is that when I open a file with ed, I can't see any of the text previously written into it.
- `ls`, which is used for listing files in a directory has many options. An interesting one is `-t` which lists files by time they were last modified and not alphabetically. `-l` list long info lines which include who owns the files and where they were created. `-r` reverses the order files are listed in. Options can be combined so `ls -lrt` lists long descriptions of files according when they were last modified in reverse order.
- Options have to precede file names. 
- Options can be combined or left separate but this is not always the case.
- Options in unix are not uniform between different commands, so `-l` means something in one command and a totally different thing in another commands.
- `cat` and `pr` print the contents of a file or several files to screen. `pr` cut content into pages and put dates on top of pages
- `lp` and `lpr` are used for printing. I don't know if this is still relevant!
- **File path manipulation:** 
	* `mv`: moves a file from one location to another or simply renames a file, or replace target with the moved file.
	* `cp`: copy the contents of a file into another.
	* `rm`: removes an existing file a warning a files doesn't exist.
- file names should only be composed of underscores, letters and numbers...etc. Periods are used in extensions...
- More **file processing** commands::
	* `wc`: spits out the count of lines, words and characters in a file or a group of files.
	* **Grep:** `grep` which is used to grab lines containing patterns originates from the ed command `g/<regular-expression>/p`. Used with the option -v, it grabs the lines not containing the provided regular-expression. ch re on grep is on [chapter 4](#chapter-4-filters).
	* `sort` sorts a file line by line alphabetically. There are many options to make sort more useful.
	* `cmp file1 file2`: Used to check if two files are the same. Works on any type of file. It only shows in which line the first difference occurs.
	* `diff file1 file2`: used ti tell the differences between text files and where they occur. 

### 1.3 Directories:
- When you login, you are in your **home directory**. The **working directory** or **current directory** is the one where you are working now.
- When a file is created, it by default resides in the current directory.
- `pwd` tells you where you are located in the file system tree.
- A *pathname* is the full name of a file starting at the root, something like `/usr/you/junk`.
- When you try to run a command, the system looks inside the current directory and if it doesn't find it, it searches `/bin`, and if not then `/usr/bin`.
If you run `ls /bin /usr/bin`, you get a list of commands like `mv`, `cp`, `ed` .. etc. 
- `mkdir` is used to create a new directory and `rmdir` removes an empty directory.
- `cd` is used to changree directories. By itself it takes you to home directory. `cd ..` takes you one level back up towards the root. `cd /usr` takes you down to the user directory.

### 1.4 The shell:
- Between you and the kernel sits a program called the **shell** or command interpreter. The existance of such a program separating you from the kernel has some advantages including:
	* Filename shorthands: batches of files can be processed all at once using certain patterns.
	* Input-output redirection: You can send your output to a file or gran input from a file or directly make the output an input to another program
	* Customizing the environment: you can create your own tools.
- **Filename shorthands**: These are the basic shorthands found in SQL, regular expressions .. etc. `*` means any string of characters can be used for example to grab all files that start with 'ch' as in `ch*`, or all the files that end with '.pdf' as in `*pdf`. It's the shell that interprets `*` as a string of any length in the example `ls *.pdf` and not `ls`. 
- Other important filename shorthands include:
	* `[...]`: matches a single one of any of the characters inside the bracket and this can be done in one of the following notations:
		1. [01234546789]
		2. [1-46-9]
		3. [a-z]
	* `?` is similar to `*` but it matches any single character instead of a sequence of characters.
- **IMPORTANT:** Patterns only match existing filenames, so the following expression doesn't work:
`mv ch.* chapter.*`
- `*` is also very useful in path names. Think of the possibilities and what you can do with something like `usr/*/calendar` or `usr/*` etc.
- To neutralize the special functionality of `?` and `*` precede them with backslashes as in `\*` and `\?`. Now * and ? are just regular characters.
- **Input-output redirection:** The terminal doesn't have to always be the input, output or both of a command. The results of a command such as `cat` can be output into a file. Here are some examples of conventions for redirecting input-output that are part of the shell:
	* `cat f1 f2 f3 > temp.txt`: Instead of catting the contents to the terminal, they are catted to the file temp.txt. `>` means put the output of the command into temp.txt. If the file exists, it overwrites it and if not it creates a new file with the name provided.
	* `cat f1 f2 f3 >> temp.txt`: Instead of overwriting the content of temp.txt, it adds the content to the end of it. 
	* `mail a b c < letter.txt`: `<` means input the contents of letter.txt into a, b and c.
- The use of temp files allows you to do many nifty things.
- **Pipes:** or pipelines connect the output of a program to the input of another program without the need for temporary files. This is a fundamental contribution of the unix system. `who | sort`, for example, prints a sorted list of logged in users and `ls | sort -r` lists a revers-sorted list of files, and `ls | grep .pdf` lists file whose names include the pattern '.pdf'.
- Pipes are versatile and anything that can be input into, or output that can be output from the terminal can be done to a pipe. You can chain  as many programs in a pipe as you wish. Programs run simultaneously in a pipe, so they allow for interactivity. 
- I feel like a pipe including commands with optional filenames and arguments might cause confusion. Anyways, creating efficient pipes might need some training and getting used to.
- **Processes:** You can string two programs together using a semicolon. `date; who` prints the date and list logged users. These programs run in sequence. Using `&`, the user can go ahead and start another program while the first program is still running. So by using `wc upe.md &`, you can start another program even thiugh wc is still running. When you use `&`, a number is printed to the screen. That number is **process-id** of the running process. A process is different from a program; it's an instance of a program. Two instances of the same program are two different processes. With an `&` at the end of a pipe, only the id of the last process is printed out. The command `wait` waits until all the process started with `&`.
- The process-id can be used ot kill the process with the command `kill <process-id>`.
- `ps` is used to list all the running processes. PID is the process-id. TTY is the terminal associated with the process and TIME is how much time the process used.
- Processes might have parents and children (I won't go into this headache here).
- **nohup**: `nohup <command> &` allows you to keep running a program after you log out of the terminal.
- `nice <command> &` keeps running without taking too much resources.
- A job can also be **scheduled** through the use of `at`. For example `at 4pm echo ahmed`. I am having a hard time running this command, because of time formatting. I keep getting the message "garbled time". I'll come back to it when I have an Internet connection.
- **Environment Tailoring:** Unix can be customized to meet one's needs. The command `ssty` changes the kill line functionality from `ctl-u` to `@` through the following command `stty kill @`. Once you logout or turn off the terminal, this change disappears, but to make it persist, you can use the .profile or bash_profile file. This can be discussed somewhere else. Some commands can be added to the profile, so that every time you login, some information is printed first like how busy the system is ..etc.
- **Shell variables**: are shell properties that can be controlled and set by the user. They include
	* **PS1**: or prompt string and it can be set to anything. It usually has the user name and `$`. This can be icluded in .profile or .bash_profile as in `PS1="Yeas dear?"`.
	* **PATH:** This is the most useful shell variable. It sets where the shell should look for commands. By default, the sell looks for commands in the current directory, then in bin, then in /usr/bin. This sequence of directories is called the *search path* which is stored in the shell variable PATH. PATH has a strange syntax where directories are separated by colons. This an example of a path that includes the current directory, /bin, /usr/bin and /usr/bin/games:
		`PATH = .:/bin:/usr/bin:/usr/bin/games`
	- There is also the strange {$PATH}, which I don't understand. More search paths can be added to the first one in this manner `PATH=$PATH:/usr/news`. This new search path is added to the value of PATH which is denoted here by $PATH. The value of any shell variable is obtained by preceding it with `$`.
	-  `.` means the current directory, and a null value of PATH also means current directory.
	- You can create your own commands, collect them in a certain directory and add their search path to your PATH.
	* **HOME**: denotes the home directory.
	* **TERM**: is used by editors to manage screen and tells the editor what type of terminal you have.
- You can have your own variables to store different information such as a certain path,
 so `g=/usr/bin/games` allows you to do something like `cd g`. `g` here stores the path to said directory.
- `export` tells the shell that other programs can use sell or personl variables. This can be done in two ways:
	1. `PATH = .:/bin:/usr/bin:/usr/bin/games`: where the variable PATH is also defined and then exported.
	2. `export PATH`: where the previously defined variable is exported.
- You can create your own commands by packaging existing commands. This can create some very powerful tools.

### 1.5 The rest of the unix system:
- There is much more functionality in the unix system that's explained exhaustively in the manual. I need to learn more about how to use the manual. 

## CHAPTER 2: THE FILE SYSTEM

- Everything in the Unix system is a file. To succeed using unix commands, one need to get familiar with its file system and how files are manipulated, structured..etc. 

### 2.1 The basics of files:
- Files are just byte sequences. The meaning of those bytes depends solely on the programs that interpret them. The system doesn't care what they mean or what structure they have.
- The `od -cb <filenames>` commands print the bytes of a file as characters and their octal values. od stands for *octal dump*. This commands prints the byte content of a file, but doesn't print the end of the file. The kernel keeps track of the length of the file, so the end of the file is signaled by the fact that there is no more bytes to print out.
- Programs retrieve the data in a file through a **system call** (OS voodoo) called ***read***. Each time **read** is called it returns the next part of the file and also returns the number of bytes it reads. When, there are no more bytes, it returns zero, signaling the end of the file. Returning zero is interpretation-independent and a good way of telling the end of file without returning a special character. 
- _More voodoo. I don't see the point of cat and cat -u_

### 2.2 What is in a file? 
- The format or type of a file is determined by the program that interprets it. The file system and kernel have no idea what the type is, but it can make an educated guess through the command `file <filename>`.
- To determine the type of a file `file` reads the first few hundred bytes of the file to tell its type. Reading the extension of the file only is not reliable. Some files have system-dependent magic numbrs that determine their types. In binary files, it is octal 410. In C files, file command looks for something like "#include" to determine it's a C file. Reducing and eliminating differences between file types is an advantage for the unix system. This makes the commands more versatile and everything in unix is a file. A binary, a c file, a text file..etc., they can all be treated in the same way. It's up to specific programs to deal with distinct types of files.

### 2.3 Filenames and directories:
- Each running program, _process_, has a working directory. The process doesn't need to specify any pathname when it's interacting with the files residing in its working directory.
- `mkdir`: creates a new directory.
- `du`: prints a lit of directories and subdirectories. With the option `-a`, this command can print all directories and files as in `du -a`, and the output of this program can also be grepped in a pipe to look for a certain pattern as in `du -a | grep batata`. If you don't specify a certain directory, this command assumes you mean the current directory. 
- In pure unix directories are files. An octal dump of a directory reveals a mix of binary and textual data. The textual data have the names of files inside the directory and the directory's name. I tried this on mac OS but couldn't `od` a directory. 

### 2.4 Permissions:
- Different users have different permissions. The super-user has more permissions. The system recognize users by u-id's or user ids. Two different login ids can have the same u-id. Users are also classed into groups. The system determines what you are allowed to do based on your uid or group-id.
- The `etc/passwd` is the password file. It contains all the login information about each user of the system. (Tried this on mac but it didn't work exactly as described here but there is an etc/passwd and it has login information, but I couldn't see user id there). Each line in the password file follows the following format:
	`login-id:encrypted-password:uid:group-id:miscellany:login-directory:shell`
The shell field is often empty because you're using the default shell `/bin/sh`. Miscellany contains anything, usually name, email or phone. The second field has a an encrypted form of the password. Login encrypts the password you provide and compares it to this encrypted form and if they agree, you are allowed in (Encryption and security are vast topics beyond the scope of this book and notes).
- There are three kinds of permissions: read, write and execute. Each file has diffrent sets of permissions. As the owner of a file, you have a set of execute, write, read permissions. The group you belong to has its own set and everybody else has a different set. Listing files with the long option `ls -l` let you know what permissions a file has which are show in the first column of the list in the following form `-rw-r--r--` . The first letter in this string stands for whether the file is a directory or not. If it were, the file would look this way `drw-r--r--`. The next 3 characters `rw-`encode the permissions associated with the owner of the file. It can both write and read the file, but the file is not an executable, hence the last character is slash; it would be an `x` if it were an executable and the owner (has the permission to execute it!) The next 3 characters are the set of permissions of the group you belong too and the last 3 are for everybody else.
- The file `etc/group` encode group names, group ids and which users belong to which groups.
- _There is talk of `set-uid`, which I really don't understand. I will leave that_ to a course or a book on linux.
- `x` in a directory doesn't mean it's executable, but it means it can be searched. It's possible to give a user the permission set `--x`. This means the file can be accessed if the user knows its name, but can't run ls on it to see what other files reside in it. Conversely, `r--` means that one can run ls and see what files are in it, but can't use them.
- The **chmod** command changes permissions on a file. This can be done in two ways:
	* Octal numbers: This is done by adding a 4 for read, 2 for write and 1 for execute. E.g to change permission of a file to `-rw-rw-rw-`, we use `chmod 666 <filename>`.
	* Symbolic description: This is complicated, but `chmod +w` this would remove writing permissions from all users and `chmod -w` remove writing permissions from all. How to do it individually for the 3 sets of users, who knows??!!!
- Only super-users and the owner of a file can change its permissions. Even if somebody else allows you to write a file, the system won't allow you to change its permissions.
- If a directory is writable, users can remove files from it regardless of what these permissions these files have. To prevent certain users from deleting files from a directory, make the directory non-writable.
- Permissions and dates are not stored in files, but in a system structure called **i-nodes**.

### 2.5 Inodes:
- "A file has several components such as name, contents, and administrative information such as permissions and modification times. **Inodes** contain administrative information about the files as well as system information such as how long the file is and where it resides in disc.. etc.
- Inode has 3 times:
	* When contents were last modified.
	* When the file was last used (read or executed).
	* When the inode itself was last changed, such as when permission were set.
- These different times can be examined through different options of the `ls` command such as `-c` (_use time when file status was last changed_) and `-u` (_use time of last access, instead of last modification of the file_) along with the long format option `-l`. (Check the manual for ls options).
- Inodes are extremely important for the file system, because they are actually the files. Directory hierarchies are just a naming convenience. The system recognize files by their i-numbers, the numbers of the inodes that hold the files information.
`ls -i` reports the i-number of a file.
- `od` doesn't seem to work on macOS, but according to the book, 'od' on a directory prints a mix of binary and textual data where the text refers to file names and the binary refer to the i-numbers which are the only links between file names and their contents. A filename in a directory is called a *link* because it links a name a to an inode, and hence the data. The same i-number can appear in different directories. `rm` only removes the link to an inode. Only when the last link to a file is removed that the system removes the inode, and hence the file itself.
- When the i-number is zero, that means the contents of the file is still there and there might be another link to it.
- The `ln` command makes a link to an existing file with the syntax `ln <old-file> <new-file>`. This gives the same file two different names, allowing the same file to appear in two different directories. This seems like it creates a new file but all it does is creating a new name that points to the same file. Both names have the same i-number, which is COOOL!!! 
- The number that `ls -l` prints between the permissions and the owner of the file is the number of links to the file.
- All links to an inodes are equally important. If a change is made to a file through a link, the same change will appear in all other links since all links just point to the same inode or file. The file again is not removed until the last link to it is removed. 
- It's easy to lose files and unless the system is properly backed up, be careful when messing with links.
- While `ln` only creates a new link or pointer to the same old content, `cp` creates a new copy of the old file content.
- `mv` is a little similar to cp and ln. While cp creates new centent and while ln creates a new link to the same inode, mv keeps the content and the i-number, but only change the name of the file.
- ln, cp, mv can shuffle across directories, not just in the same directory. as in `mv /usr/junk.txt tuts/l_unix/unix_programming_env/`.

### 2.6 The directory hierarchy:
- Being familiar with the directory hierarchy of a system makes using it easier and more efficient. This structure might have changed. it's fairly different on the mac OS system, but I'll stick here to the original hierarchy.
- The top directory or the root is `/` and under it are the following directories:
	* `bin`: binaries such as who, ed, ls.
	* `boot`: bootstrap. Program that starts before the unix system is loaded.
	* `dev`: devices.
	* `etc`: et cetera or miscellany such as administrative files like passwd and groups.
	* `lib`: library. Parts of the C compiler.
	* `tmp`: temporary files. When programs are running, they create their own files or copy files they work on inside tmp. tmp is automatically cleaned when the system starts.
	* `unix`: The unix program, kernel..etc.
	* `usr`: user file system.
- This is very similar to linux and it's better to invest time getting familiar with that directory hierarchy.

### 2.7 Devices:
- Unix is or was special in that peripherals were run using files. There are references to these files inside the kernel that are converted to hardware commands that access these peripherals. Here is an example of what `ls -l /dev` would produce:
`crw--w---- 1 ahmed tty 16, 0 Aug 26 00:30 ttys000`
This information is derived from the inode of the device file. It contains its internal name which consists of its type, either `c` (character) or `b` (block) and a pair of characters, its _major_ and device numbers. The type is in the beginning of the permissions block produced by ls `crw--w----`.  Discs and tapes were block devices, while terminals and others were character devices. The major device encodes the device type, while the minor number registers its instance. In our example 16 is the major number while 0 is the minor one
- `tty` commands tells you which terminal you are using.
- Many other incomprehensible details about booting, mounting, dismounting.. etc.  
















