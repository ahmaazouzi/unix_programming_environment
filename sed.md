# Sed:
## Contents:

## Intro:
- **`sed`** is a Unix program which can also be considered a language in itself. It's not very precisely defined and its implementations across Unix-like systems have differences that are mostly trivial, but one should still be aware of them. This document will strictly focus on the GNU specification of `sed`.
- `sed` stands for **stream editor**. It is a Unix utility that takes an input stream, performs some transformation on it. It was created by Lee McMahon in 1973.
- `sed` relies heavily on regular expressions and can be very efficient and great for performing simple transformation on text. It does things that can also be done in other programming languages like Python or Perl, or other Unix utilities like `tr` and `grep`. 
- The most used parts of `sed` are simple, efficient and can be done in simple one liners. However, it can very quickly become cryptic and hard to reason about.
- This document will focus on the easy and simple parts. It will also venture into some of the more cryptic aspects of `sed` where the benefit outweighs the crypticness. *(This word probably doesn't exist).*

## Syntax:
```
<address><command>[options, ...]
```
- The **address** or **restriction** is optional. It can be a line numbers or patterns. It can specify specific line or lines, or ranges of lines.
- The **command** is a single letter like **`s`** for substitution.
- **Options** are also optional and there can be several of them allow for granular control of how the command works.
- A **grouping** of multiple commands is also possible and it can be done with braces **`{}`**, but it seems like grouping might not really be necessary except for limiting edits to specified adresses/ranges:
```
'{
<command1>
<command2>
<command3>
}'
```
- This is equivalent to:
```
<command1>; <command2>; <command3>;
```

- **Delimiters** are used in various types of expressions. The default delimiter is slash **`/`**, but you can use any ascii character as a delimiter. The following exmples are equivalent:
```sh
sed 's/a/b/g'
sed 's@a@b@g'
sed 's a b g'
sed 'svavbvg'
sed 's^a^b^g'
```
- I'll cover more syntax where appropriate.

## Shell `sed` Options:

| Option | action |
| --- | --- | 
| **`-e`** | Allows running multiple sed commands |
| **`-f`** | For executing commands from a sed scriptfile  **`sed -f scriptfile file`**|
| **`i`** | Changes a file in place (instead of printing to standard output) |
| **`-n`** | Doesn't print output unless otherwise specified in the script |
| **`-r`** | Uses extended Perl regex instead of the primitive sed regexes |

## Simple Commands that Only Modify Lines:

| Command | Action |
| --- | --- |
| **`s/foo/bar`** | Changes `foo` to `bar` |
| **`n`** | Prints the current line in the pattern space and replaces it in the pattern space with the next line. sed -n `p;n;p` prints current and next line. |
| **`p`** | Prints current pattern space  |
| **`d`** | Deletes the pattern space and immediately starts cycle for next line. Immediately means any actions following `d` don't run. |
| **`q`** | Quits processing |  |
| **`y/foo/bar/`** | Translates chars in `foo` to `bar`. Examples include translating between cases.  |
| **`=`** | Prints line numbers. `sed '$='` means print number of last line in file. |
| **`l`** | Show all characters and invisible characters like dollar signs for line endings. |

## Commands that Insert Lines:
- Can be used either as:
```bash
sed 'i something' file
```
- Or:
```bash
sed 'i \
something' file
```
- We can add multiple lines with each command as long as the command and each line to be added is followed by a **`\`**:
```bash
sed 'i \
Something \
Another thing \
Yet another thing \' file
```

## The **`s`** Command:
- When substituting you can refer back to the pattern:
	**`&`** refers back to the whole expression.
	- **`\1`**, **`\2`**, **`\2`**, etc. Act like group expressions in regular regex. Groups in sed are defined with escaped parentheses: **`\(\)`**. 

| Command | Action |
| --- | --- |
| **`i`** | Add a line before the current line |
| **`a`** | Add line after the current line |
| **`c`** | Changes the current line. Seems to act like `y` or `s` |

## Flags:
- **`s`** (not sure about other commands) have flags controlling the commands behavior. They include:

| Flag | Action |
| --- | --- |
| **`g`** | Match all in line |
| **`NUMBER`** | Match NUMBER times |
| **`p`** | print if substituted |
| **`i`** | Case insensitive |

## Addresses and Ranges:
- You can select a subset of lines to apply commands to. You can use line numbers or regex patterns to do these selections.
- An line number as an address:
```bash
sed -n '1 p' file # Prints first line
sed '$ d' file  # Deltes last line
```
- A regex as an address:
```bash
sed '/^foo/ sed/foo/bar/' file # Match only lines starting foo and change foo to bar
```
- You can also select ranges of lines:
	- With numbers as in: `sed '1,4 sed/foo/bar/' file`
	- With regexes `sed '/AAA/BBB/ sed/foo/bar/' file`.
- Ranges will also include the lines where the addresses match. There are tricks to exclude those lines, but I will not cover those here. 

## Command Grouping with `{}`:
- What is important about the braces is that they define a scope where commands would have an effect. They are usually used to delimit a scope after an address/restriction as in:
```bash
sed -n '$ {
	G
	s/\n/ /g
	p
}
H' File
```
- In this examples the commands inside the braces only apply to the last line while, while the `H` command applies to all other lines.


## Pattern Space and Hold Buffer:
- The **pattern space** is a buffer where a line being transformed in held. It's flushed after each line is processed. It can grow and hold more than one line when certain commands are used like **`N`**.

| Command | Action |
| --- | --- |
| **`N`** | Join the current and next lines into one so you can treat the two as one. You can perform matching and replacement on a pattern that spans these two lines. |
| **`P`** | Prints the pattern space until the first new line. This is in a line that consists of two lines that were joined by `N`. |
| **`D`** | Deletes the first line in the pattern space. |


- If you place lines in the **hold space**, lines stay there and don't disppear because of line changes.

| Command | Action |
| --- | --- |
| **`h`** | Replaces hold space with pattern space. |
| **`H`** | Appends pattern space to hold space. |
| **`g`** | Replaces pattern space with hold space. |
| **`G`** | Appends pattern space with the content of hold space. |
| **`x`** | Exchanges the contents of the hold and pattern spaces. |

- The following script turn all new lines in a file into spaces, turning the lines in a file into a long line separated by spaces:
```bash
sed -n '$ {
	G
	s/\n/ /g
	p
}
H' File
```

## Some Weird Corners:
- To change something to a new line, you actually need to type an actual new line preceded by an inverted slash. The following example changes all spaces into new lines:
```bash
sed 's/ /\
/' file 
```
- When matching a new line, you can use the good old **`\n`**. You need the actual new line and inverted slash only as a replacement character.
- When not using extended regex, you need to escape parentheses and braces with inverted slashes: **`\(something\)`**, **`\{NUMBER\}`**.
