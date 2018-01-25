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
 
  
 ### d. missingness per locus and per individual (heatmap)
  
  I can plot the missingness using vcfR https://knausb.github.io/vcfR_documentation/omitting_data.html
  
  First I have to gzip the vcf file and then import it into R: 
  
```
library(vcfR) 
vcf <- read.vcfR("WPAall.s3.vcf.gz")

dp <- extract.gt(vcf, element = "DP", as.numeric=TRUE) ##Creates a dataframe of the genotypes

vcf  ##gives info about the file
```

vcfR doesn't see ./. as missing data if any depth information is available. i.e. it thinks there's no missing data. But I can get around this by assigning all missing genotypes (coded 0 in dp) to NA. 

```
dp2 <- dp ##so I don't have to keep recreating the dp file

dp2(dp2==0) <- NA ##and I double checked with the vcf file if the missing data was in the correct place. 

vcf
***** Object of Class vcfR *****
69 samples
2049 CHROMs
2,049 variants
Object size: 2 Mb
11.32 percent missing data
*****        *****         *****
``` 

Okay, that corresponds with the 89% genotyping rate that i got before. I can create a really nice heatmap using vcfR, but I first want to sort the data by species so that we can compare the genotyping between them: 

``` 
dp2 <- as.data.frame(dp2)

dp3 <- dp2[c("PHE079", "PHE088", "PHE089", "PHE090", "PHE091", "PHE092", "PHE116", "PHE135", "PHE136", "PHE137", "PHE138", "PHE080", "PHE081", "PHE082", "PHE083", "PHE084", "PHE085", "PHE086", "PHE087", "PHE100", "PHE112", "PHE126", "PHE127", "PHE133", "PHE134", "PHE093", "PHE109", "PHE094", "PHE095", "PHE108", "PHE101", "PHE102", "PHE103", "PHE104", "PHE111", "PHE105", "PHE106", "PHE107", "PHE113", "PHE113.control1", "PHE113.control2", "PHE113.control3", "PHE139", "PHE139", "PHE140", "PHE141", "PHE142", "PHE143", "PHE144", "PHE145", "PHE146", "PHE147", "PHE148", "PHE149", "PHE150", "PHE150a", "PHE150b", "PHE150c", "PHE151", "PHE152", "PHE153", "PHE154", "PHE155", "PHE156", "PHE157", "PHE158", "PHE159", "PHE160", "PHE161", "PHE162")]

dp3 <- as.matrix(dp3)

pdf("WPA.Missingness.plot.pdf")
heatmap.bp(dp3[1:1000,], rlabels = FALSE)
dev.off()
``` 


## 3. Analyses as run by Edinburgh zoo before: 
  
  ### 3.1. Population Structure

  #### a. PCA 
    
```
library(pcadapt)

path_to_WPA <- "WPAall.s3.recode.vcf"

WPA.vcf <- read.pcadapt(path_to_WPA, type="vcf")

File attributes:
  meta lines: 10
  header line: 11
  variant count: 2049
  column count: 78
Meta line 10 read in.
All meta lines processed.
gt matrix initialized.
Character matrix gt created.
  Character matrix gt rows: 2049
  Character matrix gt cols: 78
  skip: 0
  nrows: 2049
  row_num: 0
Processed variant: 2049
All variants processed
 2049 variants have been processed.
 14 variants have been discarded as they are not SNPs.
 
```
 
 How many principal components explain the data? 
 
 I had to rename tmp.pcadapt to WPAall.s3.recode.pcadapt in bash (in the folder)
 ```
 x <- pcadapt(WPA.vcf, K=20)
 
pdf(WPA.pcadapt.screeplot.pdf)
plot(x,option="screeplot")  ##PC for pop structure = on the steep curve
dev.off()
```

And plot the PCA with populations coloured

```
poplist <- read.table("poplist", head=T)
attach(poplist) <- poplist[order(indiv),] ##make sure that this is in the same order as in the vcf file (check vcf with bcftools query -l)
poplist$species <- as.character(poplist$species) #to keep the order this has to be factorised

pdf("WPAall.pca.pdf")
plot(x,option="scores",pop=poplist$species)
dev.off()
```
 
 
  #### b. fastStructure with all individuals
  
Convert vcf to fastStructure format using pgdSpider. 

copy onto server and run K1-10. These have to specified manually
```
/usr/bin/python2.7 /usr/local/ngseq/src/fastStructure-1.0_20160929/structure.py -K 1 --format=str --input=WPAall.s3.str --output=WPA_K1.1

```

Choose the model complexity (How many Ks)
```
/usr/bin/python2.7 /usr/local/ngseq/src/fastStructure-1.0_20160929/chooseK.py --input=/srv/kenlab/alexjvr_p1795/WPA/Jan2018/WPAall.Analyses/fastStr/WPA

Model complexity that maximizes marginal likelihood = 5
Model components used to explain structure in data = 5
```

