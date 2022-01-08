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


