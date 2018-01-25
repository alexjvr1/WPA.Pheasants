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
    
