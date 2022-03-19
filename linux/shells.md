## Shells:
- **`bash`** is the default shell.
- You can use a different shell by simply typing its 'name', such as **`sh`** or **'csh'**. When done, type **`exit`** to go back to the default shell **`bash`**.
- You can change the default **shell prompt**, but standard ones are:

| Prompt | Significance |
| --- | --- |
| **`$`** | Logged as any user |
| **`#`** | Logged as root user |

- By the way, to log in as root user, you'd type:
```sh
su -s
```
- Some of the common  and basic commands include:

| Command | Action |
| --- | --- |
| **`date`** | Outputs time and date |
| **`who am i`** | What user you're logged as |
| **`hostname`** | Tells computer name |
| **`uname`**| Prints information about the system |
| **`id`** | Prints information about you as a user: what groups you belong to, etc. |

### Command Syntax:
- A shell command can be followed by **options** and **arguments**.
- An option is either a single letter preceded by one hyphen or a word preceded by 2 dashes as in:
	- **`ls -al`**: **`-al`** is actually two options **`-l`** and **`-a`**. 
	- **ls --help**: **`--help`** is a whole-word option.
- A command can also have one or more arguments.
- The tricky part is that an option itself might be associated with an argument. Think for example of specifying the video/audio format of file you download with `youtube-dl`. `-f` is the format option and the value `140` is the argument associated with it:
```sh
youtube-dl -f 140 somefile...
```
- When using a word as an option the argument is separated from the option using a **`=`** sign as in:
```sh 
ls --hide=*.html. # Hide all files that end wht `html`
```
- A few commands, however, can have option words preceded by single hyphens.
- A special option **`+`** after **`date`**. It allows you to format the date output as in the following command which displays the current day of the month:
```sh
date +'%d' # No space is allowed after `+`
```

### Locating Commands:
- ***This section is extremely important and clears out much of the confusion and frustration I've had over the last few weeks (years actually).***
- Where are the shell commands and how does the shell find them? Let's answer this question step by step.
- For the shell to find a command and run it, the command must either be in the so-called **`**`$PATH`**`** or you must type the absolute or relative location of the command. This is why we type **`./someCommand`**. You're telling the shell to look inside the current folder which contains the command `someCommand`.
- Typing the full path can be annoying, so we have the **`PATH`** variable which contains a list of paths the shell can search for a given command. It might look something like this:
```sh
/usr/local/bin:/usr/bin:/bin:/usr/local/sbin:/usr/sbin:/sbin:/home/USER/bin
```
- The order of paths in the `PATH` variable is crucial. These paths are evaluated from left to right, so `/usr/local/bin` is evaluated first, then `/usr/bin`, then `/bin`, etc. 
- This order can be a source of mystifying frustration for the uninitiated. If two commands (or same command with the same name are located in two paths), the one located in the first path will run. To run the second one, you must specify its full path (relative or absolute). The other day, I wasted a couple hours because of this.  I had installed a python package twice, one with `sudo pip install` and the other with `pip install`. One of them run but not the other. I also installed another package that depended on one of the packages, but the main program could never import it. The program that didn't happen to have the dependency was the one that ran because it was to the left of the path.
- Classic Unix/Linux commands are like **`wc`**, **`ls`**, etc. are generally stored in one of these:
	- **`/bin`**
	- **`/usr/bin`**
	- **`/usr/local/bin`**.
- Administrative commands are located usually in one of these:
	- **`/sbin`**
	- **`usr/sbin`**

- A user might or might not have his own **`bin`** folder or might have another folder that serves the same purpose. If such a folder doesn't exist you need to create it and add your world-changing commands to it and add it to the path variable. In the `PATH` variable above we have **`/home/USER/bin`** where `USER` stands for any user name. In some ubuntus, I've seen something like **`/home/USER/.local/bin`** perpended to the PATH. ***After some basic research, it tunred out that python package added itself there (Don't laugh at me, the reason I'm reading this book is to stop saying seemingly dumb stuff like this).***
- When you make your own commands you can add them to:
	- **`/home/USER/bin`** to make them available to you.
	- **`/usr/local/bin`** to make them them available to all users.
-


