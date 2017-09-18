* TOC
{:toc}

# Writing Scripts

## Shell Scripts

Sometimes we need to perform repetitive tasks on multiple files, or need to perform complex series of tasks and writing the set of instructions as a script is a very powerful way of performing these tasks.
They are also an excellent way of ensuring the commands you have used in your research *are retained for future reference*.
Keeping copies of all electronic processes to ensure reproducibility is a very important component of any research.

Now that we've been through just some of the concepts & tools we can use when writing scripts, it's time to tackle one of our own where we can bring it all together.

### The `shebang`

Every bash shell script begins with what is known as a *shebang*, which we would commonly recognise as a hash sign followed by an exclamation mark, i.e `#!`.
This is immediately followed by `/bin/bash`, which tells the interpreter to run the command `bash ` in the directory `/bin`.
This opening sequence is essential for all `bash` scripts & tells the computer how to respond to all of the following commands.
As a string this looks like:

```
#!/bin/bash
```

The hash symbol generally functions as a comment character in scripts.
Sometimes we can include lines in a script to remind ourselves what we're trying to do, and we can preface these with the hash to ensure the interpreter doesn't try to run them.
It's presence as a comment in the very first position of a script followed by the exclamation mark, is specifically looked for by the interpreter but beyond this specific occurrence, comment lines are generally ignored by scripts & programs.

## An Example Script

First let's look at some simple scripts.
These are really just examples of some useful things you can do & may not really be the best scripts from a technical perspective.
Hopefully they give you some pointers so you can get going

**Don't try to enter the following commands directly in the terminal!!!**
They are designed to be placed in a script which *we will do after we've inspected the contents of the script* (see next section).
Let's start by just looking through the script to make sure we understand what the script is doing.

```
#!/bin/bash

# First we'll declare some variables with some text strings
ME='Put your name here'
MESSAGE='This is your first script'

# Now well place these variables into a command to get some output
echo -e "Hello ${ME}\n${MESSAGE}\nWell Done!"
```

- You may notice some lines that begin with the # character.
These are *comments* which have no impact on the execution of the script, but are written so you can understand what you were thinking when you wrote it.
If you look at your code 6 months from now, there is a very strong chance that you won't recall exactly what you were thinking, so these comments can be a good place just to explain something to the future version of yourself.
There is a school of thought which says that you write code primarily for humans to read, not for the computer to understand.
- Another coding style which can be helpful is the enclosing of each *variable name* in curly braces every time the value is called, e.g. `${ME}`
Whilst not being strictly required, this can make it easy for you to follow in the future when you're looking back.
- Variables have also been named using strictly upper-case letters.
This is another optional coding style, but can also make things clear for you as you look back through your work.
Most command line tools use strictly lower-case names, so this is another reason the upper-case variable names can be helpful.

#### Question
{:.no_toc}
In the above script, there are two variables.
Although we have initially set them each to be one value, they are still variables.
*What are their names?*

### Writing and Executing Our First Script
{:.no_toc}

Let's create an empty file which will become our script.
We'll give it the suffix `.sh` as that is the common convention for bash scripts.
Make sure you're in the `Bash_Workshop` folder, then enter:

```
touch wellDone.sh
```

Now open this using the using the text editor *nano*:

```
nano wellDone.sh
```

Enter the above code into this file **setting your actual name as the ME variable**,  and save it by using `Ctrl+o` (indicated as `^O`) in the nano screen.
Once you're finished, you can exit the `nano` editor by hitting `Ctrl+x` (written as `^X`).
Assuming that you've entered everything correctly, we can now execute this script by simply entering

```
bash wellDone.sh
```

### Setting File Permissions

Unfortunately, this script cannot be executed without calling `bash` explicitly but we can also enable execution of the file directly by setting the execute flag in the file permissions.
First let's look at what permissions we have:

```
ls -lh *.sh
```

You should see output similar to this:
```
-rw-rw-r-- 1 your_login_name your_login_name  247 Aug  14 14:48 wellDone.sh
```

- Note how the first entry is a dash (`-`) indicating this is a file.
- Next come the three Read/Write/Execute triplets which are `rw-` followed by `rw-` and `r--`

