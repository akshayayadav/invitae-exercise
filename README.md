### Invitae Bioinformatics Exercise B.2

This repository contains the Python (Python 3.7.3) program solution for the Bioinformatics 
Exercise B.2 exercise. The program takes two commandline arguments that are the two input 
files given in the problem. The expected output is written in `Output.txt` file which is 
created in the working directory.
 
The program can be executed using the following command using Python3 interpreter in the 
system terminal:

```
python exercise_b2.py Input_file_1.txt Input_file_2.txt
```

#### Assumptions
* Both the chromosome (reference) and the transcript coordinates are 0-based.

* The transcript is always mapped from genomic 5’ to 3’.

* Since the Input file 2 only provides transcript ids, it is assumed that one transcript aligns 
to a unique location on at most one chromosome. In other words, a transcript cannot align to 2 different 
chromosomes or to 2 different locations on a single chromosome.

#### Testing and error handling
* Each line in input file 1 is inspected to check if it contains at least 4 columns and if the 
third column contains an integer (alignment start coordinate). The CIGAR string is also inspected to 
check if it contains correct characters defined on page 8 of the 
[SAM/BAM format specification](https://samtools.github.io/hts-specs/SAMv1.pdf) document, and if it has a 
correct CIGAR format. If any of these conditions are not satisfied, the program throws an 
appropriate error and exits.

* Each line in input file 2 is inspected to check if it contains at least 2 columns and if the 
second column contains an integer. If any of these conditions are not satisfied, the program 
throws an appropriate error and exits.

* If a transcript is encountered in input file 2 that was not present in input file 1, the 
program throws an appropriate warning. Also, for a defined transcript in input file 2, if the
given transcript coordinate does not have a defined chromosome coordinate (due to insertion or if the
coordinate is out of the alignment range) an appropriate warning is thrown.

#### Strengths and Weaknesses
* Strength: The program is designed to check for formatting errors in the input files like missing columns 
and blank lines

* Strength: The program also inspects the CIGAR strings for formatting errors and presence of unknown 
characters

* Strength: Calculates all the coordinate mappings between the transcripts and chromosomes along the entire
length of the alignment, and stores it in the memory. This enables quick look up and mapping of any transcript
coordinates on the genomic coordinates without the need for repeatedly processing CIGAR strings

* Weakness: Calculating and storing the coordinate mappings for entire alignments could create memory 
problems while processing large input files.

#### Bells and Whistles
* The program can be easily modified to accommodate transcripts that map to genomic 3' to 5'.
The `process_cigar_arr` function that calculates coordinate mappings between the
chromosome and the transcript can be modified to run backwards on the chromosome to accommodate
the genomic map from 3' to 5', if the reverse flag is detected in input file 1.

* To map genomic coordinates onto transcript coordinates, the dictionary that is returned by 
the function `process_cigar_arr` (which has key as the transcript coordinate and the value as
the genomic coordinate) can be easily modified so that the key is the genomic coordinate and 
the value is the corresponding transcript coordinate.

* The transcript range can be mapped on to the genomic range by sorting the coordinate 
correspondence dictionary (returned from `process_cigar_arr` function) by keys and obtaining 
the genomic coordinates that correspond to the smallest and the largest transcript coordinates.
A transcript CIGAR can be converted into a genomic CIGAR by interchanging the INSERTION, DELETION and 
other alignment operations that either consume query or reference, but not both. For example, the 
genomic CIGAR for the transcript CIGAR 8M7D6M2I2M11D7M is 8M7I6M2D2M11I7M.

* Transcript data from external sources can be obtained from RNAseq experiments that are designed to
address specific biological questions. The FASTQ files containing reads from RNAseq experiments 
can be downloaded from databases like NCBI-SRA or EBI-arrayexpress. These reads can be assembled
into transcripts using tools like Oases and Trinity, which then can be aligned to appropriate
reference genomes to obtain the SAM files containing transcript alignment coordinates and CIGAR strings.
In order to handle long CIGAR strings containing millions of alignment operations, for a very 
large number of alignments, external databases like MySQL can be  used to store and retrieve 
coordinate mappings between transcripts and chromosomes for each transcript-chromosome alignment.
 
