# Shell

This tutorial is based off of Zed Shaw's [The Command Line Crash Course](http://cli.learncodethehardway.org/book/).
This free online book provides additional practice and explanation of introductory concepts.

For more complex commands, [Explain Shell](http://explainshell.com/) is a really
useful resource for breaking up convoluted code into its constituents, and
explaining each step of the command.

##Escaping the GUI mindset

Up until this point, most of your interactions with computers have probably been
through a "graphical user interface", or GUI. In fact, it's probably so deeply
ingrained in your day-to-day usage of computers that you've never even really
thought about it. When you use your mouse to click folder icons, a browser to
view web pages, or programs to open files, you're using the GUI portion of your
operating system.

There is absolutely nothing wrong with this; GUIs are great at simplifying common
tasks and reducing learning curves. When a GUI overlaps with what you want to do,
you should use it!

What really matters here is conceptual. In order to advance as a coder, you'll
have to mentally decouple the GUI from the underlying computer. This concept is
easy to memorize and recite, but it may take a while to actually "click". Don't get
discouraged by this - you'll get there with practice!

##Shell? Terminal? Bash? CLI? What is all of this?

When you do something in your GUI, your action gets converted into a command
for the computer to run. The *shell* is a program that presents a command line
interface (*CLI*) which allows you to control your computer using commands
entered with a keyboard (instead of through the GUI). The most prevalent type
of shell is referred to as *bash* (for **B**ourne **a**gain **sh**ell). The
*terminal* is a program you run to access the shell.

While all of these terms refer to slightly different concepts, they're generally
used interchangeably in practice. The most popular term is "shell", but you'll
encounter the other terms as well.

Below is a quick cheat sheet to some of the commands we'll explore in the basic and advanced shell sections.

## 1. Shell Basics:

| Command | Definition |
|---|---|
| `.` | a single period refers to the current directory |
| `..` | a double period refers to the directory immediately above the current directory |
| `~` | refers to your home directory. _Note:_ not on Windows |
| `cd dirname` | changes the current directory to the directory `dirname` |
| `ls` | tells you what files and directories are in the current directory |
| `pwd` | tells you what directory you are in (stands for **p**rint **w**orking **d**irectory) |
| `history` | lists previous commands you have entered |
| `man [command]` | displays the **man**ual page for a command |

## 2. Creating Things:
### a) How to create new files and directories..

| Command | Definition |
|---|---|
| `mkdir dirname` | makes a new directory called dirname in the current directory. _Note:_ Windows is `mkdir .\dirname` |
| `touch filename` | makes a new empty file called filename in the current directory |
| `open filename` | attempts to open the file with default program. Analogous to double-clicking a file in a GUI file manager |
| `[program] filename` | specify a program to open a file. A good intro program is `nano` or `gedit`; more complex editors such as `vim` and `emacs` also exist |
| `cat filename` | prints the contents of the file to stdout. Useful for piping, discussed below |

### b) How to delete files and directories...

| Command | Definition |
|---|---|
| `rm filename` | deletes a file called `filename` from the current directory |
| `rm -r dirname` |  deletes the directory `dirname` from the current directory |

 **Remember that deleting is forever. There is NO going back.** It is generally
 good practice to avoid shorthand or wildcards (ie. `*`, `/`, `.`) when removing
 files and folders.

### c) How to copy and rename files and directories...

| Command | Definition |
|---|---|
| `mv tmp/filename .` | moves the file `filename` from the directory `tmp` to the current directory. _Note:_ _(i)_ the original `filename` in `tmp` is deleted. _(ii)_ `mv` can also be used to rename files (e.g., `mv filename newname`) |
| `cp tmp/filename .` | copies the file `filename` from the directory `tmp` to the current directory. _Note:_ the original file is still there |

## 3. Pipes and Filters
### a) How to use wildcards to match filenames...
Everything up to this point has been emulating things you could easily do in your GUI.
That changes in this section. In addition to regular file names, shell commands accept "wildcards" that can match
multiple multiple files and folders.

#### Table of commonly used wildcards

| Wildcard | Matches |
|---|---|
| `*` | zero or more characters |
| `?` | exactly one character |
| `[abcde]` | exactly one of the characters listed |
| `[a-e]` | exactly one character in the given range |
| `[!abcde]` | any character not listed |
| `[!a-e]` | any character that is not in the given range |
| `{software,carpentry}` | exactly one entire word from the options given |

The combination of wildcards to match complex patterns builds up to a topic
referred to as "regular expressions" or "regex", and has capabilities far beyond the wildcards shown.
See the cheatsheet on regular expressions for more "wildcard" shortcuts, Zed
Shaw's [Learn Regex The Hard Way](http://regex.learncodethehardway.org/), and
[Rubular](http://rubular.com/) for playing around with and testing ruby-flavored regex.

### b) How to redirect to a file and get input from a file ...
Redirection operators can be used to redirect the output from a program from the
display screen to a file where it is saved.

| Command | Description |
|---|---|
| `>` | write `stdout` to a new file; overwrites any file with that name (e.g., `ls *.md > markdownfiles.txt`) |
| `>>` | append `stdout` to a previously existing file; if the file does not exist, it is created (e.g., `ls *.md >> markdownfiles.txt`) |
| `<` | assigns the information in a file to a variable, loop, etc (e.g., `n < markdownfiles.md`) |

In advanced usage, you can pipe to and from more than just files. Other programs,
databases, and even your printer can accept piped data.

#### b.1) How to use the output of one command as the input to another with a pipe...
A special kind of redirection is called a pipe and is denoted by `|`.

| Command | Description |
|---|---|
| <code>&#124;</code> | Output from one command line program can be used as input to another one (e.g. <code>ls \*.md &#124; head -n 5</code> gives you the first 5 `*.md` files in your directory) |

##### Example:

```
ls *.md | grep carpentry | xargs sed -i "s/markdown/software/g"
```

changes all the instances of the word "markdown" to "software" in all `.md` files
in your current directory with the word "carpentry" in the file name. Most of
the commands shown here are new - use `man` to learn more about them!

Here are two useful analogs of the above example for code cleanup.

```
# Replaces tabs with four spaces in all Python files in the current directory
ls *.py | xargs sed -i "s/\t/    /g"
# Removes trailing whitespace in all C++ files in the current directory
ls *.cpp | xargs sed -i "s/\s\+$//g"
```

## 5. Finding Things
### a) How to select lines matching patterns in text files...
To find information within files, you use a command called `grep`.

| Example command | Description |
|---|---|
| `grep [options] day haiku.txt` | finds every instance of the string `day` in the file haiku.txt and pipes it to standard output |

#####Why is it called "grep", and not something logical (like "search")?

Especially with older languages (like bash), the genesis and transformation of
commands and syntax is a messy evolutionary process. There's a lot of random naming,
support for now-defunct standards, and multiple ways to do the same task. In the
specific example of grep, it comes from the [ed](http://en.wikipedia.org/wiki/Ed_(text_editor)) command g/re/p, which stands for
(**g**lobally search a **r**egular **e**xpression and **p**rint).

#### a.1) Commonly used `grep` options

| | `grep` options |
|---|---|
| `-E` | tells grep you will be using a regular expression. Enclose the regular expression in quotes. _Note:_ the power of `grep` comes from using regular expressions. Please see the regular expressions sheet for examples |
| `-i` | makes matching case-insensitive |
| `-n` | limits the number of lines that match to the first n matches |
| `-v` | shows lines that do not match the pattern (inverts the match) |
| `-w` | outputs instances where the pattern is a whole word |

For all the options, use `man grep`.

### b) How to find files with certain properties...
To find file and directory names, you use a command called `find`

| Example command | Description |
|---|---|
| `find . \*.js` | searches all subdirectories of the current directory (`.`) for javascript (`.js`) files |

#### b.1) Commonly used `find` options

| | `find` options |
|---|---|
| `-type [df]` | `d` lists directories; `f` lists files |
| `-maxdepth n` | `find` automatically searches subdirectories. If you don't want that, specify the number of levels below the working directory you would like to search |
| `-mindepth n` | starts `find`'s search `n` levels below the working directory |

##6. Environment variables
Your shell contains variables that are accessible to all programs and commands
you run. These are referred to as "environment variables", and are generally
used in configuring various programming languages and libraries.

To view your current environment variables, type `env`. To add a variable to
your environment, use `export`. To use a variable in a command, prepend the
variable name with a `$`. For example,

```
# Add a new environment variable
export DESKTOP=~/Desktop
# Check that it's there with the env command, a pipe, and grep
env | grep DESKTOP
# Use the environment variable in another command by prepending a $
cd $DESKTOP
```

##Bonus: Escaping the local mindset with `ssh` and `scp`

Up until this point, all commands have been running "locally", or on your own
computer. This probably seems normal to you because you're used to years of
practice with GUIs. Moreso, you're used to running computationally intensive
tasks (ie. video games) locally, and watching your laptop struggle to keep up.

Once you abandon the GUI mindset, however, you'll find that you can entirely
remove the notion of "local". In fact, you can code on "remote" machines just
as easily as local ones. This means that you can code on and run complex
computationally intensive tasks on desktops, supercomputers, and even cloud clusters
from your laptop.

We'll leave this as an exercise to you. Type `man ssh` for more.
