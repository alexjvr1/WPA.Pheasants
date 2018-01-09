# Pheasant Genomics Project

## Aims: 

Firstly to improve the SNP calling for the dataset by testing pyRAD. 

1. Mountain and Malaysian Peacock Pheasants 

- Identify pure and hybrid individuals
  
2. Bornean Peacock Pheasants

  - Identify hybrids

  - Relatedness
  

## iPyrad

I'm using a new version of pyrad: http://ipyrad.readthedocs.io/files.html

There's a stand alone installation for ipyrad and it can be installed on the local drive on the server without any permissions. 

The basic conda installation didn't work for me. I had to uninstall and reinstall everything using pip

```
pip uninstall ipyrad
pip install ipyrad
```




## Data

Paired-End ddRAD data sequenced on MiSeq. 

Restriction enzymes: 

SbfI (8bp recognition sites) TGCAGG-3'
 
SphI (7bp recognition site) CATGC-3'

39 individuals in total (40 samples). 

4 Species. 

Barcode information in: ddRAD_Info_to_TBray_11.2017.xlsx

Each sample was included 3 times. 

I first need to split the file into fwd and reverse reads. 
Then I need to check for Illumina adapters
Then I need to reverse complement read2 and concatenate to the first read. (I think this has already been done)

Since I have paired-end reads I can also look for PCR duplicates which I couldn't do with any of my other data so far. 

  - Interesting test: How much PCR duplication in this dataset? 
  - Can we compare between species/data quality/Illumina lanes. 
  
  
  ## Demultiplex reads
  
  I'm using process_radtags to demultiplex the data. All data are here on the fmg server: 
  
  /srv/kenlab/alexjvr_p1795/WPA/Nov2017


