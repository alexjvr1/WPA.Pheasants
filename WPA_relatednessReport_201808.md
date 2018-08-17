# WPA report on relatedness

Analyses by Vilhelmiina Haavisto & Alexandra Jansen van Rensburg


Aim: calculate relatedness and inbreeding across the dataset of 63 individuals genotyped with genome-wide ddRAD loci. 



## Data

De novo assembly and variant calling has previously been reported (WPA report Feb 2018: Genetic analysis for captive management of Peacock 
Pheasants, Feb 2018). 

Relatedness analyses and estimates of heterozygosity can be affected by missingness in the dataset (ref). Thus we filtered the pyRAD 
variant call file (vcf) to remove 1) SNPs with a minor allele frequency of less than 5%, and 2) SNPs that were genotyped in less than 80% of 
the individuals. 3) We also filtered for linkage by retaining only one SNP per locus.  

```


```



## Relatedness

We estimated relatedness between individuals based on the KING method. This 


```

```

The output from vcftools was processed and plotted using R
```


```


## Inbreeding 

To estimate whether populations were inbred, we estimated the method-of-moments inbreeding coefficient (F) in PLINK. 
([observed hom. count] - [expected count]) / ([total observations] - [expected count]))

Where total observations is the number of genotyped loci. 

This analysis was run separately for each species, as the analysis depends on the within species MAF and heterozygosities. 
For small population size MAF cannot be imputed accurately, thus --read-freq is used to read in the MAF based on allele counts. 

These analyses should be conducted on LD-pruned datasets. I've done this with --thin in vcftools (keep only 1 SNP per locus). 
However, this does not exclude loci in linkage equilibrium that are on different loci. Thus I used PLINK to filter for further for LD

```




```


I split the vcf files so that I had one for each species. Then filtered for missingness and MAF within each of the datasets. 
Once I'd done this, there were <600 loci left within each dataset, and as few as 20 for the small datasets. Given the limited 
data I cannot calculate inbreeding at this point. 
