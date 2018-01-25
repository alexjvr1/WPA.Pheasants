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

Final output: 

2049 loci genotyped for 69 individuals. 

Genotyping rate of 0.886066


## 2. statistics on output
  
  ### a. pyradOpt graphs
  
To select the optimum number of clusters I ran pyRAD for 85-95% clustering across 10 individuals (2 per species). 

I copied all the stats output files to my computer: /Users/alexjvr/2016RADAnalysis/WPA/Jan2018_pyRADOpt

I copied the final tables from each of these into excel, added the clustering threshold and loaded this into R.

Graphs: pyradOpt.plots_20170124.pdf (on my computer). 

According the the graphs, it looks like the optimal clustering threshold is between 90-93%. For over clustering (low threshold) we expect high heterozygosity and low numnbers of loci, and for over splitting (high threshold) we expect low heterozygosity and increased loci. 

But remember that I have a filter for min-depth 2. So I see a decrease in the number of loci found at 93%. To me it looks like the heterozygosity decreases more rapidly at ~90 clustering threshold. At 94% loci are split up so much that even within species they are not the same anymore. 

I could probably choose a lower threshold... 

 ### b. number of loci -> filtering -> final loci
 
 
| Filter |	number of sites |
|-------------------------------------------------------|------|
|variants called by pyRAD	|184264|
|remove loci with minor allele frequency <5%	|125671|
|remove loci genotyped in less than 80% of individuals	|15618|
|reduce to one SNP per locus|	2049|
  
  
 ### c. number of loci called per individual
 
 Number of consensus reads in the final dataset. 
 
 in R: 
 
 ```
 library(ggplot2)
 
 poplist <- read.table("poplist", header=T)
 
 head(poplist)
 
    indiv   species         colour
1 PHE079  Mountain mediumseagreen
2 PHE080 Malaysian        hotpink
3 PHE081 Malaysian        hotpink
4 PHE082 Malaysian        hotpink
5 PHE083 Malaysian        hotpink
6 PHE084 Malaysian        hotpink
 
 
WPAall.stats <- read.table("WPAall.stats", head=T)  ##copied from the end of the final ipyrad outfile

head(WPAall.stats)

    Indiv state reads_raw reads_passed_filter clusters_total clusters_hidepth
19 PHE101     7    736916              734096          29156            11443
20 PHE102     7    884581              880236          33845            11991
21 PHE103     7    503629              502065          24538            10423
22 PHE104     7    806535              803410          33746            11801
28 PHE111     7    574081              571910          28709            10871
30 PHE113     7    519350              518017          33144            10067
   hetero_est error_est reads_consens loci_in_assembly species      colour
19   0.005720  0.001546          9027             7352 Bornean dodgerblue1
20   0.005668  0.001632          8929             7191 Bornean dodgerblue1
21   0.005242  0.001591          9310             7536 Bornean dodgerblue1
22   0.006352  0.001682          9082             7270 Bornean dodgerblue1
28   0.005823  0.001606          9494             7650 Bornean dodgerblue1
30   0.003841  0.000900          9020             7522  Bronze darkorchid3 


WPAall.stats$species <- poplist$species
WPAall.stats$colour <- poplist$colour

attach(WPAall.stats) 
WPAall.stats <- WPAall.stats[order(WPAall.stats$species),] ##order by species name

WPAall.stats$Indiv <- factor(WPAall.stats$Indiv, levels=WPAall.stats$Indiv) ##keep the order of all the individuals

p22 <- ggplot(WPAall.stats, aes(x=Indiv, y=reads_consens)) + geom_bar(stat="identity", colour=WPAall.stats$colour)  ##barplot

pdf("WPAall.consensusReads.perIndiv.pdf")
p22
dev.off()
 ```
 
  
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
