# Editing, Finding and Searching Text Files
## Table of Contents:
* [Intro](#intro)
* [Editing Text Files with `vi` and `vim`](#editing-text-files-with-vi-and-vim)
	+ [Using `vi`](#using-vi)
		+ [Adding Text](#adding-text)
		+ [Moving around in Text](#moving-around-in-text)
		+ [Deleting, Copying and Changing Text](#deleting-copying-and-changing-text)
		+ [Movement and Editing a Number of Times with Using Numbers](#movement-and-editing-a-number-of-times-with-using-numbers)
		+ [Pasting (Putting) Text](#pasting-(putting)-text)
		+ [Repeating Commands](#repeating-commands)
		+ [Exiting `vi`](#exiting-vi)
		+ [Other Useful Commands and Tips](#other-useful-commands-and-tips)
	+ [Skipping around in the File](#skipping-around-in-the-file)
	+ [Searching for Text](#searching-for-text)
	+ [Using "ex mode"](#using-ex-mode)
	+ [Learning more about `vi` and `vim`](#learning-more-about-vi-and-vim)
* [Finding Files](#finding-files)
	+ [Using `locate` to Find Files by Name](#using-locate-to-find-files-by-name)
		+ [Using `locate`](#using-locate)
	+ [Searching for Files with `find`](#searching-for-files-with-find)
		+ [Finding Files by Name](#finding-files-by-name)
		+ [Finding Files by Size](#finding-files-by-size)
		+ [Finding Files by User](#finding-files-by-user)
		+ [Finding Files by Permission](#finding-files-by-permission)
		+ [Finding Files by Date and Time](#finding-files-by-date-and-time)
		+ [Using `-not` and `-or` when Finding Files](#using--not-and--or-when-finding-files)
		+ [Finding Files and Executing Commands](#finding-files-and-executing-commands)
	+ [Searching in Files with `grep`](#searching-in-files-with-grep)

## Intro:
- This document will tackle three basic topics:
	- Editing text files with non-graphical text editors, specifically **`vi`** and its fancier sibling **`vim`**.
	- Finding files in a system or in specific directories based on some criteria such as filename, size, ownership, modification date, etc. 
	- Searching files for certain patterns, words, etc. 

## Editing Text Files with `vi` and `vim`:
- ***I've spend some time with `vim` elsewhere, so I might go light on some topic, or I might go deep on this and just tackle my `vim` knowledge for good!!***

### Using `vi`:
- When you first enter `vi` or `vim`, you can't probably do anything apart from navigating an existing document you've opened with the command, the way you'd navigate a `man` page. By the way **`vim`** stands for **`vim` improved**. `vi` doesn't have cool stuff like syntax coloring and visual highlighting. The latter seems indispensable to me!
- What sets `vi` from graphical text editors most of us noobs are used to such as a *Sublime Text* and *VS Code* is that it has two operating modes:  **command mode** and **input mode**. In graphical text editors these two modes are mashed together so you can navigate the document as insert text into the file all at the same time. We will learn about these operating modes.  
- When you first open vim, you are by default in the command mode. In this mode, clicking on a key such as `v` doesn't print a v into the screen but highlights something on the screen. Commands are case sensitive also. To input text into the document, you need to issue some command that tell the program to allow you to insert text into the file.

#### Adding Text:
- Learn all these and don't restrict yourself to `i`. For many months, I entered insert mode, move one character at a time to the end of the line, then pressed Enter to add a new line!! This is extremely stupid considering that it can be done with just pressing `o`.

| Command | Action |
| --- | --- |
| **`i`** | Inserts text that starts to the left of the cursor. |
| **`I`** | Inserts at the beginning of the current line. |
| **`a`** | Add text that starts to the right of the cursor. |
| **`A`** | Append text at the end of the end of current line. |
| **`o`** | Open below. Opens a line below the current line and puts you in insert mode. |
| **`O`** | Open above. Opens a line above the current line and puts you in insert mode. |

- When done with inserting text, press the Esc key. It's great to map the caps lock and turn it into an Esc key.
- By the way, to get out of the _ex mode_ (the one you get into from command mode by typing `:` to do things like `:q`, etc.), you press Esc twice.

#### Moving around in Text:
- Some of these are really really important. Others are great if you can remember them. 

| Command | Action |
| --- | --- |
| **`Arrow keys`** | Move cursor left, right, up, down one character at a time. |
| **`h`** | Move left one char at a time. |
| **`l`** | Move right one char at a time.  |
| **`j`** | Move down one char at a time.  |
| **`k`** | Move up one char at a time.  |
| **`Spacebar`** | Move forward one char at a time. |
| **`Backspace`** | Move back one char at a time. |
| **`w`** | Move to the beginning of the next word (delimited by spaces, tabs and punctuation). |
| **`W`** | Move to the beginning of the next word (delimited by spaces and tabs only). |
| **`b`** | Move to the beginning of the previous word (delimited by spaces, tabs and punctuation). |
| **`B`** | Move to the beginning of the previous word (delimited by spaces and tabs only). | |
| **`0`** (zero) | Move to the beginning of the current line |
| **`^`** | Move to the beginning of the current line (Less convenient than `0`). |
| **`$`** | Move to the end of the current line |
| **`H`** | Moves cursor to upper left corner of the screen (not necessarily top of file). |
| **`M`** | Moves cursor to the first character of the middle line in the screen. |
| **`L`** | Moves cursor to the lower left corner of the screen. |

#### Deleting, Copying and Changing Text:
- **`x`**, **`y`**, **`d`**, and **`c`** along with their capitalized counterparts can be used to delete, copy and edit characters, words or lines or larger chunks of text when used either by themselves or in combination with other keys. The following table shows how they are used in general:

| Command | Action |
| --- | --- |
| **`x`** | Deletes character under cursor. |
| **`X`** | Deletes character before the cursor. |
| **`d<?>`** | Deletes some text. |
| **`c<?>`** | Changes some text. Basically it erases some text and p |
| **`y<?>`** | Yanks (copies) some text. |

- The commands above (those with the placeholder **`<?>`**)can be combined with movement commands to have more control over what you wanna edit, delete or yank as the following table show:

| Command | Action |
| --- | --- |
| **`dw`** | Delete word after the cursor |
| **`db`** | Delete word after the cursor |
| **`dd`** | Delete current line |
| **`d$`** | Delete characters from cursor to end of line. |
| **`d0`** | Delete characters from cursor to the beginning of line. |
| **`c$`** | Change (erase) characters from current to end of line and put you in insert mode. |
| **`c0`** | Change (erase) characters from current to end of line and put you in insert mode. |
| **`cl`** | Erase current character and go into insert mode. |
| **`cc`** | Erase current line and go into insert mode. |
| **`yy`** | Yank the current line into the buffer*??!?!!* |
| **`y)`** | Copy the current sentence to the right of the cursor into the buffer. |
| **`y}`** | Copy the current paragraph to the right of the cursor into the buffer. |


#### Movement and Editing a Number of Times with Using Numbers:
- You can combine the commands we've seen so far (for movement, editing, copying and deleting) with numbers to do the given action for a number of words, lines or characters, etc. The following table provides some examples:

| Example | Action |
| --- | --- |
| **`3dd`** | Delete next 3 lines, starting with the current one. |
| **`3dw`** | Delete next 3 words. |
| **`5cl`** | Erase next five characters and enter insert mode. |
| **`12j`** | Move down 12 lines. |
| **`5cw`** | Erase 5 next words and put you in insert mode. |
| **`4y)`** | Copy next 4 sentences cursor. |

#### Pasting (Putting) Text:
- When you copy text into the buffer (by deleting, copying it or changing it using any of these commands or their variations `c`, `d`, `x`, `y`). You can paste (put) this text into the file with one of the following commands:

| Command | Action |
| --- | --- |
| **`P`** | Put text to the left of cursor if it were chars and words; above current line if it were a line. |
| **`p`** | Put text to the right of cursor if it were chars and words; below current line if it were a line. |

#### Repeating Commands:
- The dot command (**`.`**) allows you to repeat the last command in somehow an intelligent way. For example, if you search and then change the word `class` to `function` using `cw`, you can go to another occurrence of the word `class` using the command **`n`**, place cursor at its beginning, press the dot command and the word would be changed to `function`. You can keep doing this to the following occurrences of the world `class` by alternating `n` and `.` ***AMAZING!!!!!!***

#### Exiting `vi`:

| Command | Action |
| --- | --- |
| **`ZZ`** | Save changes and exit. |
| **`:w`** | Save changes without exiting. |
| **`:wq`** | Save changes and exit, same as `ZZ`. |
| **`:q`** | Exit (possible when no changes have been made). |
| **`:q!`** | Exit without saving changes. |

#### Other Useful Commands and Tips:

| Command | Action |
| --- | --- |
| **`u`** | Undo last change. |
| **`CTRL+R`** | Redo last undone change. |
| **`:!command`** | Allows you run a shell command while you are editing something in `vim`. *AMAZINGGG!!!*, maybe list current directory files or see date or `sed` something!!! To go back to `vim`, press Enter. You can also launch a shell from this command, then when done, exit that shell to come back. Save your work, though, before going into a new shell, because you might forget to come back to `vim`. |
| **`CTRL+G`** | Prints the name of the current file and current you're editing at the bottom of the screen. It also prints the number of lines in the file, percentage of where you are in line, and column where cursor is. This is helpful after breaks and whatnot! |

### Skipping around in the File:
- The following pages allow you to move more efficiently in large files that cover more than what can be displayed at once on the screen:

| Command | Action |
| --- | --- |
| **`CTRL+f`** | Move ahead one page at time. |
| **`CTRL+b`** | Move backward one page at a time. |
| **`CTRL+d`** | Move ahead half a page at a time. |
| **`CTRL+u`** | Move back half a page at a time. |
| **`G`** | Go to the last line of the file. |
| **`1G`** | Go to first line of the file. |
| **`gg`** | Go to first line of the file. |
| **`35G`** | Go to line number 35. |

### Searching for Text:
- ***This is the part I had very little knowledge about***. 
- to Search a file, you'd use either:

| Command | Action |
| --- | --- |
| **`/`** | Search forward relative to the cursor current position. |
| **`?`** | Search backward relative to the current cursor position. |

- Examples include:
	- **`/Ahmed`**.
	- **`?Saqqa`**.
	- **`/S*ra`**.
	- **`?[Pp]resident`**.

- After typing the search term, press enter. To continue searching for other occurrences of the same pattern use:

| Command | Action |
| --- | --- |
| **`n`** | Keep searching in the same direction. |
| **`N`** | Continue searching but in the opposite direction. |


### Using "ex mode":
- `vi` is originally based on yet an older editor called `ex` which allowed you to search some pattern and edit it in one or more lines, but couldn't print more than one page! `vim` still retains vestiges of `ex`. When you type a column **`:`** followed by something like **`:q`**, you are basically in **ex mode**. The following examples show some of the more useful facets of the ex mode:

| Command | Action |
| --- | --- |
| **`:g/Action`** | Search for the word `Action` and prints all occurrences of lines where it is. Like `grep`. |
| **`:s/Action/action`** | substitute the first occurrence of `Action` with `action` in the current line. |
| **`:g/Action/s//action`** | Find all first occurrences of `Action` in each line and substitute them with `action` |
| **`:g/Action/s/action/g`** | Find all occurrences of `Action`  and substitute them with `action` |
| **`:g/Action/s/action/gp`** | Find all occurrences of `Action`  and substitute them with `action` and print all changes so you can check if something goes wrong, or modified things you didn't mean to change. |

### Learning more about `vi` and `vim`:
- To learn more about `vim` and `vi`, type **`vimtutor`** which steps you through a fun tutorial of vim and its great features.

## Finding Files:
- The most basic Linux installation can have many many files in it you need a way to search these files and find what you need. There are a few utilities that do just that and more:
	- The **`locate`** utility allows you to find files by name.
	- The **`find`** command can find files based on many attributes.
	- The **`grep`** allows you to search files themselves for a given pattern. 

### Using `locate` to Find Files by Name:
- The **`locate`** utility is not included in Ubuntu (might be in RHEL and Fedora).
- A closely related command is **`updatedb`**. In some systems it's scheduled to run daily.
- Our command **`locate`** searches this database for a file name.
- The sturdier command `find` does all what `locate` can do and much more. Why use `locate`? This command has both advantages and disadvantages. It is much faster to locate files with this command because we are searching a database rather than searching the system. This database is populated once a day at a non-busy time.
- Disadvantages include the fact that `locate` reflects the state of the system when the database was last updated. Any files added after that update will not be located, and you might see files that don't exist anymore. 
- `locate` might also not include certain files. By default remotely mounted filesystems and those on CDs, etc. are not included. You can use some configuration file `/etc/updatedb.conf` to  not include other files or types of files in certain locations.
- Permissions are also preserved when using locate. A regular user cannot see files that can only be seen bu a sudoer!
- A string being searched can appear anywhere in the file paths returned. You can also use shell metacharacters such as **`*`**.

#### Using `locate`:
- Examples of `locate` usage include:
```bash
locate  locate *twitter*pycache*
locate passwd
```
- A useful option is **`-i`** for ignoring case.
- `locate` only search of the given pattern in paths, and not necessarily a file name. If you type `bash`, you'd get different levels of directories containing the string `bash` such as `bash-completion`, `bash-whatever` and files like `.bashrc`, etc.

### Searching for Files with `find`:
- **`find`** is the more robust file finding command mainly because it allows you to search for files based on a variety of attributes. It also allow you to act on the files you find using the **`-exec`** or **`-okay`** options (we will see these later).
- `find` is slower than `locate` because it searches the actual system rather than a database. However, you get an up-to-date view of the system. To mitigate the slowness problem, you can limit the part of the filesystem you'd search. 
- `find` can search files using its filename, ownership, permission, size, modification times and other things. The attributes can also be combined to polish and zero the search down. 
- Examples of using `find` include:
```sh
 find
 find /etc
 find $HOME -ls
 ``` 
 - By running `find` without options or arguments, `find` finds all the files in the current directory, including those in subdirectories. It is somehow similar to running `ls -R` but not quite. The output filenames are all full paths to each file and directory.
 - You can also specify a certain point in the filesystem where to search such as `/etc` in our example. 
 - `find` also respects permissions so a regular users cannot see files visible under root only. Such files or directories result in `permission denied` messages.
 - Using the **`-ls`** option make `find` act like `ls -l`. It list file paths along with permissions, size, etc. 

#### Finding Files by Name:
- To find a file by name, you'd use:
	- **`-name`** which can take either the exact name of a file, or a file matching pattern. It is case-sensitive.
	- **`-iname`**: Same as `-name` but ignores case.
- It might be a good idea to enclose patterns in quotes. 
- Directories cannot be found with this option (unless maybe combined with some other option).

#### Finding Files by Size:
- The **`-size`** option allows you to search files that are equal to, smaller or larger than a given size. Seems to only consider Megabytes (**`M`**) and Gigabytes (**`G`**) as units:
```
find -size +10M
find -size -1G
find -size 0M
```

#### Finding Files by User:
- You can also search files by specifying the **`-user`** or **`-group`** that owns it. You can also use the **`-not`** and **`-or`** option to refine the search further:
```sh
find / -not -user root
find -user am or -user sam
find -group it
```

#### Finding Files by Permission:
- Searching files by permission can be crucial or at least useful and convenient for those concerned with security, both malicious attackers and ethical defenders. You can quickly find files with open and dangerous permissions throughout your system in a particular where you've created new files. 
- There are some differences between what I'm reading and what I see on the Ubuntu system. I'll stick to Ubuntu.
- You can search files by permissions using both octals and symbols just as you'd do with setting permissions through `chmod`. 
- Searching by permissions is done with the **`-perm`** option which has three varieties or modes:
	1.  or : All 3 bits much match 

| Octal | Symbolic | Meaning |
| --- | --- | --- |
| **`664`** | **`u=rw,g=rw,o=r`** | The full number must match. |
| **`-222`** |  **`-u=w,g=w,o=w`** | If any of the bits is present, there is a match. `-000` will match anything. `-222` matches all writables including `222`, `333`, `666`, `777`, etc. All three bits should be present |
| **`/222`** | **`/a=w`** | If any one bit is in any of the three numbers there is a match. `/222` will match `112`, `121`, `211`, et, etc. |

- An example:
```sh
find /somewhere -perm /222 -type f
find /somewhere -perm /a=w -type f
```
- The previous example has two examples that do the exact same thing. It checks if all files are read-only in `/somewhere`.

#### Finding Files by Date and Time:
- The date field in each file is changed whenever the file is accessed, changed or its metadata has been changed. This is important for security and other things. Examples of when you might need to search files by date and time include:
	- You've recently changed a configuration file somewhere, but forgot which it was. You can go to where you think the file is, something like `/etc` and search that location by time.
	- You think you might have been hacked, so you go check sensitive locations and find of permissions were changed on files in the last However many days.
	- You can find if there are unnecessary files that you haven't accessed in a very long time so you can archive or delete them.
- You can search files by two time units: days `date` and minutes `min`.  You can also search if the file was accessed, changed or has its metadata modified. These are done with the following options:

| Option | Description |
| --- | --- |
| **`-atime`** | Days since last accessed |
| **`-ctime`** | Days since last changed |
| **`-mtime`** | Days since last metadata changed |
| **`-amin`** | Minutes since last accessed |
| **`-cmin`** | Minutes since last changed |
| **`-mmin`** | Minutes since last metadata changed |

- Another detail about these options is that the time can be preceded by:
	- A plus **`+`**  Any time before that minute in the past resulting from subtracting the given days/minutes from now.
	- A minus **`-`** Now minus the number of days or minutes.
- Examples include:
```bash
find /etc/ -cmin -10 # Find if a file in /etc has been modified in the last 10 minutes
find /idle -ctime +300 # See if file has last been accessed more than 300 days ago.
``` 

#### Using `-not` and `-or` when Finding Files:
- We've seen **`-not`** and **`-or`** earlier. These two options allow you to construct complex queries to really refine a search:
```sh
find -user am -or -user sam -or -size +1M
```

#### Finding Files and Executing Commands:
- There are also convenient options that allows you to run commands on found files:
	- With **`find [options] -exec command {} \;`**, you'd execute the command without first prompting.
	- With **`find [options] -ok command {} \;`**, you will need to okay dangerous commands.
- Examples include:
```sh
find -type f -exec cp {} ../l/ \;
```

### Searching in Files with `grep`:
- I use **`grep`** regularly, but not to its full potential.
- In addition to searching one or more files using `grep`, you can also search directory structures recursively.
- You can also list just names of files containing the pattern instead of all the lines.
- The Following table show examples of how `grep` is used in Linux:

| Example | Action |
| --- | --- |
| **`grep -i sth *`** | Search all files in current directory for pattern `sth` regardless of case. |
| **`grep -vi sth someFile.txt`** | Print all lines in `someFile.txt`that don't contain case-insensitive variations of `sth` |
| **`grep -rli mama lala/`** | Search files in directory `lala/` for pattern `wawa`  recursively using `-r` option. List only files containing the pattern `-l`. `-i` is for ignoring case. |



