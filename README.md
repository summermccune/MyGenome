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

![F1screenshot.png](/data/F1screenshot.png)

## 2. Ran trimmomatic
```bash
java -jar ~/sequences/trimmomatic-0.38.jar PE -threads 2 -phred33 -trimlog UFVPY210_errorlog.txt UFVPY210_1.fq.gz UFVPY210_2.fq.gz UFVPY210_1_paired.fq.gz UFVPY210_1_unpaired.fq.gz UFVPY210_2_paired.fq.gz UFVPY210_2_unpared.fq.gz ILLUMINACLIP:adaptors.fasta:2:30:10 SLIDINGWINDOW:20:20 MINLEN:120
```

## 3. Count number of forward reads remaining
```
grep ....
```
