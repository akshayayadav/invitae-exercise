### Invitae Bioinformatics Exercise B.2

 This repository contains the Python (Python 3.7.3) program solution for the Bioinformatics Exercise B.2 exercise. The 
 program takes two commandline arguments that are the two input files given the problem. The expected output is created
 in a `Output.txt` file that is created in the working directory.
 
 The program can be executed using the following command using Python3 interpreter in the system terminal:
```
python exercise_b2.py Input_file_1.txt Input_file_2.txt
```

#### Assumptions
* Both the chromosome (reference) and the transcript coordinates are 0-based.

* The transcript is always mapped from genomic 5’ to 3’

* Since the Input file 2 only contains transcript ids, one transcript matches to a unique 
location on a single chromosome. A same transcript cannot match to 2 different chromosome 
or to 2 different locations on a same chromosome.

#### Testing and error handling
* Each line in input file 1 is inspected to check if it contains at least 4 columns and if the 
third column contains an integer (alignment start coordinate). The CIGAR string is also inspected to 
check if it contains correct characters defined on page 8 of the 
[SAM/BAM format specification](https://samtools.github.io/hts-specs/SAMv1.pdf) and if it has a 
correct CIGAR format. If any of these conditions are not satisfied the program throws an 
appropriate error and exits.

* Each line in input file 2 is inspected to check if it contains at least 2 columns and if the 
second column contains an integer. If any of these conditions are not satisfied the program 
throws an appropriate error and exits.

* If a transcript is encountered in input file 2 that was not present in input file 1, the 
program throws an appropriate warning. Also, for a defined transcript in input file 2, the
given transcript coordinate does not have a corresponding chr coordinate (due to insertion or the
coordinate is out of the alignment range) an appropriate warning is thrown.

#### Bells and Whistles
   
  
