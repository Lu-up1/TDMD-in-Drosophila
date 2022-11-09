## 1 TDMD identification analysis
### 1.1 Cutadapt in FASTQ
cutadapt -a TGGAATTCTCGGGTGCCAAG -A GATCGTCGGACTGTAGAACT -o `test_R1_cut.fastq` -p `test_R2_cut.fastq` `test_R1.fastq test_R2.fastq` --minimum-length 18 -j 10

### 1.2 Pear in FASTQ
pear -f `test_R1_cut.fastq` -r `test_R2_cut.fastq` -j 10 -o `test`

### 1.3 Collapse PCR duplicated reads
fastx_collapser -i `test.assembled.fastq` -o `test_collapsed.fasta`

### 1.4 Remove UMI sequences from 5' and 3' of FASTA
cutadapt -u 4 -u -4 -m 18 `test_collapsed.fasta` -o `test_cutN.fasta` -j 10              

### 1.5 Run hyb(https://github.com/gkudla/hyb) software
module load hyb

module load unafold

hyb analyse in=`test_cutN.fasta` db=`20220221_dm6_unique` type=mim pref=mim format=fasta

### 1.6 Potential TDMD miRNA-target RNA hybrids identification
python3 CLASH.py Viennad_to_Table -i `test_cutN_comp_20220221_dm6_unique_hybrids_ua` -c `dm6_all_conservation_score.txt` -t `20220221_dm6_unique.fasta` -n `martquery_1109155317_9_name.txt`

`20220221_dm6_unique.fasta` file size is 95MB.
  
`dm6_all_conservation_score.txt` file size is 1.4G.
  
`martquery_1109155317_9_name.txt` file size is 8.3MB.

  

## 2 miRNA analysis
### 2.1 Deduplicate clean-miRNA-seq reads from BGI
  python3 CLASH.py deduplicate_BGI -i `test_miRNA_BGI.fq`
### 2.2 miRNA abundance calculation
python3 CLASH.py miRNA_abundance -i `test_miRNA_BGI_deduplicated.fa` -d `20220221_dm6_miRNA_database.fasta`

### 2.3 Differential expression level analysis
`Deseq.R`

### 2.4 miRNA length distribution (isoform) count
python3 CLASH.py miRNA_length_distribution -i `test_miRNA_BGI_deduplicated.fa` -d `20220221_dm6_miRNA_database.fasta` -m `[--miRNA_sequence]`

e.g. miR-999 length distribution count

*python3 CLASH.py miRNA_length_distribution -i `test_miRNA_BGI_deduplicated.fa` -d `20220221_dm6_miRNA_database.fasta` -m TGTTAACTGTAAGACTGTGTCT*

## 3 poly-A RNA-seq analysis
### 3.1 Buind Drosophila genome database index by Hisat2
hisat2-build `GCF_000001215.4_Release_6_plus_ISO1_MT_genomic.fna drosphila`

### 3.2 Cutadapt in FASTQ
cutadapt -a AGATCGGAAGAGCACACGTCTGAACT -A AGATCGGAAGAGCGTCGTGTAGGGA -o `test_RNAseq_R1_cut.fq` -p `test_RNAseq_R2_cut.fq` -m 20 -j 12 `test_RNAseq_R1.fq` `test_RNAseq_R2.fq` 

### 3.3 Mapping
 hisat2 -x /blue/mingyi.xie/luli1/genome_index/Hisat2_Drosophila_genome/drosphila -1 ../Cut_data/M1_1_cut.fq -2 ../Cut_data/M1_2_cut.fq -S M1.sam -p 12

