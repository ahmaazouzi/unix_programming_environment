# THE UNIX PROGRAMMING ENVIRONMENT

- this is an old book but it's held in high esteem by many. Seeing how many linux commands resemble those of unix and having enjoyed the C book, co-written by Kernighan, I thought I'd skim over this one and see what it still has to offer.
- These are my notes on what I understand of the book and what I manage to read of it.
- I will use macOS 10.x to test the scripts provided in the book and probably report if they are still relevant.
- I might skip the few commands and things I already know about how to use the terminal.

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

### 1.1 Files and common commands:
- Information is stored in files which have names, content, locations and metadata such as who owns them.
- **ed 101:** ed is an old text editor, much older than both vi and emacs. Basic commands of ed:
	* `a`: start adding text.
	* `.`: By itself on a line. Stop adding text.
	* `w <filename>`: save to named file
	* `q`: quit ed.
	* `1,$p`: prints to screen contents of file from line 1 to last line
- Weird thing is that when I open a file with ed, I can't see any of the text previously written into it.
- `ls`, which is used for listing files in a directory has many options. An interesting one is `-t` which lists files by time they were last modified and not alphabetically. `-l` list long info lines which include who owns the files and where they were created. `-r` reverses the order files are listed in. Options can be combined so `ls -lrt` lists long descriptions of files according when they were last modified in reverse order.
- Options have to precede file names. 
- Options can be combined or left separate but this is not always the case.
- Options in unix are not uniform between different commands, so `-l` means something in one command and a totally different thing in another commands.
- `cat` and `pr` print the contents of a file or several files to screen. `pr` cut content into pages and put dates on top of pages
- `lp` and `lpr` are used for printing. I don't know if this is still relevant!













 