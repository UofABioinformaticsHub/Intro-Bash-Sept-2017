* TOC
{:toc}

# sed: The Stream Editor

One very useful command in the terminal is `sed`, which is short for *stream editor*.
Instead of the `man` page for `sed` the `info sed` page is larger but a little easier to digest.
This is a very powerful command which can be a little overwhelming at first.
If using this for your own scripts & you can't figure something out, remember *'Google is your friend'* & sites like \url{www.stackoverflow.com} are full of people wrestling with similar problems to you.
These are great places to start looking for help & even advanced programmers use these tools.

For today, there are two key `sed` functionalities that we want to introduce.

1. Using `sed` to alter the contents of a file/input;
2. Using `sed` to print regions of a file


## Altering a file or other input

`sed` uses *regular expressions* that we have come across under the `grep` section, and we can use these to replace strings or characters within a text string.
The command works in the form `sed 'SCRIPT' INPUT`, and the script section is where all the action happens.
Input can be given to `sed` as either a file, or just as a text stream via the *pipe* that we have already introduced.

In the following example the script begins with an `s` to indicate that we are going to make a substitution.
The beginning of the first pattern (i.e. the *regexp* we are searching for) is denoted with the backslash, with the identical delimiter indicating the replacement pattern, and this is in turn completed with the same delimiter.
Try this simple example from the link \url{http://www.grymoire.com/Unix/Sed.html} which is a very detailed & helpful resource about the usage `sed`.
Here we are sending the input to the command via the pipe, so no `INPUT` section is required:

```
echo Sunday | sed 's/day/night/'
```

Here you are passing `sed` the string Sunday, and `sed` takes day and turns it into night.  
`sed` will only replace the first instance of the string on any line, so try:

```
echo Sundayday | sed 's/day/night/'
```

It only replaced the first instance of day and left the second.  You can make it 'global', where it switches every instance by using the `g` option at the end of the pattern like this:

```
echo Sundayday | sed 's/day/night/g'
```

You can **capture** parts of the pattern using parentheses and access that in the second part of the regular expression (what you are switching to) using \1, \2, etc., to denoted the number of the captured string.
If you wanted to match `ATGNNNTGA`, where `N` is any base, and just output these three bases you could try the following

```
echo 'ATGCCAGTA' | sed -r 's/ATG(.{3})GTA/\1/g'
```

Or if we needed to replace those three bases with an expanded repeat of them, you could do the following where we capture the undefined string between `ATG` & `GTA`, and expand it to three times:

```
echo 'ATGCCAGTA' | sed -r 's/ATG(.{3})GTA/ATG\1\1\1GTA/g'
```

The `\1` entry takes the contents of the first parenthesis and uses it in the substitution, even though you don't know what the bases are.
Note that the `-r` option was set for these operations, which turns on extended regular expression capabilities.
This can be a powerful tool & multiple parentheses can also be used:

```
echo 'ATGCCAGTA' | sed -r 's/(ATG)(.{3})(GTA)/\3\2\2\1/g'
```

In this command we captured each codon separately, then switched the order of the first & last triplet and expanded the middle unknown string twice.
*Note how quickly this starts to look confusing though!*
Taking care to be clear when writing these types of procedures can be an important idea when you have to go back & re-read your code a year or two later.
(Yes this will happen a lot!!!)


## Displaying a region from a file

The command `sed` can also be used to replicate the functionality of the `head` & `grep` commands, but with a little more power at your fingertips.
By default `sed` will print the entire input stream it receives, but setting the option `-n` will turn this off.
Try this by adding an `n` immediately after the `-r` in one of the above lines & you will notice you receive no output.
This is useful if we wish to restrict our output to a subset of lines within a file, and by including a `p` at the end of the script section, only the section matching the results of the script will be printed.

Make sure you are in the `Bash_Workshop` directory & we can look through the file `Drosophila_melanogaster.BDGP6.ncrna.fa` again.

```
sed -n '1,10 p' Drosophila_melanogaster.BDGP6.ncrna.fa
```

This will print the first 10 lines, like the `head` command will by default.
However, we could now print any range of lines we choose.
Try this by changing the script to something interesting like `101,112 p`.
We could also restrict the range to specific lines by using the `sed` increment operator `~`.

```
sed -n '1~4p' Drosophila_melanogaster.BDGP6.ncrna.fa
```
This will print every 4th line, beginning at the first, and is very useful for files with multi-line entries.
The `fastq` file format from NGS data, and which we'll look at in subsequent workshops will use this format.

We can also make sed operate like `grep` by making it only print the lines which match a pattern.
```
sed -rn '/TGCAGGCTC.+(GA){2}.+/ p' Drosophila_melanogaster.BDGP6.ncrna.fa
```

Note however, that the line numbers are not present in this output, and the pattern highlighting from `grep` is not present either.

# Some Important Programming Concepts

Before moving on to `awk` in the next section, we need to quickly recap two of the most widely used techniques
in programming:

1. The `for` loop
2. Logical tests using an `if` statement

## For Loops

A `for` loop is what we use to cycle through an input one item at a time.
As a simple example, we could print each number from a set of numbers.

```
for i in 1 2 3; do (echo -e $i); done
```

In the above code, the fragment before the semi-colon asked the program to cycle through
the values 1, 2 & 3, letting the variable `i` take each value in order of appearance.
First `i = 1`, then `i = 2` & finally `i = 3`.
If you’re wondering why we chose `i`, it just seemed like a sensible choice for an integer.
We simply needed to choose a name for a variable which we would pass the values to.

After assigning each value to `i`, was the instruction on what to `do` for each value.
Note that the value of the variable `i` was prefaced by the dollar sign (`$`).
This is how the bash shell knows it is a **variable**, not the letter `i`.
The command `done` then finished the `do` command.
All commands like `do`, `if` or `case` have completing statements, which
respectively are `done`, `fi` & `esac`.

Another important concept which was glossed over in the previous paragraph is that of a **variable**.
These are essentially just ‘placeholders’ which have a value that can change (hence the name).
In the above loop, the same operation was performed on the variable `i`, but the value changed from 1 to 2 to 3.
Variables in shell scripts can hold numbers or text strings (e.g. file names) and don’t have to be formally defined as in some other languages.

An alternate approach could be to make a breathtaking claim about some files.
Here we’ll use the variable called `f`, which seems sensible for a filename.

```
cd ~/Bash_Workshop
for f in $(ls); do (echo -e "I can see the file $f"); done
```

Note, that we’ve also assigned the output from the command `ls` to this variable, by using the `$()` syntax.
The use of double quotes for the echo command also allows us to refer to the values held by `f`.
Single quotes at this point would only return the characters `$f`.

## If Statements

If statements are those which only have a binary ‘yes’ or ‘no’ response, or more correctly a `TRUE` or `FALSE` response.
For example, we could specify things like:

• `if (i> 1)` then `do` something, or
• `if (fileName==bob.txt)` then `do` something else

Notice that in the second if statement, there was a double equals sign (`==`).
This is the programmers way of saying compare the first argument with the second argument.
A single equals sign is generally interpreted by a program as assign the value of the first argument to be the second argument.
This use of ‘double operators’ is very common, notably you will see `&&` to represent the command ‘and’, and `||` to represent ‘or.’

A final useful trick to be aware of is the use of an exclamation mark to reverse a command.
A good example of this is the use of the command `!=` as the representation of *not equal to* in a logical test.

# awk: A command and a language

Moving on to awk, this is a very useful tool which can be used either as a command, as well as functioning as it’s own language.
We’ll just use it as a command today, and it is extremely useful for dealing with tab- or comma-separated files, such as we often see in biological data.

The basic structure of an awk command is:  
`awk '/<pattern>/' file`  

`awk` will then search the file and output any line containing the regular expression pattern (kind of like `grep`).

With awk, you can also use the format:  

`awk '\{<code>\}' file`

where you can put a program, or set of instructions in the curly braces.
In the code, you can specify values from different columns of the file by using the numbers $1, $2, etc., (or
you can use $0 for the whole line).
Values can also be returned in the output by using the command print followed by the field number.

We have that file `dm6.gff` and we’ve already looked at it at little, so let’s pull out some
particular features!
Make your terminal as wide as the screen, then change into the appropriate directory & enter

```
awk '{if ($3=="rRNA") print $0}' dm6.gff
```

Here we’ve specified that the third field must be an rRNA, so this will give us all of the ribosomal RNAs annotated in the file.

We could also make it a little more complex and just look for genes in a given region.
```
awk '{if ( ($3=="gene") && ($4 > 10000) && ($4 < 20000) ) print $0}' dm6.gff
```

In the above code, `$3=="gene"` asks for the entry in the third field to be “gene.”
The next two fragments request for the values in the fourth field (i.e. `$4`) to be between 10000 & 20000.
Thus we have found all the features annotated as genes in a 10kb region of this genome.

Notice that each these three commands were enclosed in a pair of brackets within an outer pair of brackets.
This gave a command of the form: `( (Condition1) && (Condition2) && (Condition3) )`

After this came the fragment `print $0` which asked `awk` to print the entire line if the 3 conditions are true.
**You’ve just written (& hopefully understood) a computer program!**

### Question
{:.no_toc}

*What does this do?*
```
awk '{if (($5 - $4 > 1000) && ($3 == "gene")) print $0}' dm6.gff
```

If you don’t want to output all of the columns, you can specify which ones to output.
While we’re at it, let’s save the output as a file:

```
awk '{if (($5 - $4 > 1000) && ($3 == "gene")) print $1, $2, $4, $5, $9}' dm6.gff > awkout.txt
```
