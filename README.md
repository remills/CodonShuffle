# CodonShuffle

### Description ###

This package can be used to generate and analyze permuted sequences of an open reading frame from a viral genome. Permuted sequences are generated by shuffling bases, while preserving the protein coding sequence. The resulting sequences have large numbers of synonymous substitutions. These sequences may differ in various features determined by the nucleic acid sequence (e.g. dinucleotide frequency or free energy of RNA folding). Additional scripts are then used to quantify these differences relative to the unpermuted sequence, and a least squares method is applied to identify permuted sequences that are most similar to this "wild type."

___
###Instructions###

1. Download files and folders
2. Install dependencies by running the install_dependencies.py script
3. Run the python file (CodonShuffle.py)
3. Select sequence file (input_file.fas)  
4. Select a single permutation script to use (3n, dn23, dn31, dn231)
5. Select desired number of permuted sequences
6. Select the genomic feature to be evaluated 
7. Output graphs with the analysis

Files and Folders:

* CodonShuffle.py

Script to shuffle nucleotides and evaluate genomics features

Input Commands:

-i Input 

* Fasta file (input_file.fas) of open reading frame beginning with ATG

-s Permutation script 

* n3, dn23, dn31, or dn231. Specifies which of the permutation scripts are used (see below)

-r Number of replicates

* Number of permuted sequences that the program will generate, default is 1000

-m Genomic feature(s) to be used

* Specifies the genomic features incorporated into final least squares distance. Default is all. If only a subset are desired, the user must specify each: CAI, ENC, VFOLD, UFOLD, DN, CPB separated by spaces. Two options are given for assessing free energy of RNA folding as described below. UNAfold is more efficient, but only commercially available. ViennaRNA Fold is free. The user should specify which folding algorithm to use (if any) in the -m command.

-g Graphics

* When selected, CodonShuffle.py will generate graphs of distributions of values for all genomic features

-h Help

* Help menu

Outputs:

* Graphs of distribution of values for genomic features of permuted sequences. Each file will have the input sequence and permutation algorithm in the title with the following suffixes:

    _dn.pdf (graph of dinucleotide frequencies for permuted sequences)

    _dnls.pdf (graph of aggregate least squares value of overall dinucleotide bias for input and permuted sequences)

    fas.hamming.pdf (graph of hamming distance between permuted sequences and wild type)

    .fold.pdf (graph of minimum free energy of RNA folding for permuted sequences)

    .out.enc.pdf (graph of ENC for input and permuted sequences)

    .cai.pdf (graph of CAI for input and permuted sequences)
    
    .cpb.pdf (graph of CPB for input and permuted sequences)


* Graph showing Hamming distance versus Least Squares distance for all permuted sequences with suffix fasfinal_graph.pdf

* Table with hamming distance, values for each genomic feature, aggregate least squares distance with suffix _final_table.txt. Headers are as follows:

	sequence number (input sequence is 0)
	
	Distance.ls (overall least squares distance)
	
	Nucleotide_difference
	
	CPB (codon pair bias)
	
	DN_least_square (aggregate least squares value for 16 dinucleotide frequencies)
	
	DN..AA. etc. (individual dinucleotide frequency measurements)
	
	VFOLD.mfe (minimum free energy from Vienna RNA)
	
	ENC (effective number of codons)
	
	CAI (codon adaptive index)

* Multiple sequence fasta file with suffix .fas contains all permuted sequences with sequence identifiers (for example, replicate_1)

* Additional files contain intermediate data output during the CodonShuffle run.

    _least_square.txt, least square values for input and permuted sequences
    
    .blk, file with codon usage frequencies for input and permuted sequences
    
    .cpb, codon pair bias scores for input and permuted sequences
    
    .dn, frequencies for all 16 dinucleotides for input and permuted sequences
    
    .dnls, aggregate least squares value of overall dinucleotide bias for input and permuted sequences
    
    fas_distance_table.txt, hamming distance in nucleotides between each permuted sequence and the input
    
    .fasfold_table_mfe.txt, minimum free energies of RNA folding for permuted sequences
    
    .out, effective number of codon values for input and permuted sequences
    
    new_table_final_graph, tab delimited text file of least squares and Hamming distance for input and all permuted sequences
    
    .cai, codon adaptive index values for input and permuted sequences

Example Usage:

$python CodonShuffle.py -i Poliovirus_1_Mahoney_P1.fas -s dn23 -r 100 -m CAI ENC CPB -g

* Coleman_CPS.csv

Table with codon pair scores (CPS) for all 3721 possible codon pair combinations (excluding Stop codons) in the human ORFeome obtained from <a href="http://www.ncbi.nlm.nih.gov/pubmed/18583614" target="_blank">Coleman, et al, Science 2008</a>.  

* Poliovirus_1_Mahoney_P1.fas

Fasta sequence file of capsid region of Poliovirus 1 

* RNA_sliding_window.py

Input Commands:

-i Input

Fasta file (input_file.fas)

-s Seleted sequence

Name of selected sequence (use the the name after >)


Script to split selected sequence and compute minimum free energy. The splited sequence has 100 nucleotide and 80 overlap nucleotide. 

