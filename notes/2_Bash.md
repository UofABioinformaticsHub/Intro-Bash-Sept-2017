* TOC
{:toc}

# Introducing Bash

## Initial Goals

1. Gain familiarity and confidence within the `bash` environment
2. Learn how to navigate directories, as well as to copy, move & delete the files within them
3. Look up the help page of a command needed to perform a specified task


An important step in many analyses is moving data around on a high-performance computer (HPC), and setting jobs running that can take hours, days or even weeks to perform.
For this, we need to learn how to write scripts which perform these actions, and the primary language for this is `bash`.

We can utilise `bash`in two primary ways:

1. Interactively through the terminal
2. Through a script which manages various stages of an analysis

For the first sections today we will work interactively, however a complete analysis should be scripted so we have a record of literally everything we do: *Reproducible Research*

## Bash

There are several variants of the shell, such as the Bourne-again shell (i.e. `bash`) and the C-shell `csh` (please feel free to make all the jokes you can think of).
The terminal has a library of commands which are built into it at the time of installing the operating system,
and which are part of the Bourne-again Shell, or bash.
(Historically, it’s a replacement for the earlier Bourne Shell, written by Stephen Bourne, so the name is actually a hilarious pun.)
We’ll explore a few of these commands below, and the words shell or `bash` will often be used interchangeably with the phrase terminal window.
Our apologies to any purists.
If you’ve ever heard of the phrase *shell scripts*, this refers to a series of these commands strung together into a text file which is then able to be executed as a single command, and which provides a record of every step performed.

# Finding your way around

Firstly we need to open a terminal as we did during the setup steps.
You will notice some text describing your computer of the form

 `user@computer:~$`


The tilde represents your current directory (see below), whilst the dollar sign just indicates the end of the address & the beginning of where you will type commands.
This is the standard interface for the Bourne-again Shell, or `bash`.
(Historically, `bash` is a replacement for the earlier Bourne Shell, written by Stephen Bourne, so the name is actually a hilarious pun.)
We'll explore a few important commands below, and the words shell and bash will often be used interchangeably with the terminal window.
Our apologies to any purists.

If you've ever heard of the phrase `shell scripts`, this refers to a series of commands strung together into a text file which is then able to be run as a single process.

## Where are we? (`pwd`)

Type the command `pwd` in the terminal and you will see the output which describes the `home` directory for your login.

```
pwd
```

The command `pwd` is what we use to print the current (i.e. working) directory.
This is what will be referred to as your *home* directory for the remainder of the workshop.
This is also the information that the tilde represents as a shorthand version, so whenever you see the tilde in a directory path, it is interpreted as this directory.

