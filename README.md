# THE UNIX PROGRAMMING ENVIRONMENT

This is an old book but it's held in high esteem by many. Seeing how many linux commands resemble those of unix and having enjoyed the C book, co-written by Kernighan, I thought I'd skim over this one and see what it still has to offer. These are my notes on what I understand of the book and what I manage to read of it. I will use macOS 10.x to test the scripts provided in the book and probably report if they are still relevant. I might skip the few commands and things I already know about how to use the terminal.

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
- `du`: prints a list of directories and subdirectories. With the option `-a`, this command can print all directories and files as in `du -a`, and the output of this program can also be grepped in a pipe to look for a certain pattern as in `du -a | grep batata`. If you don't specify a certain directory, this command assumes you mean the current directory. 
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

## CHAPTER 3: USING THE SHELL
- The shell is the program that interprets the requests you make to the system. A lot can be accomplished with the shell alone.

### 3.1 Command line structure:
- The simplest command is made of one word such as `ls`, `who` or `date`. A command is usually terminated with new line, but it can also be terminated with a semicolon, so multiple commands can be present in the same line as in `date; ls; who`.
- Because of operator precedence rules, a semicolon may introduce some issues especially in pipes. In `date; who | wc` you might want to get the word count of the output of both who and date, but you only get those of date. `|` has a higher precedence than `;`. To solve this problem, group who and date with parentheses as in `(date; who) | wc`. This gives the word count of both date and who. Now, the outputs of the two commands "are concatenated into a single stream that can be sent down a pipe."
- `tee` saves the a pipe's data into a file and its output as in `(date; ls) | tee save.txt | wc`. In this example, tee dumps data in save.txt and wc receives that same data. (_I don't know what's the point of this_).
- `&` is another important command terminator. `long-running-command &` tells the shell to not wait for the command to end. It can keep running in the background while the user can keep using other commands of the shell. This terminator can be used with such commands as `sleep` ot do interesting things. `sleep` is a command that waits a specified time in seconds before it exists. It can be used to create reminders (_asynchronous stuff??!!!_). The following example should be self-explanatory: `(sleep 5; echo tea is ready) & echo tea will be ready in 5 seconds`. The precedence of `&` is higher than that of a semicolon, so the parentheses are essential in this example.
- Since pipes are commands, `&` also applies to pipes.
- **Command arguments:** commands can take arguments which are words separated by spaces or tabs. These arguments are usually files. Commands also take options which are arguments starting with minus signs such as `-c`.
- **Special characters** such as `>`, `<`, `|` ,`&`, `;` are not arguments but are used to control how the shell run commands.

### 3.2 Metacharacters:
- These are special characters that are interpreted differently by the shell. The asterix `*` is the most commonly used metcharacter. It is used to search a directory and stands for any string of characters. Filename-matching characters, though, ignore files starting with dots, to avoid problems with `.` (current directory) and `..` (parent directory).
- There are many metacharacters. To prevent the shell from interpreting these characters as metacharacters, enclose them in single quotes (`$ echo '****'` instead of `$ echo ****`) or precede them with backslashes (`$ echo \*\*\*\*` instead of `$ echo ****`). Single quotes are less problematic than double quotes and the user needs the keep the distinction between the two. Backslashes are still the easiest way to avoid all confusion. 
- Another vital use of single quotes is that they allow for including newline in the printed text.
```sh
$ echo 'hello
> world'
```
will include a new line after hello.
- **Secondary prompt** appears when you type newline inside quotes or double quotes. It is printed by the shell in a new line when it expects you to type more to complete a command. It is stored in the shell variable PS2 and can be changed and stored in .profile through this command:
```sh
PS2=">>>"
```
- A backslash at the end of a line allows you to continue typing on the next line. The backslash is discarded in general but is retained inside single quotes:
```sh
$ echo abc\
> ddd\
> zz
abcdddzz
```
while:
```sh
$ echo 'abc\
> ddd\
> zz'
abc\
ddd\
zz
```
- `#` is used for comments.
- Some super nerdy talk of why `echo` and why not the newline supplemented by echo is a good idea!! 
- **Shell metacharacters:**

