# MyGenome
Analysis for ABT/CS485G genome assembly

## 1. Analysis of sequence quality
The F1 and R1 datasets were analyzed using fastqc
```bash
ssh -Y sfmc225@sfmc225.cs.uky.edu
cd MyGenome
fastqc &
```

Load F1 and R1 datasets into GUI interface
Take screenshots of output files:

![Folder for screenshots](/data/fastqc/ss1.png)

## Ran trimmomatic
```bash
java -jar ~/sequences/trimmomatic-0.38.jar PE -threads 2 -phred33 -trimlog UFVPY210_errorlog.txt UFVPY210_1.fq.gz UFVPY210_2.fq.gz UFVPY210_1_paired.fq.gz UFVPY210_1_unpaired.fq.gz UFVPY210_2_paired.fq.gz UFVPY210_2_unpared.fq.gz ILLUMINACLIP:adaptors.fasta:2:30:10 SLIDINGWINDOW:20:20 MINLEN:120
```

## Count number of forward reads remaining
```
zgrep '' UFVPY210_1_paired.fq.gz | zgrep AGGCAGAAAT+ATAGAGAGGT -A 1 | zgrep -v AGGCAGAAAT | zgrep -v ^- | zgrep '[AGTCN]' -o | wc -l
```

## Count number of bases
```bash
cat UFVPY210_1_paired.fq.gz UFVPY210_2_paired.fq.gz | zgrep '' | zgrep AGGCAGAAAT+ATAGAGAGGT -A 1 | zgrep -v AGGCAGAAAT | zgrep -v ^- | zgrep '[AGTCN]' -o | wc -l
```

## 2. Genome assembly

## Run velvet with a range of k-mer values and step size 10
```bash
 sbatch velvetoptimiser_noclean.sh UFVPY210 61 131 10
```
We will use this to better narrow down our k-mer values

![Output](Data/velvet_61_131_10_output.txt)

## Run velvet with more optimal k-mer values and step size 2
```bash
sbatch velvetoptimiser_noclean.sh UFVPY210 81 121 2
```

![Output](Data/velvet_81_121_2_output.txt)

## 3. BLASTing my genome

## Run a blastn search using the sequence in MoRepeats.fasta as the query

```bash
blastn -subject UFVPY210_nh.fasta -query UFVPY210_nh.fasta -out UFVPY210_genomeBLASTn0 -evalue 1e-20 -outfmt 0
```

## Record results for outfmt 6

```bash
blastn -subject UFVPY210_nh.fasta -query UFVPY210_nh.fasta -out MoRepeats.UFVPY210_genomeBLASTn6 -evalue 1e-20 -outfmt 6
```

## BLASTing against the mitochondrial genome and the B71v2sh reference genome


```bash
blastn -query MoMitochondrion.fasta -subject UFVPY210_final.fasta -evalue 1e-50 -max_target_seqs 20000 -outfmt '6 qseqid sseqid slen length qstart qend sstart sed btop' -out MoMitochondrion.MyGenome.BLAST
```

```bash
awk '$4/$3 > 0.9 {print $2 ",mitochondrion"}' B71v2sh.UFVPY210.BLAST > UFVPY210_mitochondrion.csv
```

![Result CSV file](Data/UFVPY210_mitochondrion.csv)

## 4. Assessing genome completeness using BUSCO

## 5. Gene prediction

## Running SNAP

```bash
 snap-hmm Moryzae.hmm UFVPY210_final.fasta > UFVPY210-snap.zff
```

## Use fathom to compute statistics from the ZFF and FASTA files
```bash
 fathom UFVPY210-snap.zff UFVPY210_final.fasta -gene-stats
```

## making GFF file in the older GFF2 format

```bash
 snap-hmm Moryzae.hmm UFVPY210_final.fasta -gff > UFVPY210-snap.gff2
```

## Running Augustus
```bash
augustus --species=magnaporthe_grisea --gff3=on --singlestrand=true --progress=true ../snap/UFVPY210_final.fasta > UFVPY210-augustus.gff3
```
![Augustus result](Data/UFVPY210-augustus.gff3)



