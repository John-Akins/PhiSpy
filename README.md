# INTRODUCTION

PhiSpy is a computer program written in C++, Python and R to identify prophages in a complete bacterial genome sequences.

Initial versions of PhiSpy were written by 

Sajia Akhter (sajia@stanford.edu)
PhD Student
Edwards Bioinformatics Lab (http://edwards.sdsu.edu/labsite/)
Computational Science Research Center (http://www.csrc.sdsu.edu/csrc/)
San Diego State University (http://www.sdsu.edu/)


Improvements, bug fixes, and other changes were made by

Katelyn McNair
Edwards Bioinformatics Lab (http://edwards.sdsu.edu/labsite/)
San Diego State University (http://www.sdsu.edu/)



# SYSTEM REQUIREMENTS

The program should run on all Unix platforms, although it was not tested in all platforms.


# SOFTWARE REQUIREMENTS

PhiSpy requires following programs to be installed in the system. NOTE: You can ignore this if you're using the singularity container method of installation.

1. Python - version 2.7.2 or later
2. Biopython - version 1.58 or later 
3. gcc - GNU project C and C++ compiler - version 4.4.1 or later
4. The R Project for Statistical Computing - version 2.9.2 or later
5. Package randomForest in R - version 4.5-36 or later
 

# INSTALLATION

1. Clone this repository
2. `% cd PhiSpy`
3. `% make`
4. For ease of use, add the location of PhiSpy.py to your $PATH.

# ALTERNATE INSTALLATION

1. Get [singularity](http://singularity.lbl.gov/all-releases)
2. Build phispy.img using this [repository](https://github.com/hurwitzlab/singularity-phispy)
3. Run the singularity image `% singularity exec phispy.img PhiSpy.py`
4. NOTE: if you haven't used singularity before you'll need to know about [binding directories](http://singularity.lbl.gov/quickstart#working-with-files) so that PhiSpy can find your input and output.

TO TEST THE PROGRAM

1. `% cd PhiSpy`
2. `% ./PhiSpy.py -i Test_Organism/160490.1/ -o output_directory -t 25`

Test_Organism/160490.1/ is a seed annotation directory for genome 'Streptococcus pyogenes M1 GAS'. 
You will find the output files of this genome at output_directory.


# TO RUN PHISPY

`% ./PhiSpy.py -i organism_directory -o output_directory -c`

where:
'output directory': Output directory is the directory where the final output file will be created.

'organism directory': The seed annotation directory for the input bacterial organism whose prophage(s) need to be identified. 

You can download the SEED genomes from the [PhAnToMe database](http://www.phantome.org/Downloads/genomes/seed/)

Or, 
If you have new genome, you can annotate it using the [RAST server](http://rast.nmpdr.org/rast.cgi). 
After annotation, you can download the genome directory from the server.

Or, 
If you have the GenBank file (containing sequence) of the genome, you can convert it using the following command:
`% python genbank_to_seed.py GenBank_file.gb organism_directory`

Now to run PhiSpy, use organism_directory as 'organism directory'. 
 

The program will access the following files in the organism_directory:
i.   contig file: organism_directory/contigs
ii.  tbl file for peg: organism_directory/Features/peg/tbl
iii. assigned_functions file: organism_directory/assigned_functions or organism_directory/proposed_functions or organism_directory/proposed_non_ff_functions  
iv.  tbl file for rna: organism_directory/Features/rna/tbl


# REQUIRED INPUT OPTION 

The program will take 1 command line input.

It shows a list (run with -c option) and asks for a number from the list. 
In the list, there are several organisms and each organism is associated by a number. 
If you find a closely related genome of your interested organism enter the number. PhiSpy will consider that genome as training genome.
Otherwise, enter 0 to run with generic training set.


# HELP

For the help menu use the -h option:
% python PhiSpy.py -h


# OUTPUT FILES

There are 2 output files, located in output directory.

1. prophage.tbl: This file has two columns separated by tabs [id, location]. 
The id should be in the format: fig|genomeid.pp.number, where genome id is the id of the genome that is being processed, 
and number is a sequential number of the prophage (starting at 1). 
Location should be in the format: contig_start_stop that encompasses the prophage.
 
2. prophage_tbl.txt: This is a tab seperated file. The file contains all the genes of the genome. The tenth colum represents the status of a gene. If this column is 1 then the gene is a phage like gene; otherwise it is a bacterial gene. 
This file has 16 columns:(i) fig_no: the id of each gene; (ii) function: function of the gene;	(iii) contig; (iv) start: start location of the gene; (v) stop: end location of the gene; (vi) position: a sequential number of the gene (starting at 1); (vii)	rank: rank of each gene provided by random forest; (viii) my_status: status of each gene based on random forest; (ix) pp: classification of each gene based on their function; (x) Final_status: the status of each gene, if this column is 1 then the gene is a phage like gene, otherwise it is a bacterial gene; (xi) start of attL; (xii) end of attL; (xiii) start of attR; (xiv) end of attR; (xv) sequence of attL; (xvi) sequence of attR.


 