Outputs:

File with splited Sequence 

Graph with sliding window 

Example Usage:

$python RNA_sliding_window.py -i Poliovirus_1_Mahoney_P1.fas -s Replicate_2 

* install_dependencies.py

Script to install python library, configure and make Emboss package. This commmand will require admim password.  

Example Usage:

$sudo python install_dependencies.py

* Lib folders

Folder with the software necessary in the analysis

### Required third-party resources ###
* Python (Package: Numpy, Scipy, Matplotlib, Pandas, Statsmodels, Patsy, Biopython, Pyfasta and Ggplot)
* <a href="http://emboss.sourceforge.net/" target="_blank">Emboss</a>  
* <a href="http://codonw.sourceforge.net/" target="_blank">Codonw</a> 
* <a href="http://mfold.rna.albany.edu/" target="_blank">UNAfold</a> (unless user decides to use a free package for this analysis, see above)
* <a href="http://www.tbi.univie.ac.at/RNA/" target="_blank">ViennaRNA</a> 
* <a href="https://pip.pypa.io/en/latest/quickstart.html" target="_blank">Pip</a>

All dependencies will be installed by first running the install script. Simply open a terminal window and from the parent directory, type "sudo python install_dependencies.py" You will be prompted to enter your administrator password. Once installed, you will be able to run CodonShuffle. Additional required dependencies (i.e. CodonW and Emboss are installed by the main CodonShuffle.py command).

 

___
### Scripts for shuffling ###

The first step in the process is to generate permuted versions of the input sequence. Four different scripts are used, which shuffle the codons within an open reading frame, largely preserving the codon usage within the original sequence. The end result is a set of permuted sequences with a large number of synonymous mutations. Up to four different scripts can be used in the permutation step. They are based on scripts described in [Belalov and Lukashev, PLoS ONE 2013](http://journals.plos.org/plosone/article?id=10.1371/journal.pone.0056642). A fifth script described in this paper was not used as it will also change the GC content of the sequence. These are all now located within the main CodonShuffle.py script.

#####N3 
Shuffles the third position of each codon throughout a sequence.

#####Dn23
Shuffles dinucleotides consisting of the second and third position of each codon.

#####Dn31
Shuffles dinucleotides consisting of the third position of one codon and the first position of the next.

#####Dn231
Shuffles units consisting of the second and third position of one codon and the first position of the next.

___
### Analysis of Permuted Sequences ###

CodonShuffle.py will analyze the permuted sequences for a number of genomic features using the programs or scripts below. We have provided them separately should users decide to perform a separate analysis on their data. 

* Minimum free energy, calculated with _UNAfold_ or _ViennaRNA_

* Effective number of codon, calculated with _Codonw_

* Codon adaptation index, calculated with _Emboss_ 

* Codon pair bias, calculated with custom R script _CPB_calculation(n3, dN23, dN31, OR dN231)_

* Nucleotide distance, calculated with custom python script located within main CodonShuffle script

* Dinucleotide frequency, calculated with custom R script _Dinucleotide_Frequency.R_

* Dinucleotide bias, least squares regression based on dinucleotide frequencies, calculated with custom R script _Z-Score_normalization_Dinuc.R_

* Z-score normalization of each genomic feature, calculated with custom R script _Z-Score_normalization.R_

* To sort the final CodonShuffle.py output by aggregate least squares distance, use _Sort_sequences_by_Least_Square.R_

Genomic feature definition:

* Minimum free energy 

Compute minimum free energy of an RNA sequence. In our implementation, we used UNAfold, as it can handle large numbers of sequences efficiently. However, it is only commercially available. We include an option to use ViennaRNA, which is free, but may not handle large numbers of sequences well. 

* Effective number of codons, calculated with Emboss

Quantifies the codon usage of a sequence. This measure of synonymous codon usage bias, the 'effective number of codons used in a gene', can be calculated from codon usage data alone, and is independent of gene length and amino acid (aa) composition. It can range from 20, in the case of extreme bias where one codon is exclusively used for each aa, to 61 when the use of alternative
synonymous codons are equally likely.

* Codon adaptation index

Quantifies the codon usage of a sequence. CAI measures the deviation of a given protein coding gene sequence with respect to a reference set of genes.


* Codon pair bias

Quantifies the distribution of the codon pair in the sequence. CPB is defined as the arithmetic mean of the individual codon-pair scores within the sequence. Codon-pair score is defined as the natural log of the quotient of the observed occurrences of a given codon-pair divided by the expected number of occurrences of the codon-pair found.


* Nucleotide distance

Nucleotide difference distance of the new sequence from the original sequence (wild type)


___
### Scripts for Graphing Results###

CodonShuffle.py will generate output graphs if -g is selected. If the user wishes to generate these plots separately, the following scripts may be used:

* _ENC_graph.R_ (for effective number of codons)
* _Fold_graph.R_ (for free energy of RNA folding)
* _Graph_(CAI).R_ (for codon adaptation index based on human usage)
* _Hamming_Distance_Graph_(ggplot).R_ (for plotting Hamming distance versus least squares distance)