In the above command, the home directory began with a slash, i.e. `/`.
On a Linux-based system, this is considered to be the root directory of the file system.
Windows users would be more familiar with seeing `C:\` as the root of the drive, and this is an important difference in the two directory structures.
Note also that whilst Windows uses the backslash (\\) to indicate a new directory, a Linux-based system uses the forward slash (/), or more commonly just referred to simply as `slash`, marking another but very important difference between the two.

## Changing Directories (`cd`)

**Open your normal file/directory browser** and change to the directory indicated by the `pwd` command.
In this section we will learn how to change directories using `bash`.
Try to mirror this using your conventional GUI approach so you can understand what's actually happening.

Another built-in command is `cd` which we use to *change directory*.
No matter where we are in a file system, we can move up a directory in the hierarchy by using the command

```
cd ..
```

**NB: The space between the `cd` and `..` is important**

The string `..` is the convention for *one directory above*, whilst a single dot represents the current directory.

Enter the above command and notice that the location immediately to the left of the \$ has now changed.
This is also what will be given as the output if we entered the command `pwd` again.

```
pwd
```

If we now enter

```
cd ..
```

one more time we should be in the root directory of the file system.
Try this and print the working directory again (`pwd`).
The output should be the root directory given as `/`.

We can change back to the original location by entering one of either:

```
cd /home/your_login_name
```

(please use your actual login) or

```
cd ~
```

or even just

```
cd
```

### Relative Vs Absolute Paths

The approach taken above to move through the directories used what we refer to as a *relative path*, where each move was made relative to the current directory.
An *absolute path* in `bash` will always begin with the root directory symbol `/`.

For example, `/path` would refer to a directory called `path` in the root directory of the file system (NB: This directory doesn't really exist, it's an example).
In contrast, a *relative path* can begin with either the current directory (optionally indicated by `./`) or a higher-level directory (indicated by `../` as mentioned above).
A subdirectory `path` of the **current** directory could thus be specified as `./path`, whilst a subdirectory of the **next higher** directory would be specified by `../path`.
Another common relative path is the one mentioned right at the start of the session, specified with `~`, which stands for your home directory.

We can also move through multiple directories in one command by separating them with the forward slash `/`.
For example, we could also get to the root directory from our home directory by typing
```
cd ../../
```

Using the above process, return to your home directory `/home/your_login_name`.

In the above steps, this has been exactly the same as clicking through directories in our familiar folder interface that we're all familiar with.
Now we know how to navigate folders using `bash` instead of the GUI.
This is an essential skill when logged into an HPC or a VM.

### Important
{.:no_toc}

*Although we haven't directly discovered it yet, a Linux-based file system such as Ubuntu or Mac OS-X is also* **case-sensitive**, *whilst Windows is not.
For example, the command `PWD` is completely different to `pwd` and if `PWD` is the name of a command which has been defined in your shell, you will get completely different results than from the intended `pwd` command.*

## Looking at the Contents of a Directory (`ls`)

There is another built-in command `ls` that we can use to **list** the contents of a directory.
This is a way to get our familiar folder view in the terminal.
Enter the `ls` command as it is and it will print the contents of the current directory.

```
ls
```

This is the list of files that we normally see in our graphical folder view.
Check using this method if you'd like.

Alternatively, we can specify which directory we wish to view the contents of, without having to change into that directory.
We simply type the `ls` command, followed by a space, then the directory we wish to view the contents of.
To look at the contents of the root directory of the file system, we simply add that directory after the command `ls`.

```
ls /
```

Here you can see a whole raft of directories which contain the vital information for the computer's operating system.
Among them should be the `/home` directory which is one level above your own home directory.
This is actually where the home directories for all users are located and your home folder will be within this folder under your login name.
It's important to be aware that `/home` and your home folder `~` are actually two separate (but related) locations.

## Question
{.:no_toc}

 Try to think of two ways we could inspect the contents of the `/home` directory from your own home directory.

<div style="font-color:blue">
*Hint:
When working in the terminal, you can scroll through your previous commands by using the up arrow to go backward, and the down arrow to move forward.
This can be a big time saver if you've typed a long command with a simple typo, or if you have to do a series of similar commands.*
</div>

## Creating a New Directory (`mkdir`)

Now we know how to move around and view the contents of a directory, we should learn how to create a new directory using bash instead of the GUI folder you are used to.
If you already have a directory for this course, navigate to this directory using the `cd` command, remembering that your home directory is represented by the `~` symbol.

Now we are in a suitable location, let's create a directory called `Bash_Workshop`.
To do this we use the `mkdir` command as follows:

```
cd ~
mkdir Bash_Workshop
```

Importantly, this command can only make a directory directly below the one we are currently in.
If automating this process via a script it is very important to understand the difference between *absolute* and *relative* paths, as discussed above.

## Adding Options To Commands

So far, the commands we have used were given either without the use of any subsequent arguments, e.g. `pwd` & `ls`, or with a specific directory as the second argument, e.g. `cd ../` & `ls /home`.
Many commands have the additional capacity to specify different options as to how they perform, and these options are often specified between the command name, and the file being operated on.
Options are commonly a single letter prefaced with a single dash (`-`), or a word prefaced with two dashes (`--`).
The `ls` command can be given with the option `-l` specified between the command & the directory.
This options gives the output in what is known as *long listing* format.

*Inspect the contents of your current directory using the long listing format.
Please make sure you can tell the difference between the characters `l` & `1`.*

```
ls -l
```

The above will give one or more lines of output, and one of the first lines should be something similar to:

`drwxrwxr-x 2 your_login_name your_login_name 4096 Aug 7 hh:mm Bash_Workshop`

where `mmm dd hh:mm` are time and date information.

## File Permissions

The letter `d` at the beginning of the initial string of codes `drwxr-xr-x` indicates that this is a *directory*.
Most often, these letters are known as flags which identify key attributes about each file or directory, and beyond the first flag (`d`) they appear in strict triplets.

- The first entry shows the file type and for most common files, this entry will be `-`.
- The triplet values `rwx` simply refer to who is able to read, write or execute the contents of the file or directory.

These triplets refer to:

1. the file's owner,
2. the current user &
3. all users

Flags will only contain the values `r` (read), `w` (write), `x` (execute) or `-` (not enabled).
These are very helpful attributes for data security & protection against malicious software.
For example, if we wish to *write-protect* an important file, we can disable the `w` flag.

The entries `your_login_name your_login_name` respectively refer to who is the owner of the directory (or file) & to which group of users it belongs.
Again, this information won't be particularly relevant to us today, but this type of information is used to control who can read and write to a file or directory.
Finally, the value `4096` is the size of the directory structure in bytes, whilst the date & time refer to when the directory was created.

Let's look in your home directory (`~`).

```
ls -l ~
```

This directory should contain numerous files and folders.
There is also a `-` instead of a `d` at the beginning of the initial string of flags will help indicate the difference between files and folders.
On Ubuntu or git bash files and folders will also be displayed with different colours, but this may or may not be the case for those of you on OSX.

There are many more options that we could specify to give a slightly different output from the `ls` command.
Two particularly helpful ones are the options `-h` and `-R`.
We could have specified the previous command as

```
ls -l -h ~
```

This will change the file size to `human-readable` format, whilst leaving the remainder of the output unchanged.
Try it & you will notice that where we initially saw `4096` bytes, the size is now given as `4.0K`, and other file sizes will also be given in Mb etc.
This can be particularly helpful for larger files, as most files in bioinformatics are very large indeed.

The additional option `-R` tells the `ls` command to look through each directory recursively.
If we enter

```
ls -l -R ~
```

the output will be given in multiple sections.

The first is what we have seen previously, but following that will be the contents of each sub-directory.
It should become immediately clear that the output from setting this option can get very large & long depending on which directory you start from.
It's probably not a good idea to enter `ls -l -R /` as this will print out the entire contents of your file system.

In the case of the `ls` command we can also specify all the above options together in the command
```
ls -lhR ~
```

This can often save some time, but it is worth noting that not all programmers write their commands in such a way that this convention can be followed.
The built-in shell commands are usually fine with this, but many NGS data processing functions do not accept this convention.

## The *Panic* Button

It's easy for things to go wrong when working in the command-line, but if you've accidentally:

- set something running which you need to exit or
- if you can't see the command prompt, or
- if the terminal is not responsive

there are some simple options for stopping a process & getting you back on track.
Some options to try are: \\

| Command  | Result |
|:-------- |:------ |
| `Ctrl+c` | Kill the current job |
| `Ctrl+d` | End of input         |
| `Ctrl+z` | Suspend current job  |

`Ctrl+c` is usually the first port of call when things go wrong.
However, sometimes `Ctrl+c` doesn't work but `Ctrl+d` or `Ctrl+z` does.

A good example would be if you are curious and want to see what happens if you typed `ls -R /`.
As this would dump the entire contents of your computer onto the screen, you can kill the process at any time by entering `Ctrl+c`.


# Manuals and Help Pages

## Accessing Manuals

In order to help us find what options are able to be specified, every command built-in to the shell has a manual, or a help page which can take some time to get familiar with.
*These help pages are displayed using the pager known as* `less` which essentially turns the terminal window into a text viewer, so that we can display text easily in the terminal window, but with no capacity for us to edit the text.

To display the help page for `ls` enter the command

```
man ls
```

As beforehand, the space between the two is important & in the first word we are invoking the command `man` which then looks for the *manual* associated with the command `ls`.
To navigate through the manual page, we need to know a few shortcuts which are part of the `less` pager.

Although we can navigate through the `less` pager using up & down arrows on our keyboards, some helpful shortcuts are:

| Command    | Action |
|:---------- |:------ |
| `<enter>`  | go down one line |
| `spacebar` | go down one page (i.e. a screenful) |
| `b`        | go **b**ackwards one page |
| `<`        | go to the beginning of the document |
| `>`        | go to the end of the document |
| `q`        | quit |


Look through the manual page for the `ls` command and answer the following question:

## Question
{.:no_toc}

*If we wanted to hide the group names in the long listing format, which extra options would we need set when searching our home directory?*

We can also find out more about the `less` pager by calling it's own `man` page.
Type the command:

```
man less
```

and the complete page will appear.
This can look a little overwhelming, so try pressing `h` which will take you to a summary of the shortcut keys within `less`.
There are a lot of them, so try out a few to jump through the file.

A good one to experiment with would be to *search for patterns within the displayed text* by prefacing any pattern with a slash.
Try searching for a common word like *the* or *to* to see how the function behaves, then try searching for something a bit more useful, like the word *move*.

## Accessing Help Pages

As well as entering the command `man` before the name of a command you need help with, you can often just enter the name of the command with the options `-h` or `--help` specified.
Note the convention of a single hyphen which indicates an individual letter will follow, or a double-hyphen which indicates that a word will follow.
Unfortunately the methods can vary a little from command to command, so if one method doesn't get you the manual, just try one of the others.
We've already seen that the `-h` option for `ls` won't be likely to get you the help page.


*Hint:
Sometimes it can take a little bit of looking to find something and it's important to be realise we won't break the computer or accidentally launch a nuclear bomb when we look around.
It's very much like picking up a piece of paper to see what's under it.
If you don't find something  at first, just keep looking and you'll find it eventually.*


### Questions
{.:no_toc}

Try accessing the manual for the command `man` all three ways.
*Was there a difference in the output depending on how we asked to view the manual?*

# Some More Useful Commands

## Tab auto-complete

A very helpful & time-saving tool in `bash` is the ability to automatically complete a command, file or directory name using the `<tab>` key.
Move the the directory above the `Bash_Workshop` directory using the `cd` command.
(If you're already in this directory, you'll simply need `cd ..`).

Now **try typing `ls Bash` & then hit the `<tab>` key**.
Notice how `Bash_Workshop` is completed automatically!
This functionality will automatically fill as far as it can until conflicting options are reached.
In this case, there was only one option so it was able to complete all the way to the end of the file path.
Where multiple options are present, you can hit the `<tab>` key twice and all options will be given to you.

This can be used to also find command names.
Type in `he` followed by two strikes of the `<tab>` key and it will show you all of the commands that begin with the string `he`, such as `head`, `help` or any others that may be installed on your computer.
If we'd hit the `<tab>` key after typing `hea`, then the command `head` would have auto-completed, although clearly this wouldn't have saved you any typing.


## A series of commands to look up

So far we have explored the commands `pwd`, `cd`, `ls` & `man` as well as the pager `less`.
Inspect the `man` pages for the commands in the following table  & fill in the appropriate fields.
Have a look at the useful options & try to understand what they will do if specified when invoking the command.

Write your answers on a piece of paper, or in a plain text file.

| **Command** | **Description of function**   | **Useful options** |
|:----------- |:----------------------------- |:------------------ |
| `man`       | Display on-line manual        | -k                 |
| `pwd`       | Print working directory, i.e show where you are | none commonly used |
| `ls`        | List contents of a directory  | -a, -h, -l         |
| `cd`        | Change directory              | (scroll down in `man builtins` to find `cd`) |
| `mv`        |                               | -b, -f, -u         |
| `cp`        |                               | -b, -f, -u         |
| `rm`        |                               | -r (careful...)    |
| `rmdir`     |                               |                    |
| `mkdir`     |                               | -p                 |
| `cat`       |                               |                    |
| `less`      |                               |                    |
| `wc`        |                               | -l                 |
| `head`      |                               | -n# (e.g., -n100)  |
| `tail`      |                               | -n# (e.g., -n100)  |
| `echo`      |                               |  -e                |
| `cut`       |                               | -d, -f, -s         |
| `sort`      |                               |                    |
| `uniq`      |                               | -c                 |
| `wget`      |                               |                    |


*Hint:
Sometimes the side effects of a command can also be useful.
For example, we can also use `touch` to create an empty file using the command string `touch filename`.*


# Putting It All Together

Now we can use some the above commands to perform something useful.

## Creating and Viewing a File

First, let’s make sure we're in `~/Bash_Workshop`

```
cd ~/Bash_Workshop
```

Remember you can use *tab-autocomplete* once you've started typing that directory name


Now we'll create an empty text file.
(Check the man page for touch if you’re not sure about this line.)

```
touch hello.txt
```

We can read it, but it won’t have anything in it yet.
```
cat hello.txt
```

Obviously nothing was printed to your terminal in the previous line because the file is empty.
Let’s write something to the file, then try reading it again.
In the following line, the symbol `>>` places the text at the end of whatever is already in the file.
As the file is currently empty, this will just write a single line.
```
echo "Hello" >> hello.txt
cat hello.txt
```

We can find a whole lot of information about the file.
```
wc hello.txt
```

### Question
{.:no_toc}

*In the previous line, what do the three numbers represent?*


When we added the word `Hello` to our file, we used the symbol `>>` which actually wrote the word to the end of the file.
As the file was completely empty, this placed the word in the first line.
This is a *VERY* handy short-cut for writing information to the end of a file.

Now let’s add more to the file.
```
echo "It's me" >> hello.txt
cat hello.txt
wc hello.txt
```

*For the last two of the above commands we could have used the auto-complete feature of the
bash terminal.
Did you remember this trick?*

## Copying and Renaming a File

Later today, we’re going to look through a file containing a list of words.
Let's download this from the internet and place it in our `Bash_Workshop` folder.
(Feel free to copy & paste the url to the file. It's pretty long.)

```
cd ~/Bash_Workshop
wget https://uofabioinformaticshub.github.io/Intro-Bash-Sept-2017/files/american-english
```

The name for this file isn't as convenient as we'd like, so we can use the command `mv` to rename it.
Note that under `bash` to rename a file, we *move* it to the same folder, but with a different filename.
This is a slightly unconventional way to think about renaming, but once you get used to it it does make sense.

```
mv american-english words
```

We can look at the first 5 lines of the file using

```
head -n5 words
```

Or we can look at the last 10 lines of the file using

```
tail -n10 words
```

We could even page through the file using `less`. (Remember to hit q to exit the pager.)

```
less words
```

We can even find how many lines there are in the file by using

```
wc -l words
```

We'll come back to this file in the next section when we explore regular expressions.

### Task
{.:no_toc}

Let's try a new task.
However, this time you'll have to think of the commands yourself.

1. Use the `cd` command to make sure you are in the folder `Bash_Workshop`
2. Use the command `wget` to download the `gff` file `ftp://ftp.ensembl.org/pub/release-89/gff3/drosophila_melanogaster/Drosophila_melanogaster.BDGP6.89.gff3.gz`
3. Now unzip this file using the command `gunzip`.
(Hint: After typing `gunzip`, use tab auto-complete to add the file name.)
4. Change the name of the file to `dm6.gff` using the command `mv`
5. Look at the first 10 lines using the `head` command
6. Change this to the first 5 lines using `head -n5`
7. Look at the end of the file using the command `tail`
8. Page through the file using the pager `less`
9. Count how many lines are in the file using the command `wc -l`