#### Question
{:.no_toc}

*Interpret the final triplet? What are these permissions indicating, and for whom?*

As you can see, the `x` flag has not been set in any of the triplets, so this file is not executable as a script yet.
To do this, we simply need to set the `x` flag, then we'll look again using long-listing format.

```
chmod +x wellDone.sh
ls -lh *.sh
```

Note how the file now has the `x` flag set for every user, which means every user can execute this script.
Now we can execute the script by calling it using the file path.
One of the settings in `bash` though won't allow you to execute the file from the same folder, so we need to add the `./` prefix to the script.

```
./wellDone.sh
```

We can set each of these flags for all triplets using `+` to turn the flag on, or `-` to turn the flag off.
If we wanted to remove `write` permissions for all users we could simply use the command:

```
chmod -w wellDone.sh
ls -lh *.sh
```

This can be a very useful trick for *write-protecting* files!

These flags actually represent *binary bits* that are either on or off.
Reading from right to left:
1. the first bit is the execute flag, which has value 1
2. the second bit is the write flag, which has the value 2
3. the third bit is the read flag, which has the value 4

Thus each combination of flags can be represented by a single integer, as shown in the following table:

| Value | Binary | Flags | Meaning |
|:----- | ------ | ----- |:------- |
| 0     | `000`  | `---` | No read, no write, no execute |
| 1	    | `001`  | `--x` | No read, no write, execute	|
| 2	    | `010`  | `-w-` | No read, write, no execute	|
| 3	    | `011`  | `-wx` | No read, write, execute	  |
| 4	    | `100`  | `r--` | Read, no write, no execute	|
| 5	    | `101`  | `r-x` | Read, no write, execute	  |
| 6	    | `110`  | `rw-` | Read, write, no execute	  |
| 7	    | `111`  | `rwx` | Read, write, execute	      |

We can now set permissions using a 3-digit code, where 1) the first digit represents the file owner, 2) the second digit represents the group permissions and 3) the third digit represents all remaining users.

To set the permissions for our script to `read-write-execute`for you and any other users in the group you belong to, we could now use
```
chmod 774 wellDone.sh
ls -lh *sh
```

#### Question
{:.no_toc}
*What will the final 4 in the above settings do?*

### Modifying our script

In the initial script we used two variables `${ME}` and `${MESSAGE}`.
Now let's change the variable `${ME}` in the script to read as `ME=$1`.
First we'll create a copy of the script to edit, and then we'll edit using `nano`

```
cp wellDone.sh wellDone2.sh
nano wellDone2.sh
```

We may need to set the execute permissions again.

```
chmod +x wellDone2.sh
ls -lh *sh
```

This time we have set the script to *receive input from stdin* (i.e. the terminal), and we will need to supply a value, which will then be placed in the variable `${ME}`.
Choose whichever random name you want (or just use "Boris" as in the example) and enter the following
```
./wellDone2.sh Boris
```

As you can imagine, this style of scripting can be useful for iterating over multiple objects.
A trivial example, which builds on a now familiar concept would be to try the following.
```
for n in Boris Fred; do (./wellDone2.sh ${n}); done
```

As a good example, this script could summarise key features in a file.
Then we could simply pass the script multiple files using this strategy, and write the output to another file using the `>` symbol.

## Using `for` Loops

Here's an example of a script which uses a `for` loop.

```
#!/bin/bash

FILES=$(ls)

COUNT=0
for f in ${FILES};
  do
    ((COUNT++))
    ln=$(wc -l ${f} | sed -r 's/([0-9]*).+/\1/g')
    echo "File number ${COUNT} (${f}) has ${ln} lines"
  done
```

#### Task
{:.no_toc}
Save this as a script in the `Bash_Workshop` folder called `lineCount.sh`.
**Add comments** where you think you need them to make sure you understand what's happening.


## A More Advanced Script

In this section we'll write a script for the dm6:ncrna fasta file.
Briefly inspect the file before checking the script.
We're going to extract some key information from those sequence header lines.

```
head Drosophila_melanogaster.BDGP6.ncrna.fa
```

Now let's look through the following script before we run it.
There is a lot of extra information here!

