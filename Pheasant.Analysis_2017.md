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


I first need to split the file into fwd and reverse reads. 
Then I need to check for Illumina adapters
Then I need to reverse complement read2 and concatenate to the first read. (I think this has already been done)

Since I have paired-end reads I can also look for PCR duplicates which I couldn't do with any of my other data so far. 

  - Interesting test: How much PCR duplication in this dataset? 
  - Can we compare between species/data quality/Illumina lanes. 

The data we got wasn't raw data. They are single .fq files per individual, with _1 and _2 reads (not interleaved). I think the reverse reads have already been reverse complemented here, but I'm not sure. 

Side note: All 8 query individuals have the lines annotated /1 and /2 for fwd and rev. (PHE126, 127, 133-138). 

First I'll need to split the file into fwd and reverse reads. 

Check at which line the reverse reads start (this is for all reference individuals). 
```
for i in *.fq
do grep -nr '_2$' $i > $i.lines
done

head -n 1 *lines
```

And then split the data into two files
```

```


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





