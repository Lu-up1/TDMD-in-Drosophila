
### miRNA abundance calculation
python3 CLASH.py miRNA_abundance -i [--input] <fasta/fastq> -d [--miRNA_database]


### Deduplicate clean-miRNA-seq reads from BGI
python3 CLASH.py deduplicate_BGI -i [--input] <fastq>

### Potential TDMD miRNA-target RNA hybrids identification
python3 CLASH.py Viennad_to_Table -i [--viennad] <TXT> -c [--transcript_ConservationScore_database] -t [--transcript_database] -n [--name_database]
