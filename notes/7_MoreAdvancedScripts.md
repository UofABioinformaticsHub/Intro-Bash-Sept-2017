* TOC
{:toc}

# More Advanced Scripts

## Loops and paired-end reads

Above you learnt how to use a `for` loop in a `bash` script, and as you can see its pretty easy to run through a process one by one using a single variable.
In the task above we used:

```
for fasta in *.fa
 do
   grep -c "^>" ${fasta} >> ${fasta}.count
done
```

This loop takes your fasta sequence and counts the number of sequences in it.
Because every sequence starts with a header line (which also starts with “>”), you can just count the lines that start with the character `>`. Easy

However, what happens when you need to iterate over two paired-fasta files?
And not only that, you might need to do something specific to each file.
This often happens when you have paired-end sequencing data, where you have sequence from the forward strand of the DNA insert, and a sequence from the reverse strand).
Below we are going to run through an example of using paired-end fasta files.

## Paired-end BLAST script

A common task in any Bioinformatics pipeline setup, whether you’re analysing Sanger or Next-generation sequencing data, is answering the following question:

*“I have a sequence, so how the hell do I find out what the hell this is??”*

The answer is the Basic Local Alignment Search Tool (BLAST). BLAST is one of the most widely used tools and processes in computational biology, and its two publications that describe the process are [actually both in the top 20 all time for scientific citations!!](http://www.nature.com/news/the-top-100-papers-1.16224).
This process uses local alignment to match a sequence against an existing sequence database.
Using the large global sequence databases, from places such as NCBI’s nucleotide or protein archive (or Genbank), we can run BLAST to match any sequence.

The BLAST+ toolkit, [which is available from NCBI](https://blast.ncbi.nlm.nih.gov/Blast.cgi?CMD=Web&PAGE_TYPE=BlastDocs&DOC_TYPE=Download), has the ability to run remote BLAST searches against those large databases, but for this example we are going to BLAST some paired-end sequences against its target genome.

If you'd like to try running the script, download the BLAST+ toolkit for your system then call an instructor over to help you install it.
*This may be quite challenging for Windows Users, but we're game if you're game.*

Below is our script which:

1. Downloads the genome (Arabidopsis thaliana)
2. Creates the database
3. Iterates through R1 and R2 fasta files and runs BLASTn against each file

Note how we can specify each of the paired-end reads by using variable replacement in the for loop, where `${fasta}` indicates the normal target, and `${fasta/R1/R2}` is the second target where we’ve replaced R1 with R2

```
#!/bin/bash

DIRECTORY=`pwd`

# Download the genome
wget -c ftp://ftp.ensemblgenomes.org/pub/plants/current/fasta/arabidopsis_thaliana/dna/Arabidopsis_thaliana.TAIR10.dna.toplevel.fa.gz

cd ${DIRECTORY}

# Create the blastdb
gunzip Arabidopsis_thaliana.TAIR10.dna.toplevel.fa.gz
makeblastdb -in Arabidopsis_thaliana.TAIR10.dna.toplevel.fa \
   -dbtype nucl \
   -parse_seqids \
   -out Athaliana_BLAST_db

for fasta in *_R1*.fa
 do
   # Lets run the R1 fasta files
   blastn -db Athaliana_BLAST_db -query ${fasta} \
      -outfmt 6 -max_target_seqs 1 -max_hsps 1 \
      -out $(basename ${fasta} .fa).blastn.txt

   # Lets run the R2 fasta files
   blastn -db Athaliana_BLAST_db -query ${fasta/R1/R2} \
      -outfmt 6 -max_target_seqs 1 -max_hsps 1 \
      -out $(basename ${fasta} .fa).blastn.txt
done
```

## Another Advanced Script

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

[Home](../)