| Character | Description
| --- | --- |
| `>` | *prog > file* direct standard output to *file*
| `>>` | *prog >> file* append standard output to *file*
| `<` | *prog < file* take standard input from file *file*
| `|` | *p1 | p2* connect standard output of *p1* to standard input of *p2*
| `<< str` | *here document*: standard input follows, up to next *str* on a line by itself
| `*` | match any string of zero or more characters in filenames
| `?` | match any single character in filenames.
| `[ccc]` | match any single character from *ccc* in filenames.
| `;` | command terminator: `p1;p2` does `p1`, then `p2`
| `&` | like `;` but doesn't wait for `p1` to finish
| ``...`` | run command(s) in ...; output replaces \`...\`
| `(...)` | run command(s) in ... in a subshell
| `{...}` |run commands(s) in ... in current shell(rarely used)
| `&1, $2, etc.` | $0...$9 replaced by arguments to shell file
| `$var` | value of shell variable `var`
| `${var}` | value of `var`; avoids confusion when concatonated with text;
| `\` | *\c* take character *c* literally, newline discarded
| `'...'` | take ... literally
| `"..."` |  take ... literally after `$`,`` `...` `` and `\` interpreted
| `#` | if `#` starts word, rest of line is comment
| `var=value` | assign to variable `var`
| `p1 && p2` | run `p1`, if unsuccessful, run `p2`
| `p1 || p2` | run `p1`, if unsuccessful, run `p2`

