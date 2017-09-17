* TOC
{:toc}

# Some More Useful Commands

Now we've found how to navigate directories in `bash` let's move onto some useful tools, tricks and commands.

## Tab auto-complete

A very helpful & time-saving tool in `bash` is the ability to automatically complete a command, file or directory name using the `<tab>` key.
Move the the directory above the `Bash_Workshop` directory using the `cd` command.
(If you're already in the `Bash_Workshop` directory, you'll simply need `cd ..`).

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
| `echo`      |                               | -e                 |
| `cut`       |                               | -d, -f, -s         |
| `sort`      |                               |                    |
| `uniq`      |                               | -c                 |
| `wget`      |                               |                    |


*Sometimes the side effects of a command can also be useful.
For example, we can also use `touch` to create an empty file using the command string `touch filename`.*

To demonstrate this, let's create an empty text file.

```
touch blank.txt
```

We can read it, but it won’t have anything in it, as it's an empty file.
```
cat blank.txt
```

Let's delete it as it's really a bit of a pointless file.

```
rm blank.txt
```

# Text In the Terminal

## Introducing Standard Output

All the information we've seen in the terminal so far has been from a data stream known as *standard output*, or `stdout` for short.
There are two primary data streams in play when we use commands in `bash`.
The first as we've seen is `stdout`, with the alternative stream being *standard error*, or `stderr` for short.
This is where commands and tools send their error messages.
We'll ignore that for the rest of the day, but it's good to know it exists.

When a command sends information to us via `stdout`, we refer to this as *printing* to `stdout`.
This dates back to the days before everyone had printers, when printing to the screen was the main method of interacting with computers.

We can display a line of plain text in `stdout` by using the command `echo`.
The most simple function that people learn to write in most languages is called `Hello World` and we’ll do the same thing today.

```
echo 'Hello World'
```

That’s pretty amazing isn’t it & you can make the terminal window say anything you want without meaning it.

```
echo 'This computer will self destruct in 10 seconds!'
```

There are a few subtleties about text which are worth noting.
Inspect the man echo page & note the effects of the -e option.
 This allows you to specify tabs, new lines & other special characters by using the backslash to signify these characters.
 This is an important concept & the use of a backslash to escape the normal meaning of a character is very common.
 Try the following three commands & see what effects these special characters have.

```
echo 'Hello\tWorld'
echo -e 'Hello\tWorld'
echo -e 'Hello\nWorld'
```

As we’ve seen above, the command `echo` just repeats any subsequent text.
Now enter

```
echo ~
```
### Question
{:.no_toc}

*Why did this happen?*

Although this may have seemed trivial, we often include lines like the above in scripts to provide messages about the progress of our tasks.
As we'll see later today, `echo` is actually a very heavily used command.


## Redirecting `stdout` To a File

Instead of just sending the output of a command to `stdout`, we can redirect this output into a file using the `>` symbol.
If the file doesn't exist, `bash` will simply create the specified file and write the output into it.
If the file *does* exist, **it will be immediately overwritten without any warnings**.

Let's see the `>` symbol in action.
First, let’s make sure we're in `~/Bash_Workshop`

```
cd ~/Bash_Workshop
```

Now we'll combine the `echo` command with the redirection:

```
echo 'Hello' > hello.txt
```

Notice that the word `Hello` was no longer printed to your screen.
Instead a file called `hello.txt` was created and we can simply view the entire contents of the file using the command `cat`

```
cat hello.txt
```

If we wish to add any additional information to the file, we can use the `>>` symbol which *appends* the new information to the file.
Again, if the file doesn't already exist it will be created, but this time if the file **does** exist, the file will not be overwritten, but rather the new information will be added to the end of the existing information.
Let's see this in action.

```
echo 'It's me.' >> hello.txt
cat hello.txt
```

We can also check how many lines we have in this file using the command `wc`.

```
wc hello.txt
```

### Questions
{:.no_toc}

*In the previous line, what do the three numbers represent?*

*Did you remember to use tab auto-complete in the above commands?*

## Redirecting `stdout` to Another Command

We've just seen how we can take `stdout` and send it to a file, but we can also send it to another command.
This is one of more common things you will do in `bash` and we can perform complex manipulations on the data within a file, without ever opening the file in the manner you would be most familiar with.

