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

To estimate whether populations were inbred, we used two approaches: 

1) estimating the inbreeding coefficient. That is the 
