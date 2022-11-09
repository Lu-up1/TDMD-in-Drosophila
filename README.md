## TDMD identification analysis
### Cutadapt
cutadapt -a TGGAATTCTCGGGTGCCAAG -A GATCGTCGGACTGTAGAACT -o scr_1_R1.fq -p scr_1_R2.fq /orange/mingyi.xie/luli/Exp170_Dme/Scr-1-1_S1_L004_R1_001.fastq /orange/mingyi.xie/luli/Exp170_Dme/Scr-1-1_S1_L004_R2_001.fastq --minimum-length 18 -j 10![image](https://user-images.githubusercontent.com/80224396/200916134-1db35d4b-241a-4ad1-966e-e96cef964a75.png)



### miRNA abundance calculation
python3 CLASH.py miRNA_abundance -i [--input] <fasta/fastq> -d [--miRNA_database]


### Deduplicate clean-miRNA-seq reads from BGI
python3 CLASH.py deduplicate_BGI -i [--input] <fastq>

### Potential TDMD miRNA-target RNA hybrids identification
python3 CLASH.py Viennad_to_Table -i [--viennad] <TXT> -c [--transcript_ConservationScore_database] -t [--transcript_database] -n [--name_database]