Example of barcode file which includes the RE sites (there should be tabs between columns, and only "-" or "_" as special characters: 

```
GCTAACATGCAGG	GTCAAGTCATGC	PHE113_2
AGGACACTGCAGG	AGCTGTCCATGC	PHE113_3
CTAGGACTGCAGG	CGATCCATGC	PHE113_1
GCTAACATGCAGG	GAAGCCATGC	PHE113_2
AGGACACTGCAGG	AGTCACATGC	PHE113_3
CGTATCATGCAGG	CGATCCATGC	PHE116_1
ACTGCACTGCAGG	GAAGCCATGC	PHE116_2
GTACACATGCAGG	AGTCACATGC	PHE116_3
```


Demultiplex using the output from the HiSeq. --disable_rad_check will allow the inclusion of the restriction site in the barcode

-r : rescue barcodes and radtags

-c : clean data, remove and read with an uncalled base

-q : discard reads with low quality scores (Q20)

-D : capture discarded reads to a file

For WPA data I ran process_radtags (only -r: rescue radtags. -c -q filters removed, since data will be cleaned downstream):

2015 samples
```
/usr/local/ngseq/stow/stacks-1.28/bin/process_radtags -i gzfastq -1 rawData/Pheasant1_S1_L001_R1_001.fastq.gz  -2 rawData/Pheasant1_S1_L001_R2_001.fastq.gz -o ./demultiplexed/ -y fastq -b barcodes/2015.barcodes --inline_inline --disable_rad_check -r -D


39417574 total sequences;
  4072468 ambiguous barcode drops;
  0 low quality read drops;
  0 ambiguous RAD-Tag drops;
35345106 retained reads.

This is only 10% ambiguous barcodes dropped - R.temp 30% on average
```
  
2017 samples
```
/usr/local/ngseq/stow/stacks-1.28/bin/process_radtags -i gzfastq -1 rawData/WildGenes23_S1_L001_R1_001.fastq.gz  -2 rawData/WildGenes23_S1_L001_R2_001.fastq.gz -o ./demultiplexed/ -y fastq -b barcodes/2017.barcodes --inline_inline --disable_rad_check -r -D

34564814 total sequences;
  21248178 ambiguous barcode drops;
  0 low quality read drops;
  0 ambiguous RAD-Tag drops;
13316636 retained reads.

This library included other samples, so I only retained ~40% of the reads. 
```

## Remove adapter dimer

First I concatenated all the copies of the forward and reverse sequences into one forward and one reverse file per sample. (each sample was sequenced three times). I'm just using cat - most of the software assumes that the reads are in the same order in both files. 

Next I'm removing adapter dimer using trimmomatic. 

For one sample
```
PHE079_R1.fq PHE079_R2.fq PHE079_R1.trimmed_PE.fq PHE079_R1.trimmed_SE.fq PHE079_R2.trimmed_PE.fq PHE079_R2.trimmed_SE.fq ILLUMINACLIP:/usr/local/ngseq/src/Trimmomatic-0.33/adapters/TruSeq3-PE.fa:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:36

Multiple cores found: Using 16 threads
Using PrefixPair: 'TACACTCTTTCCCTACACGACGCTCTTCCGATCT' and 'GTGACTGGAGTTCAGACGTGTGCTCTTCCGATCT'
ILLUMINACLIP: Using 1 prefix pairs, 0 forward/reverse sequences, 0 forward only sequences, 0 reverse only sequences
Quality encoding detected as phred33
Input Read Pairs: 260727 Both Surviving: 252866 (96.98%) Forward Only Surviving: 7076 (2.71%) Reverse Only Surviving: 685 (0.26%) Dropped: 100 (0.04%)
```

A small number of reads are being dropped because they're unpaired. I'm not sure why this is happening, because process_radtags should've already taken care of this. I'll have to read up on this a bit. 

I'm also having trouble specifying two variables in my loop. So I'll run trimmomatic individually for each sample: 

```
TrimmomaticPE: Started with arguments: PHE080_R1.fq PHE080_R2.fq PHE080_R1.trimmed_PE.fq PHE080_R1.trimmed_SE.fq PHE080_R2.trimmed_PE.fq PHE080_R2.trimmed_SE.fq ILLUMINACLIP:/usr/local/ngseq/src/Trimmomatic-0.33/adapters/TruSeq3-PE.fa:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:36
Multiple cores found: Using 16 threads
Using PrefixPair: 'TACACTCTTTCCCTACACGACGCTCTTCCGATCT' and 'GTGACTGGAGTTCAGACGTGTGCTCTTCCGATCT'
ILLUMINACLIP: Using 1 prefix pairs, 0 forward/reverse sequences, 0 forward only sequences, 0 reverse only sequences
Quality encoding detected as phred33
Input Read Pairs: 684997 Both Surviving: 663234 (96.82%) Forward Only Surviving: 19748 (2.88%) Reverse Only Surviving: 1732 (0.25%) Dropped: 283 (0.04%)
TrimmomaticPE: Completed successfully

TrimmomaticPE: Started with arguments: PHE081_R1.fq PHE081_R2.fq PHE081_R1.trimmed_PE.fq PHE081_R1.trimmed_SE.fq PHE081_R2.trimmed_PE.fq PHE081_R2.trimmed_SE.fq ILLUMINACLIP:/usr/local/ngseq/src/Trimmomatic-0.33/adapters/TruSeq3-PE.fa:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:36
Multiple cores found: Using 16 threads
Using PrefixPair: 'TACACTCTTTCCCTACACGACGCTCTTCCGATCT' and 'GTGACTGGAGTTCAGACGTGTGCTCTTCCGATCT'
ILLUMINACLIP: Using 1 prefix pairs, 0 forward/reverse sequences, 0 forward only sequences, 0 reverse only sequences
Quality encoding detected as phred33
Input Read Pairs: 591892 Both Surviving: 572911 (96.79%) Forward Only Surviving: 17438 (2.95%) Reverse Only Surviving: 1356 (0.23%) Dropped: 187 (0.03%)
TrimmomaticPE: Completed successfully

TrimmomaticPE: Started with arguments: PHE082_R1.fq PHE082_R2.fq PHE082_R1.trimmed_PE.fq PHE082_R1.trimmed_SE.fq PHE082_R2.trimmed_PE.fq PHE082_R2.trimmed_SE.fq ILLUMINACLIP:/usr/local/ngseq/src/Trimmomatic-0.33/adapters/TruSeq3-PE.fa:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:36
Multiple cores found: Using 16 threads
Using PrefixPair: 'TACACTCTTTCCCTACACGACGCTCTTCCGATCT' and 'GTGACTGGAGTTCAGACGTGTGCTCTTCCGATCT'
ILLUMINACLIP: Using 1 prefix pairs, 0 forward/reverse sequences, 0 forward only sequences, 0 reverse only sequences
Quality encoding detected as phred33
Input Read Pairs: 704827 Both Surviving: 683265 (96.94%) Forward Only Surviving: 19617 (2.78%) Reverse Only Surviving: 1694 (0.24%) Dropped: 251 (0.04%)
TrimmomaticPE: Completed successfully

TrimmomaticPE: Started with arguments: PHE083_R1.fq PHE083_R2.fq PHE083_R1.trimmed_PE.fq PHE083_R1.trimmed_SE.fq PHE083_R2.trimmed_PE.fq PHE083_R2.trimmed_SE.fq ILLUMINACLIP:/usr/local/ngseq/src/Trimmomatic-0.33/adapters/TruSeq3-PE.fa:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:36
Multiple cores found: Using 16 threads
Using PrefixPair: 'TACACTCTTTCCCTACACGACGCTCTTCCGATCT' and 'GTGACTGGAGTTCAGACGTGTGCTCTTCCGATCT'
ILLUMINACLIP: Using 1 prefix pairs, 0 forward/reverse sequences, 0 forward only sequences, 0 reverse only sequences
Quality encoding detected as phred33
Input Read Pairs: 528355 Both Surviving: 511498 (96.81%) Forward Only Surviving: 15375 (2.91%) Reverse Only Surviving: 1258 (0.24%) Dropped: 224 (0.04%)
TrimmomaticPE: Completed successfully

TrimmomaticPE: Started with arguments: PHE084_R1.fq PHE084_R2.fq PHE084_R1.trimmed_PE.fq PHE084_R1.trimmed_SE.fq PHE084_R2.trimmed_PE.fq PHE084_R2.trimmed_SE.fq ILLUMINACLIP:/usr/local/ngseq/src/Trimmomatic-0.33/adapters/TruSeq3-PE.fa:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:36
Multiple cores found: Using 16 threads
Using PrefixPair: 'TACACTCTTTCCCTACACGACGCTCTTCCGATCT' and 'GTGACTGGAGTTCAGACGTGTGCTCTTCCGATCT'
ILLUMINACLIP: Using 1 prefix pairs, 0 forward/reverse sequences, 0 forward only sequences, 0 reverse only sequences
Quality encoding detected as phred33
Input Read Pairs: 667704 Both Surviving: 645327 (96.65%) Forward Only Surviving: 20485 (3.07%) Reverse Only Surviving: 1633 (0.24%) Dropped: 259 (0.04%)
TrimmomaticPE: Completed successfully

```







Code I tried. Doesn't work. 
```
for file in demultiplexed/*_R1.fq; do withpath="${file}"; filename="demultiplexed/"; base="${filename%*_*.fq}"; echo "${base}"; 
java -jar /usr/local/ngseq/src/Trimmomatic-0.33/trimmomatic-0.33.jar PE 
"${base}"*_R1.fq 
"${base}"*_R2.fq 
"${base}"*_R1.trimmed_PE.fq {base}"*_R1.trimmed_SE.fq 
"${base}"*_R2.trimmed_PE.fq {base}"*_R1.trimmed_SE.fq 
ILLUMINACLIP:/usr/local/ngseq/src/Trimmomatic-0.33/adapters/TruSeq3-PE.fa:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:36; done
```


## iPyrad test

So now I've demultiplexed, removed adapters, and combined multiple reads into one. 

I've also renamed all the files to have _R1_ and _R2_ as is needed by ipyrad. 

Aim: 

1. Run iPyrad on a subset of samples to test whether it works. 

2. Test different clustering thresholds (85-95%) to find the optimal clustering threshold across all species


Representatives of different species used for test: 

### Malayan

PHE080, PHE081

### Bornean

PHE101, PHE102

### Mountain

PHE088, PHE089

### Germains

PHE093, PHE109

### Palawan

PHE095, PHE108