And now I'll create the structure plots for K5-8


Copy the output files to the mac. 


Graphs can be found here: WPAall.structure.plots_20180125.xlsx


#### c. DAPC

** I'm not using this analysis... 

Since fastStructure is not available immediately, I'll replicate the Zoo's analysis with DAPC to look at population assignment: 

```
library(adegenet)

WPA.dapc <- read.structure("WPAall.s3.str")  ##original structure input from vcf2str in pgdspider. 2049 loci, 69 individuals. Col 2 = pop assignment. 

##estimate the number of clusters

grp.WPAall <- find.clusters(WPA.dapc, max.n.clust=40)

##kept 70 PCs (i.e. all of them)
##Chose K=7; this is where the graph flattens off, and it is the expected number of species. 


dapc1.WPA <- dapc(WPA.dapc, grp.WPAall$grp)
scatter(dapc1.WPA)
```
 
 ### 3.2. Hybridisation with Mountain peacock (query individuals)
  
 First I'll subset all the data to include only the query samples, Mountain, Malaysian, Grey, and Bronze. 
 
 ```
 vcftools --vcf WPAall.s3.recode.vcf --keep Malaysian.Mountain.Grey.Bronze.names --recode --recode-INFO-all --out WPAsubset
 ```
  
  
  #### a. PCA with 
  
 
```    
path_to_WPAsubset <- "WPAsubset.recode.vcf"

WPAsubset.vcf <- read.pcadapt(path_to_WPAsubset, type="vcf")
 
```
 
 How many principal components explain the data? 
 
 I had to rename tmp.pcadapt to WPAall.s3.recode.pcadapt in bash (in the folder)
 ```
 x <- pcadapt(WPAsubset.vcf, K=20)
 
 Reading file WPAsubset.recode.pcadapt...
Number of SNPs: 2039
Number of individuals: 53
Number of SNPs with minor allele frequency lower than 0.05 ignored: 299
9014 out of 108067 missing data ignored.


pdf("WPAsubset.pcadapt.screeplot.pdf")
plot(x,option="screeplot")  ##PC for pop structure = on the steep curve
dev.off()
```

And plot the PCA with populations coloured

```
subset.poplist <- read.table("subset.poplist", head=T)
attach(subset.poplist) <- subset.poplist[order(indiv),] ##make sure that this is in the same order as in the vcf file (check vcf with bcftools query -l)
subset.poplist$species <- as.character(subset.poplist$species) #to keep the order this has to be factorised

pdf("WPAsubset.pca.pdf")
plot(x,option="scores",pop=subset.poplist$species)
dev.off()
```
  
 
#### b. fastStructure with Mountain, Malaysian, Bronze, Grey and queries
  

Convert vcf to fastStructure format using pgdSpider. 

copy onto server and run K1-10. These have to specified manually

/srv/kenlab/alexjvr_p1795/WPA/Jan2018/WPAall.Analyses/fastStr.subset
```
/usr/bin/python2.7 /usr/local/ngseq/src/fastStructure-1.0_20160929/structure.py -K 1 --format=str --input=WPAall.s3.str --output=WPA_K1.1

```

Choose the model complexity (How many Ks)
```
/usr/bin/python2.7 /usr/local/ngseq/src/fastStructure-1.0_20160929/chooseK.py --input=/srv/kenlab/alexjvr_p1795/WPA/Jan2018/WPAall.Analyses/fastStr.subset/WPAsubset

Model complexity that maximizes marginal likelihood = 3
Model components used to explain structure in data = 3
```

And now I'll create the structure plots for K3-5


Copy the output files to the mac. 


Graphs can be found here: WPAall.structure.plots_20180125.xlsx

  
  
###  3.3 Parentage analysis for Bornean peacock
    

subset the vcf file for only the Bornean pheasant. 

```
vcftools --vcf WPAall.s3.recode.vcf --keep Bornean.names --recode --recode-INFO-all --out WPA.Bornean

Keeping individuals in 'keep' list
After filtering, kept 5 out of 69 Individuals
Outputting VCF file...
After filtering, kept 2049 out of a possible 2049 Sites
Run Time = 0.00 seconds
```


#### a. Loci with maf >0.3


```
vcftools --vcf WPA.Bornean.recode.vcf --maf 0.3 --recode --recode-INFO-all --out WPA.Bornean.0.3

VCFtools - v0.1.14
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf WPA.Bornean.recode.vcf
	--recode-INFO-all
	--maf 0.3
	--out WPA.Bornean.0.3
	--recode

After filtering, kept 5 out of 5 Individuals
Outputting VCF file...
After filtering, kept 159 out of a possible 2049 Sites
Run Time = 0.00 seconds    
```
