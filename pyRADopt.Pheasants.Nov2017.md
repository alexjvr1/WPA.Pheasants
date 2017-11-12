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


I first need to split the file into fwd and reverse reads. 
Then I need to check for Illumina adapters
Then I need to reverse complement read2 and concatenate to the first read. (I think this has already been done)

Since I have paired-end reads I can also look for PCR duplicates which I couldn't do with any of my other data so far. 

  - Interesting test: How much PCR duplication in this dataset? 
  - Can we compare between species/data quality/Illumina lanes. 
  
  
  ## Demultiplex reads
  
  I'm using process_radtags to demultiplex the data. All data are here on the fmg server: 
  
  /srv/kenlab/alexjvr_p1795/WPA/Nov2017
  
```


```
  
  
  