To send `stdout` to another command, we simply use the *pipe* symbol (&#124;).
As a simple example, we could take the output from a long `ls` command and pipe it into `less` for easier browsing.
The following command will usually give you more information than `bash` will retain in a `stdout` screen.
```
ls -R /usr/bin
```

If we wanted to keep all this for closer examination, we could send it to `less` instead of having all of the information dumped on the screen.

```
ls -R /usr/bin | less
```

Page through this for a while, then when you're bored quit using the `q` key.

# Working with Files

## Copying a File (`cp`)

Copying a file using `bash` is so easy it hardly requires any effort.
The command simply follows the syntax `cp sourceFile destinationFile`.

By way of simple example

```
cp hello.txt hello.txt-copy
```

We don't need these any more, so let's be brave and delete both.

```
rm hello*
```

## Downloading a File (`wget`)

Later today, we’re going to look through a file containing a list of words.
Let's download this from the internet and place it in our `Bash_Workshop` folder, using the command `wget`, which stands for `web get`.
(Feel free to copy & paste the url to the file. It's pretty long.)

```
cd ~/Bash_Workshop
wget https://uofabioinformaticshub.github.io/Intro-Bash-Sept-2017/files/american-english
```

## Renaming a File (`mv`)

The name for this file isn't as convenient as we'd like, so we can use the command `mv` to rename it as the file `words` instead of `american-english`.
Note that under `bash` to rename a file, we *move* it to the same folder, but with a different file name.
This is a slightly unconventional way to think about renaming, but once you get used to it, it does make sense.

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

We could even page through the file using `less`. (Remember to hit `q` to exit the pager.)

```
less words
```

We can even find how many lines there are in the file by using

```
wc -l words
```

We'll come back to this file in the next section when we explore regular expressions.

## Working with Compressed Files

In bioinformatics, we often have to deal with compressed files as it is far easier to transmit large files across network connections if they are compressed.
The most common types of compression you will see are:

| File Suffix | Compression Command | Extraction Command | Useful Arguments |
|:----------- |:------------------- |:------------------ |:---------------- |
| .zip        | zip                 | unzip              | -d, -c, -f       |
| .gz         | gzip                | gunzip <br> zcat   | -d, -c, -f       |
| .tar.gz     | tar                 | tar                | -x, -v, -f, -z   |
| .bz2        | bzip2               | bunzip2            |                  |


Often, files you download will be compressed (tar: tape archive) and archived (zipped).
If you see file name suffixes like .tar, .zip, .gz, and/or .bz2, among others, that is what these are.
To explore what these command-line options do, please check the `man` or `--help` pages.

A very useful trick is the use of the command `zcat`.
This extracts a `gzip` compressed file to `stdout` leaving the original file unchanged.
From here we can pipe (&#124;) into `less` or `head` to quickly look through a file before extracting.

### Task 1
{:.no_toc}

Let's try a new task.
However, this time you'll have to think of how to execute the commands yourself.

1. Use the `cd` command to make sure you are in the folder `Bash_Workshop`
2. Use the command `wget` to download the compressed `gff` file `ftp://ftp.ensembl.org/pub/release-89/gff3/drosophila_melanogaster/Drosophila_melanogaster.BDGP6.89.gff3.gz`
3. Preview the file before extracting using `zcat` and `head` along with the pipe symbol (&#124;)
4. Now unzip this file using the command `gunzip`.
(*Hint: After typing `gunzip`, use tab auto-complete to add the file name*.)
5. Change the name of the file to `dm6.gff` using the command `mv`
6. Look at the first 10 lines using the `head` command
7. Change this to the first 5 lines using `head -n5`
8. Look at the end of the file using the command `tail`
9. Page through the file using the pager `less`
10. Count how many lines are in the file using the command `wc -l`

# Working With Tabular Files

The file we have just downloaded begins with comment lines (starting with `#`) then follows a column layout, with columns being separated by `<tab>` markers.
As each line can be very long, they may wrap across more than one line.
Make sure you can spot this format though.

## Extracting Columns (`cut`)

We can use the command `cut` to cut one or more columns from this file.
Call up the `man` page.

```
man cut
```

(And if you think that sounds violent, try `man kill` or `man killall`)

We introduced this earlier, and the options we're interested in now are `-f` and `-s`.

### Question
{:.no_toc}
*Why is the `-s` command relevant here?*

The third column contains information about what type of feature is on each line.
We can get just this column using the following.

```
cut -f3 -s dm6.gff
```

This will just dump all the information to `stdout`.
In the next couple of sections we'll sort then count these entries

## Sorting Data (`sort`)

This command is one of the most intuitive to use so requires little explanation.
Let's pipe the out of the previous cut into the `sort` command.

```
cut -f3 -s dm6.gff | sort
```

This has sorted all the type into alphabetic order

## Getting Unique Entries (`uniq`)

Now we have these sorted we can use the command  `uniq` to just return a single entry for each feature type.

```
cut -f3 -s dm6.gff | sort | uniq
```

This has given us a simple list of the different feature types in this file.
We can also count these using the `-c` option.

```
cut -f3 -s dm6.gff | sort | uniq -c
```

### Questions
{:.no_toc}
*What would happen if we didn't include the `sort` step?<br>
What does this tell us about how the `uniq` command works?*