### 3.3 Creating new commands:
- When a sequence of commands are run frequently, it would be a good idea to package them in a new command.
- The book provides an example of a program that counts how many users are logged in the system. The following pipe does that job:
```sh
who | wc -l
```
This pipe is written into a file *nu*using an editor or simply echoed into that file:
```sh
echo 'who | wc -l' > nu
```
The shell, named `sh` in unix, is then used to to run the program. It takes its input from the file *nu* as in:
```sh
sh <nu
```
It's ridiculous to type `sh` every time you want to run this type of file. This file needs to be turned into a *shell file.* Generally speaking, if a file is executable and contains text, the shell assumes it is a shell file. Change permissions of the file to make it executable:
```sh
chmod +x nu
```
Now nu is a shell command that can be run just by typing `nu`.
- The way the shell runs `nu` is to create a new shell process as if `sh nu` were typed. This shell process is called a *sub shell* (a shell process invoked by the current shell).
- The new command is only available in your current directory. To make it available to your system or your repertoire, put it in a specific directory and add it to your PATHin your `.profile` or `.bash_profile`. There are different conventions for this. The book suggests a certain `usr/you/bin`.
- To create a temporary sub shell, you can use the following command:
```sh
sh # This creates a sub shell that is different from the parent shell
```
You can do work in this shell and when you are done, just run the command `exit`. _(The book mentions `ctl-d`, but that doesn't work in my machine.)_

### 3.4 Command arguments and parameters:
- How do we supply our new command with arguments such as files to work on? The book provides the example of a command `cx` that make files executable. Arguments are denoted by a sequence of symbols $1 to $9 where $1 stands for the first argument, $2 stands for the second argument and so on until $9. If our shell file contains the following command `chmod +x $1`, then  the command will work on the first argument, and the sub-shell replaces `$1` with `nu` in the command `cx nu`.
-  How to handle **more than one argument?** You can do the naive `chmod +x $1 $2 $3 $4 $5 $6 $7 $8 $9`. This will work if you have less than 9 files. The unused arguments will be empty strings. If you have more than 9 arguments, the program fails. This can be solved by the the following nifty convention: 
```sh
chmod +x $*
```
This way, your command will work no matter how many arguments you have.
- Files are not the only type of arguments you can add to your commands. Consider the example provided by the book where `grep` is used to grap text from files:
```sh
echo 'grep $* usr/lib/phone_book' > search_phones
```
This command returns all lines containing the grepped pattern. There is only one little problem. When the grepped pattern is made of more than one word, it becomes more than one argument. Our command gets two arguments instead of one resulting in the failure of the program. To fix this problem, `$*` needs to be placed inside double quotes. While single quotes doesn't allow any character to be interpreted by the shell, double quotes allow `$`, `\`, and `...` to be interpreted instead of just read as regular characters. The correct command then becomes:
```sh
echo 'grep "$*" usr/lib/phone_book' > search_phones
```
Now, the strings that replaces `"$*"` will be a single argument even if it contains a blank.
- Argument `$0` is the name of the program itself. (_I saw this in the C book_).

### 3.5 Program output as arguments:
- Arguments can be generated explicitly, with metacharacters like `*` or they can be generated by running programs. The output of a pgromar itself can be the argument to a command. You can get the output of a program by enclosing it in back quotes \`...\`'. The following  command:
```sh
echo the date is
```
prints the date, not the string "date". The command `date` runs and its output becomes one of `echo`'s arguments.
- Imagine a command that would grab a title of a to be created file from another file:
```sh
touch `cat lala`
```

### 3.6 Shell variables:
- Shell variables are also called _parameters_. They include variables such PATH and HOME that we saw earlier. They are different from positional arguments such as $1 and $2.
- Shell variables can be created, accessed and modified. Here is an example of modifiying a shell variable, the PATH:
```sh
PATH=:/bin:/usr/bin
```
There must be no spaces around the equal sign and value must be a single word with no blanks. If it contains shell metacharacters, it should be enclosed in single quotes.
- The value of a parameter can be accessed by preceding it with a dollar sign.
- Conventionally, special shell variables are in uppercase such as PATH and HOME. You can create your own shell variables to store paths or strings and give them lowercase names.
- `set` prints out all shell variables.. There are way too many. To print specific variable values use echo as in `echo $PATH`.
- The value of a variable is associated with the shell that created it. It is not passed to its child shells. 
- A shell file (dicussed at 3.3) can't change the variables of their parent shell. But there is a workaround where commands in files are run using the dot command `.` Let's say we have a file games_dir containing a command to add the games directory to the PATH variable. Running this file can be done as follows:
```sh
. games_dir
```
games_dir need not be made executable, because the dot comand doesn't actually execute the commands in the file. It merely read them and the current shell executes them. The disadvantage is that the file can't have arguments $1, $2.. etc. 
- There is a different way to change a shell variable by sub shells. _(I couldn't get it to work and thought the syntax was bad. I will come back to it when need for it rises or see if there is a more modern way of doing that)._
- To make shell variable values available ot sub shells, the `export` command is used. There is no way of and no reason for making sub shell variables visible to parent shells.
```sh
$ x="2000"
$ sh 			# start subshell
$ echo $x
				# blank; $x not avaiblable
$ exit 			# leave subshell
$ export x 		# make x available to subshell
$ sh 			# start subshell again
$ echo $x 		
2000			# Voilà! x is here! It was exported, don't you know?!
```
`export` is Weird, so only export variables you need to use in all shells and subshells, especially special ones such PATH and HOME.

### 3.7 More on I/O redirection:
- When an error occurs, an error message is printed on the terminal. Each program has 3 default files established when it starts. These are *file descriptors.* They are numbered by small integers and are 0, standard input and 1, standard output and 2, standard error.
- To redirect standard output to some file, you can use the following construct:
```sh
diff file1 file2 file3 2>errorfile
``` 
There should be no space between `2` and `>`.
- Redirecting both standard error and standard output to standard output is done with the construct `2>1&`. Redirecting both standard output and standard error to standard error can be done with the construct `1>2&`
- Commands can be packaged together with their arguments in the same file so that a shell file can be self contained. Such a shell command is called a *here document.* It means the input is right here and not somewhere else. The following is an example of such shell file:
```sh
grep "$*" << End
baba
caca
dada
wawa
wawa
End
```
The construct `<< End` means everything until the world "End". 
- The next table lists the various shell I/O redirections: 
| Character | Description
| --- | --- |
| `>file` | direct standard output to *file*
| `>>file` | append standard output to *file*
| `<file` | take standard input from *file*
| `p1 | p2` | connect standard output of *p1* to standard input of *p2*
| `^` | obsolete synonym for |
| `n>file` | direct output from file descriptor *n* to *file*
| `n>>file` | append output from file descriptor *n* to *file*
| `n>&m` | merge output from file descriptor *n* with file descriptor *m*
| `n<&m` |	merge input from file descriptor *n* with file descriptor *m*
| `<<s` | here document: take standard input until next *s* at beginning of a line; subtitute for $, '`...`' and '\''
| `<<\s` | here document iwht no substitution
| `<<'s'` | here document iwht no substitution

### 3.8  Looping in shell programs:
- Being a programming language, the shell has variables, loopings, conditionals.. etc. Looping over filenames is very common and is done with the shell for statment; _(might be only one to type in terminal rather than in a a shell file)._ A for statment follows this general syntax:
```sh
for var in list of words
do
	commands
done
```
The following program prints filenames one per line:
```sh
for i in *
do
	echo $i # notice we are accessing the value here. This is a little different from regular programming languages.
done
```
- It might be desirable to compress this expression into a one liner. do and done are only recognized when they appear after a newline or a semicolon.
```sh
for i in *; do echo $i; done
```
- Loop can be done over not only filenames, but from anything, a list that you type `for i in 1 2 3 4; ...` or ``for i in  `cat ...` ; ...``.

### 3.9 _Bundle_: Putting it all together:
- *(Didn't quite get it! Might come back later!)*

### 3.10 Why a programmable shell:
- Some praise of the UNIX shell and enumeration or why it is or superior. It's not just a program for running commands, but a complete language that is both easy to use, conventient and can be extended to do much much more.

## CHAPTER 4: FILTERS
- **Filters** are a "family of UNIX programs that read some inoput, perform a simple transformation on it, and write some ounput." They include programs we've seen before such as `wc`, `grep`, `sort`. In addition to these filters, this chapter discusses *programmable filters* such `sed` and `awk` which are more generalized and more sophisticated. 

### 4. The grep family
- grep follows this general syntax:
```sh
grep pattern filename
```
It searches standard output or named files for *pattern* and print each line containing that pattern. Here are examples of `grep` with some of its common options:
```sh
grep -n potato *.java # Output lines with their numbers
grep From $MAIL  # input from a shell variable
grep potato s | grep -v mashed # Get line where non-mashed potato appears
grep -i ahmed phonebook # ignore case
who | grep ahmed # pipe as input
ls | grep -v *.c
```
- grep works on a restricted form of *regular expressions.* It was invented in an evening through a surgery on the *ed* editor. Unfortunately, regular expression metacharacters are not the same as shell metacharacters. They are generally similar but there are many differences. To avoid potential problems, enclose grep patterns with single quotes.
- Here is a brief review of these regular expressions along with a detailed table:
	* `^` **anchors** the pattern to the beginning of a line and `$` anchors it to the end.
```sh
grep '^lala' baba # -> lala is sick
grep '$lala' baba # -> There is no one but lala
```
	*(End anchor doesn't work on mac. Read somwhere it has to do with how newline is constructed)*
	* grep supports **character classes** just like shell as in `[a-z]` which matches any lower case letter. If the character class starts with a circumflex `^`, the pattern matches any character except those in the class. `[^A-Z]` means any character but uppercase letters. There is mo  backslash in character classes, to match square brackets and minus signs you can use `[][-]`
	* A **period** `.` matches any single character.
	* The **closure** applies to the previous character, metacharacter or character class, and collectively matches any number (zero or more) matches of the character, metacharacter, or character class:
		* `x*` matches a sequence of x's as long as possible`xxxxxxxx`.
		* `[1-2]*` matches a numerical string `12345567`.
		* `.*` matches everything until a new line `das#(*@ _)*&?221HHbB`.
		* `.*x` matches everything up to and including the last x in the line.
Two important observations about the closure operator:
	1. Closures apply to only one character and not a sequence. `xy*` is matched by `xyyyyy` and not `xyxyxyxy`.
	2. Any number of matches includes zero. If you want at least one character matched, duplicate the character or metacharacter as in `[0-9][0-9]*`
- No grep regular expression matches a new line. grep re are applied to each line individually.
- grep is the oldest of a family that also includes `egrep` and `fgrep`. fgrep can search many literal strings simultaneously, egrep evaluates true regular expressions with an "or" operator and parantheses to group expressions.
- egrep and fgrep have the option `-f` that allows them to take their input from a file that contains the lined separated patterns to be searched.
- egrep has a few more features than grep:
	* parantheses can group expressions, so (xy)* can match an empty string, `xy`, `xyxy` or `xyxyxyxyxy`.
	* Vertical bar `|` is an "or" operator. `(today | tomorrow)` and `to(day|morrow)` match either `today` or `tomorrow`.
	* egrep also has two closure operator `+` and `?`. `x+` matches one more x's, while `x?` matches 0 or 1 x's but no more.
- This one is cool. It searches a dictionary for words that contain all vowels in alphabetical order
```sh
egrep '^[^aeiou]*a[^aeiou]*e[^aeiou]*i[^aeiou]*o[^aeiou]*u[^aeiou]*$' dict 
```
- fgrep can't interpret metacharacters, but can search for thousands of words in parallel.
- grep is older has more options than egrep and has **tagged regular expressions *(WHAT"S THAT???????)*** egrep is much faster, but the two can combined in the same program.
- **Table. grep and egrep Regular Expressions** (Decreasing order of precedence):

| Pattern | Description
| --- | --|

| Pattern   | Description
| --- | --- |
| *c* | any non-special character *c* matches itself
| `\c` | turn off any special meaning of character c
| `^` | beginning of line
| `$` | end of line
| `.` | any single character
| `[...]` | any one of characters in `...`; ranges like a-z are legal
| `[^...]` | any single character not in `...`; ranges are legal
| `\n` | what the nth \(...\) matched (grep only) *(WHAT?)*
| `r*` | zero or more occurrances of r
| `r+` | one or more occurrances of r (egrep only)
| `r?` | zero or one occurrances of r (egrep only)
| `r1r2` | r1 followed by r2
| `r1 | r2` | r1 or r2 (egrep only)
| `\(r\)` | tagged regular expression (grep only); can be nested
| `(r)` | regular expression r (egrep only); can be nested
|  | no regular expression matches a newline.

### 4. Other filters
### 4. The stream editor sed
### 4. The awk pattern scanning and processing language
### 4. Good files and good filters

## CHAPTER 5: SHELL PROGRAMMING 






















