# Pheasant Genomics Project

## Aims: 

Firstly to improve the SNP calling for the dataset by testing pyRAD. 

1. Mountain and Malaysian Peacock Pheasants 

- Identify pure and hybrid individuals
  
2. Bornean Peacock Pheasants

  - Identify hybrids

  - Relatedness
  

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

Use Trimmomatic to remove adapter dimer 


```
screen -S TrimSubset -L
for i in *.fq; do  java -jar /usr/local/ngseq/src/Trimmomatic-0.33/trimmomatic-0.33.jar PE $i $i.trim ILLUMINACLIP:/usr/local/ngseq/src/Trimmomatic-0.33/adapters/TruSeq3-PE.fa:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:36;done
```



