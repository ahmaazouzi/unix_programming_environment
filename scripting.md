## Python and the OS: 
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
| `os.path.join(dir, name)` | join some string (file name) with a directory path 
_intelligently_, so it's portable across OSs |




