## TDMD identification analysis
### Cutadapt FASTQ
cutadapt -a TGGAATTCTCGGGTGCCAAG -A GATCGTCGGACTGTAGAACT -o test_R1_cut.fastq -p test_R2_cut.fastq test_R1.fastq test_R2.fastq --minimum-length 18 -j 10

### Pear FASTQ
pear -f test_R1_cut.fastq -r test_R2_cut.fastq -j 10 -o test

### miRNA abundance calculation
python3 CLASH.py miRNA_abundance -i [--input] <fasta/fastq> -d [--miRNA_database]


### Deduplicate clean-miRNA-seq reads from BGI
python3 CLASH.py deduplicate_BGI -i [--input] <fastq>

### Potential TDMD miRNA-target RNA hybrids identification
python3 CLASH.py Viennad_to_Table -i [--viennad] <TXT> -c [--transcript_ConservationScore_database] -t [--transcript_database] -n [--name_database]
