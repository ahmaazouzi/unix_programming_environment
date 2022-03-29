# Shell Scripts:
## Table of Content:
* [Debugging Shell Scripts](#debugging-shell-scripts)
* [Shell Variables](#shell-variables)
	+ [Special Shell Positional Variables](#special-shell-positional-variables)
	+ [Reading Parameters In](#reading-parameters-in)
	+ [Parameter Expansion](#parameter-expansion)
* [Arithmetic](#arithmetic)
* [Control Flow](#control-flow)
	+ [Tests](#tests)
	+ [The `case` Command](#the-case-command)
	+ [Fizzbuzz Example](#fizzbuzz-example)
* [Useful Shell Programs for Text Manipulation](#useful-shell-programs-for-text-manipulation)
	+ [`cut`](#cut)
	+ [`tr`](#tr)

## Debugging Shell Scripts:
- To write correct shell scripts try doing the following tricks"
	- Be generous with the comments.
	- Use `echo` extensively to, for example, print dummy text representing commands. This allows you to test loop and conditional construct and how they behave with variable changes and all. 
	- Keep the code organized and readable. `bash` is an old ugly language and code in it can very easily become unreadable.
- Another useful trick is to add **`set -x`** near the top of your script or run your script with the **`-x`** option as in:
```sh
bash -x myscript
```
- What `-x` does above is print every command that gets executed by your script. This might help find bugs associated with silent errors and whatnot.

## Shell Variables:
- To access variables, we use the special character **`$`**. We can assign text to variables, or we can also use output from commands as variable values as we've seen before:
```sh
backthen=$(date)
files=`ls`
```
- Sometimes you might need to escape special characters, especially when trying to print something. You will need to escape special characters such as **`$`**, **`!`**, etc. There are two ways to escape a special shell character:
	- To escape a single special character, precede it with a backslash **`\`** as in **`echo \$HOME`**.
	- To escape multiple special characters, you enclose everything in single quotes as in **`echo '$HOME'`**. Single quotes escapes all special characters. This might be tricky to python and javascript programmers who treat single quotes and double quotes as the same. Double quotes in bash does not escape special characters which is confusing to some but is also flexible
- Another somehow tricky thing is a variable can contain the value of another variable. Our variable keeps the old value of the other variable, meaning our variable preserves its old value.

### Special Shell Positional Variables:
- There are some special characters that the shell itself assigns. These include **positional paramters** which are basically command line arguments that a script takes like **`$1`, `$2`, `$2`, `$n`**. They represent arguments to a command in the order in which they were entered. The special argument **`$0`** refers to the command/script name itself.
- Other very important special shell variables include:

| Special variable | Value |
| --- | --- |
| **`$#`** | Refers to the number of all parameters to a script. |
| **`$@`** | Holds arguments given to the command. |
| **`$?`** | Holds the exit status of the last run command. |

### Reading Parameters In:
- Parameters are read in using the **`read`**. Refer to the here docs which are much more useful in this respect.

### Parameter Expansion:
- **`$VAR`** is a shorthand for **`${VAR}`**. **`${VAR}`** is more general and be used as a part of a word in a string like '${VAR}GB' because `$VARGB` would be a different name than `$VAR`. Shell variable can do more complex things and new variables can be extracted from other variables using so-called variable expansion or parameter expansion.
- The following table lists some of the ways variables can be expanded:

| Variable Expansion | Meaning |
| --- | --- |
| **`${VAR:-VALUE}`** | This is something like Python's default argument. If variable hasn't been declared (*set*) or is empty, set it to `VALUE` |
| **`${VAR#PATTERN}`** | Remove the shortest match to `PATTERN` from the front of `VAR`'s value | 
| **`${VAR##PATTERN}`** | Remove the longest match to `PATTERN` from the front of `VAR`'s value |
| **`${VAR%PATTERN}`** | Remove the shortest match to `PATTERN` from the end of `VAR`'s value |
| **`${VAR%%PATTERN}`** | Remove the longest match to `PATTERN` from the end of `VAR`'s value |

- Let's look at some examples to understand how these word. Let's start with **`${VAR:-VALUE}`**:
```bash
gorwell="1984"

book1=${gorwell:-"unknown title"}

echo $book1 # prints '1984'

book2=${book2:-"unknown title"}

echo $book2  # prints 'unknown title'
```
- Because we set `book1` to an actual variable with a value, we got that value. For `book2`, we set it to a non-existent not set value so the value defaults to `"unknown titles"`. This seems like a safety measure to avoid unexpected results.
- Imagine we have a filename **`/home/am/firstShellScript.sh`** stored in a variable **`FILENAME`**. The following examples show how we can extract the different components of such a file name using variable expansion constructs:
```sh
FILENAME="/home/am/firstShellScript.sh"
FILE=${FILENAME##*/}
DIRECTORY=${FILENAME%/*}
NAME=${FILE%.*}
EXTENSION=${FILE##*.}
```
- ***This seems very powerful. I know this can be done with some convoluted regex and stuff, but this seems kinda easier.***

## Arithmetic:
- The shell consider all variables, including numbers, as just strings. When you try to increment or do any other kind of arithmetic on these numbers, bash converts them to integers.
- The following table lists the different ways you can do arithmetic:

| Expression | Notes |
| --- | --- |
| **`let a=1000/2`** | If you simply type `a=1000/2`, the value of `$a` would be the string "1000/2". With `let`, it is calculated as 500. No spaces between operands and operators. |
| **`a= expr 1000 / 2`** | Same as above. `expr` evaluates the arithmetic expression and stores the result in a. Requires spaces between operators and operands. |
| **`a= echo "1000 / 2" \| bc`** | `bc` seems to take its input from io, maybe a file or something and does the evaluation with that. Doesn't care about spaces, but it does floating-point arithmetic. |
| **`$((a++))`** | Double quotes preceded with a `$` change the value of `a`, increment it, decrement it. This doesn't return something, so it basically performs a side effect. |

- One useful built-in variable is **`$RANDOM`** that allows you create random (or pseudo-random) values you can use for various purposes. 

## Control Flow:
### Tests: 
- Bash syntax is total garbage by today's standards. At the heard of some important control flow statements is the **test** constructs that are used to test for different conditions. There are at least 3 of these:

| Test notation | Notes |
| --- | --- |
| **`test $a -lt 100 && test $a -gt 0 `** | Seems to require a lot of typing |
| **`[[ $a -lt 100 && $a -gt 0 ]]`** | Less typing but still clunky. This is probably the best way to stick to. |
| **`[ $a -lt 100 ]`** | This is good for one conditional, but trying to combine multiple tests with OR or AND doesn't work. |

- Quick help about test can be easily found with **`help test`** or **`man test`**. The reason for mentioning this is the fact that are there are many testing operators that you can use with anything. The good bad news is that there are different operators that should be just one, for example we have **`-eq`** for comparing numbers, and **`=`** which works better for strings. Not equal is easy for strings **`!=`**, but for numbers, you'd need **`-ne`**. The good news, however, is that you have a lot of these operators that so much and that are deeply integrated with the system and do many great things with files. Examples include:

| Example | Description |
| --- | --- |
| **`-a someFile`** | Does the file exist? |
| **`-f someFile`** | Is the file a regular file? |
| **`-O someFile`** | Do I own the file? |
| **`-r someFile`** | Is the file readable by me? |

- You can use any of these operators and tests not only as part of `if` or `while` statements, but you can also use them in standalone commands. The following example checks if a file exists and that it is a directory. If not it creates a directory with that name:
```sh
[ -d someFile || mkdir someFile ]
```
- ***Powerfullll!!***
- Another one checks if a file exists and that it is a regular file. If it does, it writes to it:
```sh
[ -f someFile && echo 'something' >> someFile ]
```
- These shortcuts involving test can even replace whole `if-else` statements as the following example shows:
```sh
[ -e $dirname ] && echo $dirname already exists || mkdir $dirname
```
- The example  checks if `$dirname` exists. If it does, we print that it exists, otherwise we create a new directory with that name.

### The `case` Command:
- This is similar to the `switch` statement from other languages. It has the following general format:
```sh

case "VAR" in 
	Result1)
		{ body };; 
	Result2)
		{ body };; 
	*)
{	 body };; 
esac
```
- The **`*)`** construct is equivalent to the switch dfault block.
- A cool example of how it's used is this backup scheduling script:
```sh
case `date +%a` in
	"Sat" | "sun" )
		echo "It's the weekend"
		;;
	*)
		echo "We still have a long week."
		;;
esac
```

### Fizzbuzz Example:
- It took some time to get this to work. I think it kinda summarizes much of what how goes into flow control in bash:
```bash
i=1

while [[ $i -le 100 ]]
do
    if [[ $(($i % 15)) -eq 0 ]]
    then
        echo "Fizzbuzz"
    elif  [[ $(($i % 5)) -eq 0 ]]
    then
        echo "Buzz"
    elif  [[ $(($i % 3)) -eq 0 ]]
    then
        echo "Fizz"
    else
        echo $i
    fi  
    ((i++)) # or 'let i=$i+1'
done
```

## Useful Shell Programs for Text Manipulation:
- Linux has also inherited many very useful utilities for text manipulation. They include, **`grep`**, **`sed`**, **`cut`** and **`tr`**. I have used and covered `grep` and `sed` elsewhere, so I will just focus on `cut` and `tr` here.

### `cut`:
- `cut` is used to extract a file into fields based on a given separator. This is very useful in extracting specific fields from large configuration files which are largely well organized and whose content follow specific patterns. Let's say, you'd have a name field, a date field, etc. Fields would be separated by something like a space, tabe or some other symbol. You'd `cut` each line using a given delimiter and then only use the fields you care about.
- The `-d` option specifies the delimiter, while something like `-f1` is for field 1, `-f2` is for field 2, etc. Check the following example:
```sh
 grep /home /etc/passwd | cut -d':' -f6 -
```
- This script looks for the pattern `/home` inside the `/etc/passwd` file. It grabs all lines containing regular users. It then cuts them using the `:` delimiter. It then keeps only the 6th field and prints it to screen.
- The hyphen at the end of the script means cut can take its input from standard input or from a pipe.

### `tr`:
- Allows you to change certain characters to another type of characters as in:
```sh
FOO="Mixed UPpEr aNd LoWeR cAsE"
echo $FOO | tr [:upper:] [:lower:]
```
