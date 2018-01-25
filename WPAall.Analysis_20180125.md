# SNP filtering and Analysis of WPA data

I've selected 93% as the optimal clustering threshold for the WPA data. 

Aim here: 

1. Filter variants

2. statistics on output
  
  a. pyradOpt graphs
  
  b. number of loci -> filtering -> final loci
  
  c. number of reads per individual
  
  d. missingness per locus and per individual (heatmap)
  
3. Analyses as run by Edinburgh zoo before: 
  
  3.1. Population Structure

    a. PCA 
    
    b. fastStructure with all individuals
    
  3.2. Hybridisation with Mountain peacock (query individuals)
  
    a. PCA with 
      
      i. Mountain Peacock and Grey (query individuals)
      
      ii. Mountain Peackock and Bronze
      
  
  3.3. Hybridisation with Malayan peacock
  
  a. PCA with 
      
      i. Malayan Peacock and Grey
      
      ii. Malayan Peackock and Bronze
      
      
  3.4 Parentage analysis for Bornean peacock
    
    a. Number of variable loci in Bornean peacock
    
    b. Loci with maf >0.3
    
    c. Obs heterozygosity
    



## 1. Filter variants

I'm using the vcf output from pyRAD 

/srv/kenlab/alexjvr_p1795/WPA/Jan2018


First I'm filtering for maf of 5%

```
vcftools --vcf WPAallMerged.vcf --maf 0.05 --recode --recode-INFO-all --out WPAall.s1

After filtering, kept 69 out of 69 Individuals
After filtering, kept 125671 out of a possible 184264 Sites
Run Time = 1.00 seconds
```

Next filter all loci genotyped in less than 80% of the individuals

```
vcftools --vcf WPAall.s1.recode.vcf --max-missing 0.8 --recode --recode-INFO-all --out WPAall.s2

After filtering, kept 69 out of 69 Individuals
After filtering, kept 15618 out of a possible 125671 Sites
Run Time = 0.00 seconds
```

Finally reduce linkage by filtering for one SNP per locus (assuming loci are 400bp in length)

```
vcftools --vcf WPAall.s2.recode.vcf --thin 400 --recode --recode-INFO-all --out WPAall.s3

After filtering, kept 69 out of 69 Individuals
After filtering, kept 2049 out of a possible 125671 Sites
Run Time = 0.00 seconds
```




2. statistics on output
  
  a. pyradOpt graphs
  
  b. number of loci -> filtering -> final loci
  
  c. number of reads per individual
  
  d. missingness per locus and per individual (heatmap)
  
3. Analyses as run by Edinburgh zoo before: 
  
  3.1. Population Structure

    a. PCA 
    
    b. fastStructure with all individuals
    
  3.2. Hybridisation with Mountain peacock (query individuals)
  
    a. PCA with 
      
      i. Mountain Peacock and Grey (query individuals)
      
      ii. Mountain Peackock and Bronze
      
  
  3.3. Hybridisation with Malayan peacock
  
  a. PCA with 
      
      i. Malayan Peacock and Grey
      
      ii. Malayan Peackock and Bronze
      
      
  3.4 Parentage analysis for Bornean peacock
    
    a. Number of variable loci in Bornean peacock
    
    b. Loci with maf >0.3
    
    c. Obs heterozygosity
