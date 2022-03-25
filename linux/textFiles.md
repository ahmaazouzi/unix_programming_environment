# Editing, Finding and Searching Text Files
## Table of Contents:

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
| **Spacebar** | Move forward one char at a time. |
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

| Example |
| --- | --- |
| **`/Ahmed`** |
| **`?Saqqa`** |
| **`/S*ra`** |
| **`?[Pp]resident`** |

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
### Using `locate` to Find Files by Name:
### Searching for Files with `find`:
#### Finding Files by Name:
#### Finding Files by Size:
#### Finding Files by User:
#### Finding Files by Permission:
#### Finding Files by Date and Time:
#### Using `not` and `or` when Finding Files:
#### Finding Files and Executing Commands: 

### Searching in Files with `grep`: