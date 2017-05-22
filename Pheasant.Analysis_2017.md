# Pheasant Genomics Project

## Aims: 

Firstly to improve the SNP calling for the dataset by testing pyRAD. 

1. Mountain and Malaysian Peacock Pheasants 

- Identify pure and hybrid individuals
  
2. Bornean Peacock Pheasants

  - Identify hybrids

  - Relatedness
  

##Data

Paired-End ddRAD data sequenced on MiSeq. 

39 individuals in total (40 samples). 

4 Species. 



##pyRAD

I'll test pyrad clustering using individuals from all the 4 species of Pheasant in the test runs. I'm using the samples specified as reference
individuals in the first report. 

I'll test clustering thresholds of 85-95% to start with. 


Chosen 7 individuals. I've tried to use reference birds with DNA taken from muscle/tissue where possible: 

Mountain: PHE088 & PHE089

Malaysian: PH086 & PHE087

Grey: PHE105 & PHE106

Bronze-tailed: PHE113


Running this on the FGCZ server. 

This is PE ddRAD, so I have to adjust the way I run pyRAD. 

I have to reverse complement the second read and concatenate it to the first read. So sequences are now longer per locus (~250bp)





