# Python and Scripting:

## Table of Contents:
* [Python Scripts](#python-scripts)
* [Files](#files)
	+ [Open/close](#open/close)
	+ [Reading files](#reading-files)
	+ [Writing to Files](#writing-to-files)
	+ [Managing Files/Directories](#managing-files/directories)
	+ [Working with Directories](#working-with-directories)
	+ [Handling CSV Files](#handling-csv-files)
* [Regex](#regex)
	+ [Capturing Groups](#capturing-groups)
	+ [Splitting](#splitting)
	+ [Replacements](#replacements)
* [Managing Data and Processes](#managing-data-and-processes)
	+ [Environment Variables](#environment-variables)
	+ [Command-Line Arguments](#command-line-arguments)
	+ [Exit Statuses](#exit-statuses)
	+ [`eval()`](#eval)
	+ [Subprocesses](#subprocesses)
		+ [Capturing Subprocess Output](#capturing-subprocess-output)
		+ [Advanced Subprocess Fiddling](#advanced-subprocess-fiddling)
		+ [Problems with Subprocesses](#problems-with-subprocesses)
* [Bash and Bash Scripting](#bash-and-bash-scripting)
	+ [Redirecting IO](#redirecting-io)
	+ [Signaling Processes](#signaling-processes)
	+ [Bash Scripting](#bash-scripting)
		+ [Variables](#variables)
		+ [Globs](#globs)
		+ [Bash Conditional](#bash-conditional)
		+ [`test`](#test)
		+ [`while` Loop](#while-loop)
		+ [`for` Loop](#for-loop)
		+ [Bash Arrays](#bash-arrays)

## Python Scripts:
- You can turn your Python scripts into executable shell files in the following steps:
	- Start the file with a _shebang_: **`#!/usr/bin/env python3`**. There are other small variations but I don't know the difference.
	- Make the file executable throught the shell command **`chmod +x <fileName>`**.
- Now your python program can be run by typing its name (or name preceded by **`./`**)and pressing TURN.
- Python and Bash scripts can be combined and used together.

## Files:
### Open/close:
- We open a file with **`open`** which we can use in one of the following idioms:
```py
file = open("someFile.txt") # open takes a file name/path
```
```py
with open("someFile.txt") as file:
	# do something here
```

### Reading files:
- We need to close a file with **`file.close()`** after we are done using it. This is not necessary with the `with open("someFile.txt") as file:` construct because the file is closed automatically after we exit the block.
- Leaving a file open can lead to funny and interesting behavior that we don't want.
- We can read the content of the file with
	- **`file.readline()`** which reads the current line (a single line starting at the current position).
	- **`file.read()`** reads from the current position to the end of the file.
- A file is basically a list of lines. A common idiom is to iterate over the lines of a file and do something to the lines as in:
```py
with open("someFile.txt") as file:
	for line in file:
		if line[0]:
			print line[:-2] # a line has `\n` and print adds an `\n`. We just need one.
```
- **file.readlines()** reads the lines of a file and puts them in a list. These lines will contain visible `\n`s.

### Writing to Files:
- To write a a file, you need to first open it using a second argument for *mode*. Mode arguments include:
	- **`r`**: read only.
	- **`w`**: write only (overwrite existing content. Creates file if it it doesn't exist).
	- **`a`**: append (write at the end of the existing content). (Creates file if it it doesn't exist).)
	- **`x`** open for exclusive writing. This DOES NOT create a file if it doesn't already exist.
	- **`r+`**: read and write. (Creates file if it it doesn't exist).
- These modes can be confusing and dangerous: 
	- `r` is not necessary as files are opened by default to be read. 
	- `w` will over-write (delete) file content as soon as its opened. 
	- `w` also results in creating a file if it doesn't exist.
- To actually write into a file, we use the verb **`write`** as in:
```py
with open("someFile.txt", "w") as file:
	file.write("Something crazy!\n")
```

### Managing Files/Directories:
- The **`os`** module allows us to interact with the operating system including ways to manage files and directories.
- **`os.remove("someFile.txt")`** removes the file.
- **`os.rename("old.txt", "new.txt")`** renames file.
- To perform `remove` and `rename` on a file, this file must first exist. We check for the existence of a file with **`os.path.exists("file.txt")`**:
```py
import os

if os.path.exists("file.txt"):
	os.remove("file.txt")
```
- Other functionality provided by the `os` module include:
	- **`os.path.getsize("f.txt")`** which returns file size in byttes.
	- **`os.path.getmtime("f.txt")`** returns a timestamp of when the file was last modified.
- `os.path.getmtime("f.txt")` spits out a timestamp which is incomprehensible to a human being. With `datetime` we can make sense of it. The following should print out a human readable time and date of when the file was last modified:
```py
import datetime
datetime.datetime.fromtimestamp(os.path.getmtime("f.txt"))
```
- **`os.path.abspath("f.txt")`** returns the absolute path of a file.

### Working with Directories:
| Command | Action |
| --- | --- |
| `os.getcwd` | Returns current directory |
| `os.mkdir("dir")` | Creates a new directory |
| `os.chdir("someDir")` | Changes directory |
| `os.rmdir("someDir")` | Remove directory (provided it's empty) |
| `os.listdir("someDir")` | List files, directories in a directory (argument is optional if you wanna list) |
| `os.path.isdir("someFile")` | Tells you if a path is a dir |
| `os.path.join(dir, name)` | join some string (file name) with a directory path _intelligently_, so it's portable across OSs |

### Handling CSV Files:
- You'd open and close a CSV file the way you'd do it with any other type of file.
- We need to **`import csv`** to be able to parse a CSV file with **`csv.reader("openedFile")`** as in:
```py
import csv

with open("file.csv") as csvFile:
	reader = csv.reader(csvFile)
	for row in reader:
		print(row)
```
- Each row is a list of elements.
- To generate a csv file, you'd use **`csv.writer("fileTobeWritten").writerows(rows)`** for generating multiple rows. You'd use **`writerow`** instead of `writerows` to generate a single row:
```py
import csv 

rows = [["Mobile", "AL"], ["Charleston", "WV"], ["Riverside", "NJ"]]

with open("cities.csv", "w") as file:
	writer  = csv.writer(file)
	writer.writerows(rows)
```
- With **`csv.DictReader(file)`** and **`csv.DictWriter(file)`** you can read and write csv files using dictionaries. Just make sure that such csv files are labeled, meaning the top row has field names.

```py
import csv
records = [{"city": "Mobile", "state": "AL"}, {"city": "Charleston", "state": "WV"}, {"city": "Riverside", "state": "NJ"}]

keys = ["city", "state"]

with open("cities.csv", "w") as cities:
	writer = csv.DictWriter(cities, fieldnames = keys)
	writer.writeheader()
	writer.writerows(records)
```

## Regex:
- Regular expressions in Python can be handled with **`re`** module which include the following great functions:
	- **`re.search(pattern, stringToBeMatched)`**: returns a match object. Search whole string for the pattern.
	- **`re.match(pattern, stringToBeMatched)`**: returns a match object. Only matches if the string starts with the pattern.
	- **`re.findall(pattern, stringToBeMatched)`**: "return all non-overlapping matches of pattern in string, as a list of strings or tupl"
- The first input to these functions need to be a raw string preceded by an **`**`r`**`** as in **`re.match(r"ca", "california")`**. The reason is to prevent confusion when escaping regex special characters, because strings also have escape characters such as `\n`.
- The case can be ignored by adding a third argument as in:
	- **`re.match(r"ca", "California", re.IGNORECASE)`**

### Capturing Groups:
- When you surround something in a pattern with parentheses you are capturing a group. The match object your rejex match returns has a function **`groups`** which is a tuple containing these groups. The following example shows how this works:
```py
import re

result = re.match(r"^(\w+), (\w+)$", "Doe, John")
print(result.groups()) # prints ('Doe', 'John')
```
- Indexes on the match object give us access to each of these groups. Conttinuing witht the example abov:
	- `result[0]` refers to the whole matched pattern `Doe, John`
	- `result[1]` refers to the matched pattern `Doe`
	- `result[2]` refers to the matched pattern `John`
- Using groups, we can shuffle the pattern and change it around.

### Splitting:
- **`re.split(pattern, stringToBeMatched)`** splits text using a regex. It spits out a list of split elements. If you you wrap separator(s) in parentheses, everything will be included in the output, including the separators as in:
```py
import re

result = re.split(r"([? ])", "What are you?Nothing") 
print(result) # ['What', ' ', 'are', ' ', 'you', '?', 'Nothing']
 ``` 

### Replacements:
- Replacements are done with the **`sub`** functions which works as follows:
```py
import re

result = re.sub(r"\d{3}-\d{3}-\d{4}", "[PHONE NUMBER]", "101-101-2020 called at 22:50")
# returns '[PHONE NUMBER] called at 22:50'
```
- This function returned the targeted string but with specified pattern replaced by the substitution pattern.
- `sub` also works like the (in)famous `sed`. You can use it along group capturing tot shuffle hings around as in:
```py
import re

result = re.sub(r"^(\w+), (\w+)$", r"\2 \1", "White, Walter")
```

## Managing Data and Processes:
- This section is about interacting with the OS user and the live interactivity with the OS and processes while they are running!!
- One great interaction tool is the **`input`** function which accept user input in the terminal. It "prompts" the user to input arguments to the program. One stupid example that illustrate the idiomatic usage of this function is as follows:
```py
#!/usr/bin/env python3

print("Welcome to our amazing adder!")

cont = "y"

while cont == "y":
    a = b = ""
    a = input("Enter first num: ")
    b = input("Enter second num: ")
    print(int(a) + int(b))
    cont = input("Do you wanna perform another operation? [y to continue] ")

print("Good bye!")
```
- The most important thing about the example above is the variable `cont` and how it's used in a loop.

### Environment Variables:
- I forgot that you can access environment variables with typing **`env`** on the shell.
- Python gives you an out of the box function to retrieve environment variables. I believe this should work uniformly across OSs.
```py
import os

print ("HOME: " + os.environ.get("HOME", "")) # Don't get spooked by empty string. It's there to avoid error. Basically something iterates over these arguments and adds them to the some string.
```
- Basically, `os.environ` is a dictionary with variable names as it keys.
- You can define your own variables. Use the shell verb **`export`** with that so the variable can "be visible by any command we call":
```bash
 export ZAZA=12345
 ```

### Command-Line Arguments: 
- **`sys.argv`** from the **`sys`** module is the list of arguments provided to the Python script. The name of the script is always the first element in this variable.

### Exit Statuses:
- A shell program returns **exit status** which tells the shell and us if it executed successfully or not. If the exit status is 0, the program has executed successfully. If it's not 0, some error has occurred. In an automated world, exit statuses allow it to see if we can retry running the program or do something to fix the cause of the failure.
- The Unix shell *variable* **`$?`** (dollar sign is the Unix variable indicator) holds the value of the last executed command. You can see its value by echoing it:
```bash
echo $?
```
- In Python, you can specify an exit status in the **`sys.exit(<exit_status_value>)`** function. Programs that run successfully always return 0, by default. You'd usually supply exit status values in case of errors as part of conditional or whatever!

### `eval()`:
- `eval()` is another way of getting user input just like `input()`, but instead of treating the input as a raw string, it would instead evaluate it if it is a valid Python expression.

### Subprocesses:
- One way to call system commands from python is the following:
```py
import os

os.system('date')
```
- What seems more *pythonic* is the using the **`subprocess`** module instead:

```py
import subprocess

result = subprocess.run(["ls", '-al'])

print(result.returncode)
```
- The **`subprocess.run()`** method takes a list of strings which represent the command and its arguments and options.
- The object return by `run` has the `returncode` attribute which is the exit status of the run subprocess.
- We use `run` when we only care about the exit status indicating weather the command ran successfully or not. There are other commands that do more when were are concerned by the output of a command.  

#### Capturing Subprocess Output:
- To actually capture the output of a subprocess, you need to add an optional argument and **`capture_output`** and set it to `True` as in. The object returned by `subprocess.run()` now has an attribute `stdout` which has the output of the run command: 
```py
import subprocess

result = subprocess.run(["ls", '-al'], capture_output=True)

print(result.stdout.decode())
```
- `stdout` is an array of bytes rather than a proper string. You can convert to a proper string of characters with the `decode()` method.
- If the script fails, then `stdout` will be empty, and the `stderr` attribute instead capture the error output.

#### Advanced Subprocess Fiddling:
- The command you want to run as a subprocess might not necessarily live in the current directory, so you will need to join it and some some voodoo with the environment variable. The following example illustrates such a a situations:
```py
import os, subprocess

thisEnv = os.environ.copy()
thisEnv["PATH"] = os.pathsep.join([".." ,thisEnv["PATH"]]) #pathsep supplies appropriate separator based on OS

result = subprocess.run(["py.py"], env=thisEnv) # Notice env parameter?
```
- You can equally change current working directory `cwd` to execute commands into different directories.
- You can also set a `timeout` option to kill a process if it runs more than a specified number of seconds.
- There is also the shell variable which can be useful but can also pose security risks. I don't know what to do with this. Probably it's explained in next section.

#### Problems with Subprocesses:
- Subprocesses involve interacting directly with a system which makes dirty and less robust. Baked in functionality that achieve the same results as subprocesses are more robust and portable. If you can avoid subprocesses and use baked in functionality then do that by all means!

## Bash and Bash Scripting:
- *I am repeating myself a little here*.

### Redirecting IO:
- Some of the less obvious aspects of IO redirection:
	- **`someScript.py < someFile.txt`** means the script needs to take its input from the file. Something like the input for the `input` function of Python.
	- **`someScript.py 2> errorFile.com`** means directs standard error (whose file descriptor is obviously 2) to the named file.
- You can also pipe the output of one file (including a Python script) into the input of another (including a Python script).

- Imagine you have the following two programs `c.py` and `a.py`:
```py
# c.py
print(333 * 33 + 50000000)
```
```py
# a.py
print("This is your result: " + input())
```
- We can pipe the output of `c.py` into the input of `a.py` as shown here:
```bash
c.py | a.py
```
- One cool pipeline:
```bash
cat someFile | tr ' ' '\n' | sort | uniq -c | sort -nr
```
- I just stole this pipeline because it has several useful commands (some of which I saw in the *Unix Programming Environment*):
	- **`tr ' ' '\n'`** or *translate* replaces spaces with new lines.
	- **`sort`** sorts lines alphabetically.
	- **`uniq`** removes redundant adjacent lines. The **`-c`** option records the count of repetitions of a line.
	- The **`-n`** option on the last sort is for sorting numerically (this is a little ambiguous). The **`-r`** options means sorting in reverse order.

### Signaling Processes:
- Using Bash or Python or the keyboard, we can send different signals to running processes to stop them, resume them, terminate them, etc. Examples of this are the:
	- `Ctl+C` command to terminate a running process.
	- `Ctl+Z` command to stop but not terminate a process.
	- `fg` command to resume execution of a stopped processes.
- **`kill`** commands kill a process by taking the process's 'pid'. The following command search for a process containing the word python, and kills the first occurrence of it:
```bash
kill $(ps | grep -m1 python | cut -d ' ' -f 1)
```
- When you type **`ps ax`** all the processes running in the system are printed to the screen.
- Some other usefull commands include:

| Command name | What it does |
| --- | --- |
| **`chown`** | Change owner of files |
| **`chgrp`** | Change group of files |
| **`file`** | Shows type of file |
| **`head`** | Prints first 10 lines |
| **`tail`** | Prints last 10 lines |
| **`less`** | Scrolls through content of a file |
| **`cut -d " " -f 1`** | Cut line based on a given separator. Prints specified chunk. |
| **`who`** | Prints logged users |
| **`uptime`** | Shows how long the computer has been running |
| **`ps ax`** | Prints all running processes |
| **`ps e`** | Shows the environment for listed processes |
| **`fg`** | Makes a stopped or background job to come to foreground |
| **`bg`** | Makes a stopped job to go to background |
| **`top`** | Shows processes that consume most of the CPU |

### Bash Scripting:
- If you find yourself repeatedly using multiple commands together, it might be advisable to store these commands in a bash script file, something like the following:

```bash
#!/bin/bash

echo "Starting at + $(date)"
echo

echo "UPTIME"
uptime
echo

echo "WHO"
who
echo

echo "PS"
ps
echo

echo "Ending at + $(date)"
```

- You can alternatively do it like this, using comma separators between short and related commands:
```bash
#!/bin/bash

echo "Starting at + $(date)"
echo

echo "UPTIME"; uptime; echo

echo "WHO"; who; echo

echo "PS"; ps; echo

echo "Ending at + $(date)"
```

#### Variables:
- A variable is a defined with something like **`variable="value"`**. To access the variable, you prefix its name with a dollar sign **`$variable`**.
- You need to **`export`** your variable so it's added the current environment. This will make it available to scripts (child processes).

#### Globs:
- **Globs** in Shell speak refers to something similar to regex meta characters, namely the symbols **`*`** and **`?`** to handle multiple files in different flexible ways. Let's say you want to do something with a set of files whose names share a certain pattern. You can obtain a list of such files with globs. Examples of their use:
	- **`echo t*`** echoes the names of files starting with `t`.
	- **`*.*`** all files with an extension.
	- **`?????`** all files whose names consists of 5 characters.
	- **`*`** all files. 

#### Bash Conditional:
- A conditiona is constructed as follows (notice the `then` and `fi` which are not normal in C-like languages like Python):
```bash
if grep "some pattern" someFile.bash
then
	echo "Found it!"
else
	echo "Nothing!"
fi
```
- An alternative one liner is constructed as follows:
```bash
if grep "some pattern" someFile.bash ;then echo "Found it!"; else echo "Nothing!"; fi;
```

#### `test`:
- The conditionals we saw above act based on the exit status of a command. Sometimes you need to act on some data you provide to the conditional.The **`test`** keyword is used to check conditionals such as equality of variables or values, if a string is empty, etc. The following example is self-evident:
```bash
if test ""; then echo "Fully full"; else echo "Empty"; fi
```
- A more popular alternative to `test` is surrounding the conditional with brackets and space as in the following example which does the same thing as the last script:
```bash
if [ "" ]; then echo "Fully full"; else echo "Empty"; fi
```

#### `while` Loop:
```bash
n=1
while [ $n -le 5 ]; do
	echo "Count is $n"
	((n+=1))
done
```
- Notice how arithmetic expressions are evaluated in bash (between double parentheses).
- A more involved while loop does actually retry runnng a another script  or command up to a certain number of times if that command fails for some reason like like network down or something:
```bash
#!/bin/bash

n=0
command=$1 # $1, $2 etc. refer to argument 1, argument 2, etc which is exit status of run program 

while ! $command && [ $n -le 5 ]; do # Command is input and is rerun when it's not 0
    sleep $n 
    ((n+=1)) # sleep value keeps growing
    echo "Retry #$n"
done
```
- You can test or mock the behavior of the last program with the following script:
```py
#!/usr/bin/env python

import sys, random

value = random.randint(0,5)
print("Returning: " + str(value))
sys.exit(value)
```

#### `for` Loop:
- The following loops is over the list `a b c d`:
```bash
for item in a b c d; do
	echo $item
done
```
- The following loop is over files using globs:
```bash
for file in *.sh; do 
	name=$(basename "$file" .sh)
	mv "$file" "$name.bash"
done
```
- Notice the dollar sign preceding a command surrounded by parentheses. We can do these when we want to store the result of running a command in a variable or if we cant to echo that result. We might do it in other situations.

#### Bash Arrays:
```bash
myArray=(1 2 3 4 5) # Create array

myself+=(1000) #append alement to array

# loop over the array
for i in ${myArray[@]}; do # @ loop till last element in array.
	echo $((i * 3));
done;
```

تم بحمد الله
