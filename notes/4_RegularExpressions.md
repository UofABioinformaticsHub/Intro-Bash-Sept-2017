* TOC
{:toc}

# Regular Expressions
{:.no_toc}

In the previous section we learned how to rename, copy & move files, as well as navigate directories.
In this section, we'll learn more about looking inside files and extracting information without needing to open them.
This can particularly useful when dealing with large files.

## Introducing Regular Expressions
Regular expressions are a powerful & flexible way of searching for text strings amongst a large document or file.
Most of us are familiar with searching for a word within a file, but regular expressions allow us to search for these with more flexibility, particularly in the context of genomics.
As we will see, we could search for a sequence that is either `AGT` or `ACT` by using the patterns  `A[GC]T` or  `A(G|C)T`.
These two patterns will search for an  `A`, followed by either a  `G` or  `C`, then followed strictly by a  `T`.
Similarly a match to `ANNT` can be found by using the patterns `A[AGCT][AGCT]T` or  `A[AGCT]{2}T`.
We'll dicuss that syntax below, so don't worry if those patterns didn't make much sense.

Whilst the bash shell has a great capacity for searching a file to matches to regular expressions, this is where languages like *perl* and *python* offer a great degree more power.
The commands `awk` & `sed` which we will look at later also use regular expressions to great effect.

## The command `grep`
The built-in command which searches using regular expressions in the terminal is `grep`.
This stands for *global regular expression print*, where *print* refers to printing the results to your terminal, which is what we commonly refer to as standard output, or `stdout`.
This function searches a file or input on a line-by-line basis, so patterns contained with a line can be found, but patterns split across lines are more difficult to find.
This can be overcome by using regular expressions in a programming language like Python or Perl.

The `man grep` page contains more detail on regular expressions under the `REGULAR EXPRESSIONS` header (scroll down a few pages).
As can be seen in the `man` page, the command follows the form

`grep [OPTIONS] 'pattern' filename`  

The option `-E` is commonly used as it it stand for *Extended*, which we can also think of as *Easier*.
As well as the series of conventional numbers and characters that we are familiar with, we can match to characters with special meaning, as we saw above where enclosing the two letters in brackets gave the option of matching either.

| Special Character | Meaning |
|:----------------- |:------- |
| \w                | match any letter or digit, i.e. a word character |
| \s                | match any white space character, includes spaces, tabs & end-of-line marks |
| \d                | match any digit from 0 to 9 |
| .                 | matches any single character |
| +                 | matches one or more of the preceding character (or pattern) |
| *                 | matches zero or more of the preceding character (or pattern) |
| ?                 | matches zero or one of the preceding character (or pattern)  |
| {x} or {x,y}      | matches x or between x and y instances of the preceding character
| ^                 | matches the beginning of a line (when not inside square brackets) |
| $                 | matches the end of a line |
| ()                | contents of the parentheses treated as a single pattern |
| []                | matches only the characters inside the brackets |
| [^]               | matches anything other than the characters in the brackets |
| &#124;            | either the string before or the string after the "pipe" (use parentheses) |
| \\                | don't treat the following character in the way you normally would.<br> This is why the first three entries in this table started with a backslash, as this gives them their "special" properties.<br> In contrast, placing a backslash before a `.` symbol will enable it to function as an actual dot/full-stop. |


## Pattern Searching

In this section we'll learn the basics of using the `grep` command & what forms the output can take.
Firstly, we'll need to get the file that we'll search in this section.
First we'll change into your `Bash_Workshop` directory, then we'll quickly check what the file `words`contains.

```
cd ~/Bash_Workshop
less words
```

As you can see, this is literally just a file with words in it.
It's actually used by many text editors for spell checking but we'll just use it to explore pattern matching.

Let's try a few searches to get a feel for the basic syntax of the command.
*Try to describe what you're searching for on your notes* **BEFORE** *you enter the command.
Do the results correspond with what you expected to see?*

```
grep -E 'fr..ol' words
```
```
grep -E 'fr.[jsm]ol' words
```
```
grep -E 'fr.[^jsm]ol' words
```
```
grep -E 'fr..ol$' words
```
```
grep -E 'fr.+ol$' words
```
```
grep -E 'cat|dog' words
```
```
grep -E '^w.+(cat|dog)' words
```

In the above, we were changing the pattern to extract different results from the files.
Now we'll try a few different options to change the output, whilst leaving the pattern unchanged.
If you're unsure about some of the options, don't forget to consult the `man` page.

```
grep -E 'louse' words
```
```
grep -Ew 'louse' words
```
```
grep -Ewn 'louse' words
```
```
grep -EwC2 'louse' words
```
```
grep -c 'louse' words
```


In most of the above commands we used the option `-E` to specify the extended version of `grep`.
An alternative to this is to simply use the command `egrep`, which is the same as `grep -E`.
Repeat a few of the above commands using `egrep` instead of `grep -E`.
This is actually the default command that many bioinformaticians use.

## Regular Expressions In a Biological Context

The `gff` file we have downloaded and renamed earlier (`dm6.gff`) contains all the genomic features from the latest build of the *D. melanogaster* genome.
Each feature is on a separate line, but if you remember from your preview there are several lines which start with the comment character `#`.
These are lines which contain important information about the file, but don't contain any information about genomic features themselves.
If we just wanted to look at these lines, the obvious thing to do would be to use a `regexp` search for lines beginning with `#`, via the `^#` syntax.

```
egrep -n '^#' dm6.gff
```

Note that this will print the line numbers.
*Were all these lines at the beginnning of the file?*

### Regular Expressions Task 1
{:.no_toc}

Knowing that each line (except for the comments) is a feature how could we count how many features are in this file?
There are 3 methods for doing this.
An obvious solution would be to find all lines which do not begin with `#` then pipe the results into `wc -l`.
The other two methods would perform this in `egrep`.

*Can you think of all 3?*

(*Hint: What to the `egrep` options `-c` and `-v` do?*)

### Regular Expressions Task 2
{:.no_toc}

Let's obtain another *D.melanogaster* file.
This time we'll get all *ncRNA* sequences from the Ensembl database.

```
wget ftp://ftp.ensembl.org/pub/release-90/fasta/drosophila_melanogaster/ncrna/Drosophila_melanogaster.BDGP6.ncrna.fa.gz
```

As we know, all fasta sequences start with a fasta header so our task is to place these in a separate file called `seqIDs.txt`.
However, when we create the file include a comment line at the beginnning with today's data and the Ensembl release number.
As you can imagine, this would be a sensible approach to take when creating files like this.

(*Hint: You may need to use the `>>` command before using `egrep`*)


[Home](../)
