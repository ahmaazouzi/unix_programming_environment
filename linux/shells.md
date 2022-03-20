## The Shell:
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
| **`exit`** | Exits the current shell |
| **`CTRL+D`** | Same as `exit` |

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
	- **`/usr/local/bin`** to make them available to all users.

### Command types:
- commands are of different types and certain types can override others. An **alias** `cd`, for example, can probably override the built in `cd`. The following table lists the different types of commands in the order in which they can override types below them:

| Command types | What | 
| --- | --- |
| **Aliases** | A name for a long command with possibly multiple options. It can even replace a pipe of commands and multiple options. You can get a list of aliases for a given shell using the `alias` command. |
| **Shell reserved words** | Words you'd use to for shell programming like `do`, `while` and `case`. It's curious that aliases can even replace these. |
| **Functions** | A set of commands. |
| **Shell built-ins** | These are built into the shell and are not found in the filesystem. They include: `set`, `echo`, `cd`, `exit`, `fg`, `pwd`, `type` etc. |
| **Filesystem commands** | These are stored in the file system and are pointed to by the `PATH` variable. |

- The **`type`** command in bash (its equivalent in other shells like **`sh`** is **`which`**) tells you the type of the command such as whether it is an alias as well as its location. with the `-a` option it tells you all its locations:
```sh
$ type -a which
```
- Output:
```
which is /usr/bin/which
which is /bin/which
```
- There is talk of a **`locate`** command, but it doesn't seem to be built into the Ubuntu distribution. 

### History and Rerunning Commands from the Past:
- The shell remembers commands you typed in the past and has comes with functionality that allows you to rerun these commands easily. This is most useful when these commands are long with several arguments and options. 
- This is largely achieved through the **shell history** which is a list of commands you ran in the past. The **`history`** commands prints this list to standard output.
- You can access the most recent command from the shell history by hitting the up arrow. the last command will appear. Combine this with a few keystrokes to edit the commands and speed up your work. The following table shows some of these powerful shortcuts

| Shortcut | Action |
| --- | --- |
| **`CTRL+K`** | Deletes characters after cursor |
| **`CTRL+U`** | Deletes character before cursor |
| **`CTRL+W`** | Deletes one word before cursor |
| **`CTRL+Y`** | Retrieves last deleted char, word or line |

- Some other very useful keystrokes include:

| Shortcut | Action |
| --- | --- |
| **`CTRL+V+Tab`** | This allows you to print a special character like a tab character |

- Shell history can help you autocomplete certain things as the following table shows:

| Pattern  | What? |
| --- | --- |
| Nothing special| Start typing name of command, function or alias to get it autocompleted. |
|  **`$`**| This is for variables |
|  **`@`**| For hostnames (doesn't seem to work) |
|  **`~`**| Picks a user. Takes you to a given user's home directory. |

- When you type the command `history`, you get a list of commands you ran in th past along with executing numbers denoting their running order in the past. You can actually rerun these commands without retyping them or scrolling ad infinitum with the up arrow key as the following table shows:

| Command | Meaning |
| --- | --- |
| **`!360`** | Runs command 360 |
| **`!!`** | Runs previous command |
| **`!?system`** | Runs the last command containing the string `system` |
| **`CTRL-R`** | Allows you research past commands containing a given string and rerun it, kinda similar to the previous command. |

- This whole history is stored in a file in the home directory of the user called **`.bash_history`**. ***Ahhah, that's what it is!!!***
- The shell history is a useful feature, but it can be exploited by malicious actors especially when the user has root access and administrative roles. An admin might have to clear their history and should probably disable root user history. Disabling history for root can be done by setting **`HISTFILE`** to **`/dev/null`** or leaving **`HISTSIZE`** blank. 


### Connecting Commands:
- Shell **metacharacters** allow you to combine and connect commands in some very powerful ways. They include:

| Metacharacter(s) | Significance |
| --- | --- |
| **`|`** | Inputs into a command the output of another command |
| **`;`** | Allows running multiple commands sequentially in one line |
| **`&`** | Works like **`;`**, but command 2 doesn't wait for the previous command to end. Closing the shell will still kill the long running command 1 |

### Expanding:
- **Expanding commands** allows you to have the output of a command as arguments/input to another command. This sounds like pipelining but not really quite. This one can do things that probably a pipe cannot such as running a command inside a string, something like formatting in python with the format method as in:
```sh
echo "Your files are:
$(ls)"
```
- Expanding commands is with either wrapping a command in parentheses preceded by a dollar sign **`$(command)`** or wrapping the command in backticks as in **``command``**.
- We can also expand arithmetic expressions using dollar sign and brackets as in:
```bash
echo "I am $[2022 - 1953] old"
```
- When you access a shell variable to read it, you are actually expanding it, hence the dollar sign.

### Shell Variables:
- The following two commands show you certain types of variables:

| Command | Action |
| --- | --- |
| **`set`** | Shows all variables for current shell |
| **`declare`** | Same as `set` |
| **`env`** | Show so-called environment variables. |

- **Environment variables** are the ones exported to any new shell you open. They are not ephemeral as the variables you create on a shell and that disappears once you exit that shell. Environment variables store a lot of useful information about your system like the username, shell prompt, OS type, etc.
- Some very common environment variables are:
	- **`PATH`**, **`HOME`**, **`PWD`**, **`HISTFILE`**, **`HISTFILESIZE`**, **`PS1`**, **`TMOUT`**, **`SHLVL`**.

### Aliases:
- **Aliases** might be one of the most useful features of a Unixy systems. An alias is a shortcut to  command or set of commands along with arguments and options. Setting aliases will save you a lot of typing and having to search how a rarely used and complicated command is used. 
- Apart from setting an alias which you should know, there are:

| Command | Role |
| --- | --- |
| **`alias`** | Sows set aliases |
| **`unalias`** | Removes an alias |


### Setting Shell Environment:
-



