```
#!/bin/bash

INFILE=$1

# Check the file has the suffix .fa or .fasta
SUFFIX=$(echo ${INFILE} | sed -r 's/.+(fasta|fa)$/\1/')
if [ ${SUFFIX} == "fa" ] || [ ${SUFFIX} == "fasta" ]; then
  echo File has the suffix ${SUFFIX}
else
  echo File does not have the suffix 'fa' or 'fasta'. Exiting with error.
  exit 1
fi

# Define the output file by changing the suffix to .locations
OUTFILE=${INFILE%.${SUFFIX}}.locations
echo Output will be written to $OUTFILE

# Get the header lines which correspond to chromosomes, then collect the
# gene id, chromosome, start and end and write to the output file
egrep '^>.+chromosome' ${INFILE} | \
  sed -r 's/.+BDGP6:([^:]*):([0-9]+):([0-9]+).+gene:([^ ]+).+/\4\t\1\t\2\t\3/g' \
  > ${OUTFILE}

echo Done
```

- After the `shebang`, the first line takes a filename as input.
- The next set of lines contains two processes:
    + The suffix `fasta` or `fa` is pulled from the filename and passed to the variable `${SUFFIX}`. *What will this return if the suffix is not either of these?*
    + Next, the value of the new variable `${SUFFIX}` is checked to make sure it contains only `fa` or `fasta`
		    + If this condition is met a message will print to `stdout`
		    + If one of these conditions is not satisfied, the script will exit giving an error message (`exit 1`)
    + The output file (`OUTFILE`) is defined by changing the suffix from whichever is provided to `.locations`. The use of the `%` to *snip* the filename and replace with another is a very useful trick
		+ Finally the header lines containing the word "chromosome" are piped into `sed`
		    + `sed` then captures the **chromosome** (`[^:]*`), **start** (`[0-9]+`), **end** (`[0-9]+`) and **gene id** (`[^ ]+`)
				+ These are returned in the order **gene id**, **chromosome**, **start**, **end**
				+ All information is written to the file specified in `${OUTFILE}``


Save this as the file `getLocations.sh` and make it executable using `chmod +x`
Now run it passing the `.fa` file as the first argument.

```
./getLocations.sh Drosophila_melanogaster.BDGP6.ncrna.fa
```

## Writing a Script To Include Options

In our final script, we'll change this to allow specifying the file using an argument (or option).
We'll also demonstrate a file checking step.

```
#!/bin/bash

INFILE=""

# This version now includes the option -f for specifying the file
while getopts 'f:' opt ; do
  case $opt in
    f) INFILE=$OPTARG ;;
  esac
done

# Check the input file exists
echo "Checking for a valid input file"
if [[ -a $INFILE ]]; then
  echo "Found ${INFILE}"
else
  echo "Could not find ${INFILE}. Exiting with error"
  exit 1
fi

# Check the file has the suffix .fa or .fasta
SUFFIX=$(echo ${INFILE} | sed -r 's/.+(fasta|fa)$/\1/')
if [ ${SUFFIX} == "fa" ] || [ ${SUFFIX} == "fasta" ]; then
  echo File has the suffix ${SUFFIX}
else
  echo File does not have the suffix 'fa' or 'fasta'. Exiting with error.
  exit 1
fi

# Define the output by changing the suffix to .locations
OUTFILE=${INFILE%.${SUFFIX}}.locations
echo Output will be written to $OUTFILE

# Get the header lines which correspond to chromosomes, then collect the
# gene id, chromosome, start and end and write to the output file
egrep '^>.+chromosome' ${INFILE} | \
  sed -r "s/.+BDGP6:([^:]*):([0-9]+):([0-9]+).+gene:([^ ]+).+/\4\t\1\t\2\t\3/g" \
  > ${OUTFILE}

echo Done
```

- Notice how we've added a section that looks for an option denoted with `-f`. This is where the script will look for the input file.
- Next we've performed a checking step to ensure the file exists. The rest of the script is the same.

We could specify any number of options to our script.
For example, we may wish to change the `BDGP6` to be more flexible for any other genome using a flag and input such as `-g genomeCode`.
