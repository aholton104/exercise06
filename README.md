# GA2 answers - Abigail Holton

## Part A: Starting a Git repository

### 1: Copy the template file and add your name

```bash
cp ../../../share/GA2/GA2_template.md README.md
```

### 2: Initialize a Git repository

### 3: Stage and commit README

## Part B: Exploring the GTF file with Unix data tools

### 4: Create `data`/`results` dirs and copy the GTF file

```bash
mkdir data results
```

```bash
cp ../garrigos/ref/annot.gtf.gz data/
```  

```bash
ls
```

The output of the command was:

```
data READ.md results
```

### 5: File size of `annot.gtf.gz`

```bash
ls data -lh
```

The output of the command was:

```
-rw-rw----+ 1 aeholton2005 PAS3493 5.1M Sep 25 12:33 annot.gtf.gz
```

### 6: Decompress the GTF file and check its size

```bash
gunzip data/annot.gtf.gz
```

<!-- Put the command you used to check the size of annot.gtf below. -->

```bash
ls data -lh
```

The output of the command was:

```
-rw-rw----+ 1 aeholton2005 PAS3493 123M Sep 25 12:33 annot.gtf
```

*Answer*: The uncompressed file is approximately **24** times larger.

### 7: Total number of lines in `annot.gtf`

```bash
wc -l data/annot.gtf
```

The output of the command was:

```
408402 data/annot.gtf
```

*Answer*: Total number of lines: **408402**

### 8: Number of lines in the table section (header lines counted by eye)

```bash
head -n5 data/annot.gtf
```

The output of the command was:

```
#gtf-version 2.2
#!genome-build TS_CPP_V2
#!genome-build-accession NCBI_Assembly:GCF_016801865.2
#!annotation-source NCBI RefSeq GCF_016801865.2-RS_2022_12
NC_068937.1     Gnomon  gene    2046    110808  .       +       .       gene_id "LOC120427725"; transcript_id ""; db_xref "GeneID:120427725"; description "homeotic protein deformed"; gbkey "Gene"; gene "LOC120427725"; gene_biotype "protein_coding"; 
```

*Answer*: Number of header lines (counted by eye): **4**

*Answer*: Number of lines in the table section: **408398**

### 9: Number of lines in the table section (`grep -v`)

```bash
grep -vc "#" data/annot.gtf
```

The output of the command was:

```
408397
```

*Answer*: Number of lines in the table section: **408397**

### 10: Count distinct sequences, save to `results/scaffolds.txt`

```bash
grep -v "#" data/annot.gtf | cut -f 1 | sort | uniq > results/scaffolds.txt
```

```bash
wc -l results/scaffolds.txt

The output of the command was:

```
167 results/scaffolds.txt
```

*Answer*: Number of distinct sequences: **167**

### 11: Frequency table of feature types

```bash
grep -v "#" data/annot.gtf | cut -f 3 | sort | uniq -c | sort -n
```

The output of the command was:

```
19673 gene
25864 start_codon
25892 stop_codon
29347 transcript
141444 CDS
166177 exon
```

### 12: Predict, then run `grep -c "gene"`

*Answer* (prediction, before running): **b**

```bash
grep -c "gene" data/annot.gtf
```

The output of the command was:

```
408397
```

*Answer*: Was your prediction correct, and what does the comparison with
the number of genes in your frequency table from question 11 tell you?

Yes, my prediction was correct. Comparison with the frequency table says that this command overcounts the number of genes since the word "gene" appears in every line of the table in the `data.annot.gtf` file. `grep` matches the pattern in the "" marks, so it counts any line with the word "gene" in it. 

## Part C: Updating your Git repository

### 13: Check Git status

```bash
git status
```

The output of the command was:

```
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        data/
        results/

no changes added to commit (use "git add" and/or "git commit -a")
```

### 14: Create `.gitignore` and check status again

```bash
touch .gitignore
```

```bash
git status 
```

The output of the command was:

```
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   .gitignore
        modified:   README.md
```

*Answer*: Is the `.gitignore` working as intended?

Yes, the `.gitignore` works as intended. The `results/` and `data/` dirs no longer appear when the `git status` command is executed.


### 15: Stage and commit `.gitignore`, then README again

## Part D: Exploring the FASTQ files with Unix data tools

### 16: Copy the FASTQ file

```bash
cp ../garrigos/fastq/S63_R1.fastq.gz data/
```

```bash
ls data/
```

The output of the command was:

```
annot.gtf S63_R1.fastq.gz
```

### 17: Check Git status again

```bash
git status
```

The output of the command was:

```
On branch main
nothing to commit, working tree clean
```

*Answer*: Does the FASTQ file show up? Why/why not?

No,  `S63_R1.fastq.gz` does not show up after executing the `git status` command because the dir it is in, `data/` is listed in the `.gitignore` file and changes to that dir are ignored. 


### 18: Number of reads in the FASTQ file

```bash
zcat data/S63_R1.fastq.gz | wc -l
```

The output of the command was:

```
2000000
```

*Answer*: Number of reads: **500000** (since 4 lines in a FASTQ file make up a single read)

### 19: Reads with at least 10 consecutive `N`s

```bash
zcat data/S63_R1.fastq.gz | grep -c "NNNNNNNNNN"
```

The output of the command was:

```
32612
```

*Answer*: Number of reads with at least 10 consecutive `N`s: **8153** (since 4 lines in a FASTQ file make up a single read **??**)

### 20: Stage and commit README again

## Bonus

### 21: Why do the counts from questions 8 and 9 differ?

*Answer*: As I understand it, the final line of the GTF file is a series of `#` that is excluded from the `grep -vc` count command, but since it does not appear at the beginning of the file, it was not counted as part of the header. The `grep` command used in question 9 excludes all "#," so it was not included in the question 9 count.

### 22: Concepts/commands you don't (fully) understand

*Answer*: While I understand the basic git workflow thus far, I am struggling with stringing commands together. It usually takes me several attemps and a lot of trouble-shooting to figure out what commands I can combine and what they do, like with the `grep`, `zcat` and `cat`, or `ls` commands. I am also struggling to understand the concept and application of the pipe. I am still not certain of what it really does and how it is used. I also don't know how the order in which you input commands changes the output. Another think I am unsure about is knowing how to intepret FASTQ files and convert "lines" to "reads." I know that 4 lines makes up a single read, but I do not know if the "divide by 4" rule applies to the outputs of all or just some commands for FASTQ files. 


10/14/2026 - Created GitHub repo (git@github.com:aholton104/exercise06.git)