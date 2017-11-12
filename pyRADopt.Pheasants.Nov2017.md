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


Example of barcode file which includes the RE sites: 

```
TCAGATGCAGG	TAGCACATGC	PHE079.1
CATGATGCAGG	GCATACATGC	PHE079.2
TCGAGTGCAGG	CTGGTCATGC	PHE079.3
GCATTTGCAGG	TAGCACATGC	PHE080.1
ACGTATGCAGG	GCATACATGC	PHE080.2
ATGCTTGCAGG	CTGGTCATGC	PHE080.3
TCAGATGCAGG	AGCTGTCCATGC	PHE081.1
CATGATGCAGG	GAGATGTCATGC	PHE081.2
```


Demultiplex using the output from the HiSeq. --disable_rad_check will allow the inclusion of the restriction site in the barcode

-r : rescue barcodes and radtags

-c : clean data, remove and read with an uncalled base

-q : discard reads with low quality scores (Q20)

-D : capture discarded reads to a file

For WPA data I ran process_radtags (only -r: rescue radtags. -c -q filters removed, since data will be cleaned downstream):

```

/usr/local/ngseq/stow/stacks-1.28/bin/process_radtags -i gzfastq -1 /rawData/Pheasant1_S1_L001_R1_001.fastq.gz  -2 /rawData/Pheasant1_S1_L001_R2_001.fastq.gz -o ./demultiplexed/ -y fastq -b ./barcodes/2015_barcodes --inline_inline --disable_rad_check -r -D

```
  
  
  
