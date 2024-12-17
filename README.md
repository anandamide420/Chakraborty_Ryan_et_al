# Chakraborty_Ryan_et_al_Odak_et al
#A public Peer Review o Chakraborty et al and Ryan et al

#These tools are discussed in more detail below.

#https://anandamide.substack.com/p/chakraborty-open-review

#https://anandamide.substack.com/p/chakraborty-part-ii-odak-et-al

#Download reads from ENA nucleotide https://www.ebi.ac.uk/ena/browser/home
#Use SRR26589920	for Odak et al. 
#Use SRR19405260 for Ryan et al.

#once downloaded, Trim the reads with Cutadapt. Illumina reads from Odak et al require the below sequences

cutadapt -a AGATCGGAAGAGCACACGTCTGAACTCCAGTCAC -A AGATCGGAAGAGCGTCGTGTAGGGAAAGAGTGT -o output_R1.fastq -p output_R2.fastq input_R1.fastq input_R2.fastq

#for Ryan et al cutadapt must have adaptors from DNBSEQ G400
cutadapt \
    -a AAGTCGGAGGCCAAGCGGTCTTAGGAAGACAA \
    -A AAGTCGGATCGTAGCCATGTCGTTCTGTGAGCCAAGGAGTTG \
    -q 20 -m 30 \
    -o trimmed_R1.fastq.gz -p trimmed_R2.fastq.gz \
    input_R1.fastq.gz input_R2.fastq.gz

    #map reads to human trimmed Pfizer reference sequence (remove the polyA signals)
    #Pfizer vaccine sequence = chrome-extension://bigefpfhnfcobdlfbedofhhaibnlghod/mega/secure.html#file/dU4QiZIC#u7JzsOCae6GS9YCf0RvcjTXxMr--rfdU99FNRzF_ocg

    bwa mem -t 24 Pfizer_no_polyA.fasta input_R1.fastq.gz input_R2.fastq.gz | samtools sort -@ 24 -o sorted_output.bam
    samtools index sorted_output.bam

    #Open in IGV
    #load Genome ->Pfizer_no_polyA.fasta
    #load file ->sorted_output.bam
