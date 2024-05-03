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
Output log file from this run:
![velvet_output1.txt](MyGenome/Data/velvet_61_121_output.txt)

## Run velvet with more optimal k-mer values and step size 2
```bash
sbatch velvetoptimiser_noclean.sh UFVPY210 81 121 2
```

## 3. BLASTing my genome

## 4. Assessing genome completeness using BUSCO

## 5. Gene prediction

