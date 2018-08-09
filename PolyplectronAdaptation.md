# Adaptation to environment in pheasants. 


### Questions

1. What is the population structure of the peacock pheasants in SE Asia? 

2. Can we resolve the demographic history of peacock pheasants in SE Asia? 

3. Can we identify signals of adaptation to environment (elevation) in P.malacense and P.imopinatum? 

4. How do these loci fit in with what we know from other species? 


## Data

The data are on the fgcz47 server

alexjvr@fgcz-c-047.uzh.ch

pwd Bombin@15


1. Select MalaysianPP and MountainPP 
```
pwd 
 /srv/kenlab/alexjvr_p1795/WPA/MS_Pheasants.AdaptationtoElevation/Data_Aug2018

awk '$3=="MountainPP" {print $1} $3=="MalaysianPP" {print $1}' WPAall.names > indivs.tokeep

```


##### Dataset1: popGen

2. Subset vcf file to keep only these individuals
```
vcftools --vcf WPAallMerged.vcf --keep indivs.tokeep --recode --recode-INFO-all --out WPA47

VCFtools - 0.1.15
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf WPAallMerged.vcf
	--keep indivs.tokeep
	--recode-INFO-all
	--out WPA50
	--recode

Keeping individuals in 'keep' list
After filtering, kept 50 out of 69 Individuals
Outputting VCF file...
After filtering, kept 184264 out of a possible 184264 Sites
Run Time = 16.00 seconds

```

3. Filter SNPs

3.1: filter loci genotyped in less than 80% of individuals 
```
vcftools --vcf WPA47.recode.vcf --max-missing 0.8 --recode --recode-INFO-all --out WPA47.s1

VCFtools - 0.1.15
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf WPA47.recode.vcf
	--recode-INFO-all
	--max-missing 0.8
	--out WPA47.s1
	--recode

After filtering, kept 47 out of 47 Individuals
Outputting VCF file...
After filtering, kept 36570 out of a possible 184264 Sites
Run Time = 6.00 seconds


```


3.2: Filter all loci with minor allele frequency <5%
```
vcftools --vcf WPA47.s1.recode.vcf --maf 0.05 --recode --recode-INFO-all --out WPA47.s2

VCFtools - 0.1.15
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf WPA47.s1.recode.vcf
	--recode-INFO-all
	--maf 0.05
	--out WPA47.s2
	--recode

After filtering, kept 47 out of 47 Individuals
Outputting VCF file...
After filtering, kept 14391 out of a possible 36570 Sites
Run Time = 1.00 seconds

```


3.3: Keep only a single SNP per locus 
```
vcftools --vcf WPA47.s2.recode.vcf --thin 300 --recode --recode-INFO-all --out WPA47.s3

vcftools --vcf WPA47.s2.recode.vcf --thin 300 --recode --recode-INFO-all --out WPA47.s3

VCFtools - 0.1.15
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf WPA47.s2.recode.vcf
	--recode-INFO-all
	--thin 300
	--out WPA47.s3
	--recode

After filtering, kept 47 out of 47 Individuals
Outputting VCF file...
After filtering, kept 2260 out of a possible 14391 Sites
Run Time = 1.00 seconds

```

3.4 Check for loci that deviate from HWE

```
vcftools --vcf WPA47.s3.recode.vcf --plink --out WPA47

plink --file WPA47 --hardy   ##this has to be run on my laptop

awk '$7>0.6 {print $0}' plink.hwe   ##look for columns where Observed Het exceeds 0.6

awk '$7>0.5 {print $0}' plink.hwe ##look for columns where Observed Het exceeds 0.5

CHR             SNP     TEST   A1   A2                 GENO   O(HET)   E(HET)            P 
   0  locus_31539:49  ALL(NP)    C    T              4/21/16   0.5122   0.4572       0.7314

```

No need to filter for HWE. 


##### Dataset2: 



3. Filter SNPs

3.1: filter loci genotyped in less than 80% of individuals 
```
vcftools --vcf WPA47.recode.vcf --max-missing 0.5 --recode --recode-INFO-all --out WPA47.AdaptLoci.s1

VCFtools - 0.1.15
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf WPA47.recode.vcf
	--recode-INFO-all
	--max-missing 0.5
	--out WPA47.AdaptLoci.s1
	--recode

After filtering, kept 47 out of 47 Individuals
Outputting VCF file...
After filtering, kept 82182 out of a possible 184264 Sites
Run Time = 9.00 seconds
```


3.2: Filter all loci with minor allele frequency <5%
```
vcftools --vcf WPA47.AdaptLoci.s1.recode.vcf --maf 0.05 --recode --recode-INFO-all --out WPA47.AdaptLoci.s2

VCFtools - 0.1.15
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf WPA47.AdaptLoci.s1.recode.vcf
	--recode-INFO-all
	--maf 0.05
	--out WPA47.AdaptLoci.s2
	--recode

After filtering, kept 47 out of 47 Individuals
Outputting VCF file...
After filtering, kept 37675 out of a possible 82182 Sites
Run Time = 4.00 seconds
```


3.3: Keep only a single SNP per locus 
```
vcftools --vcf WPA47.AdaptLoci.s2.recode.vcf --thin 300 --recode --recode-INFO-all --out WPA47.AdaptLoci.s3

VCFtools - 0.1.15
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf WPA47.AdaptLoci.s2.recode.vcf
	--recode-INFO-all
	--thin 300
	--out WPA47.AdaptLoci.s3
	--recode

After filtering, kept 47 out of 47 Individuals
Outputting VCF file...
After filtering, kept 6042 out of a possible 37675 Sites
Run Time = 0.00 seconds
```

3.4 Check for loci that deviate from HWE

```
vcftools --vcf WPA47.AdaptLoci.s3.recode.vcf --plink --out WPA47.Adapt

plink --file WPA47 --hardy   ##this has to be run on my laptop

awk '$7>0.5 {print $0}' plink.hwe
 CHR             SNP     TEST   A1   A2                 GENO   O(HET)   E(HET)            P 
   0   locus_18517:1  ALL(NP)    G    A              7/18/10   0.5143   0.4963            1
   0   locus_21387:8  ALL(NP)    T    C               3/13/8   0.5417   0.4783       0.6833
   0  locus_13530:17  ALL(NP)    A    T               3/13/9     0.52   0.4712            1
   0  locus_18963:31  ALL(NP)    G    A               2/15/7    0.625   0.4783       0.2168
   0   locus_3083:37  ALL(NP)    T    C               1/15/8    0.625   0.4575       0.1753
   0     locus_37:43  ALL(NP)    T    C              1/13/10   0.5417   0.4297       0.3567
   0  locus_31539:49  ALL(NP)    C    T              4/21/16   0.5122   0.4572       0.7314
   0  locus_14098:70  ALL(NP)    C    G              2/17/11   0.5667    0.455        0.255
   0  locus_22623:87  ALL(NP)    A    G               3/13/8   0.5417   0.4783       0.6833
   0 locus_15842:146  ALL(NP)    T    C              4/19/10   0.5758   0.4835         0.47
   0  locus_1209:153  ALL(NP)    C    A              1/15/10   0.5769   0.4401       0.1956
   0 locus_18440:190  ALL(NP)    T    C               2/14/8   0.5833   0.4688       0.3916

awk '$7>0.5 {print $0}' plink.hwe |wc -l
      13
```

Remove these 12 loci from the dataset. (first line is a header)

```
awk '$7>0.5 {print $0}' plink.hwe > locitoremove

sed -i 's/:/\t/g' locitoremove

vcftools --vcf WPA47.AdaptLoci.s3.recode.vcf --exclude-positions locitoremove --recode --recode-INFO-all --out WPA47.AdaptLoci.s4

VCFtools - 0.1.15
(C) Adam Auton and Anthony Marcketta 2009

Parameters as interpreted:
	--vcf WPA47.AdaptLoci.s3.recode.vcf
	--exclude-positions locitoremove
	--recode-INFO-all
	--out WPA47.AdaptLoci.s4
	--recode

After filtering, kept 47 out of 47 Individuals
Outputting VCF file...
After filtering, kept 6030 out of a possible 6042 Sites
Run Time = 1.00 seconds

```

### Final Datasets

#### 1. For population Structure Analyses

47 individuals

P.imopinatum (MountainPP) n=24

P.malacense (MalaysianPP) n=23

2260 loci

Total genotyping rate is 0.89126

```
/srv/kenlab/alexjvr_p1795/WPA/MS_Pheasants.AdaptationtoElevation/Data_Aug2018/WPA47.s3.recode.vcf
```

#### 2. Data for outlier analyses


6030 loci
Total genotyping rate is 0.736587
```

```











########################################################################################################################
# Familiarisation with the data, R, and Linux 
########################################################################################################################
## Data

The data are on the fgcz47 server

alexjvr@fgcz-c-047.uzh.ch

pwd Bombin@15

```
vcf file: /srv/kenlab/alexjvr_p1795/WPA/Jan2018/WPAall.Analyses/WPAall.s3.recode.vcf
```

You can get a list of the individuals (in order) in the vcf file with this commnand: 
```
# linux. Run this on the server as bcftools is installed there already. 

bcftools query -l [vcf.file.name]     ### the square brackets is a placeholder for whatever vcf file you want to look at. You can copy and paste these names out onto your computer. 
```
I've made a file with all the indiv species names here: 
```
alexjvr@fgcz-c-047.uzh.ch:/srv/kenlab/alexjvr_p1795/WPA/Jan2018/WPAall.Analyses/WPAall.names
```
You can copy that over to your computer using scp again. Or use cat WPAall.Analyses/WPAall.names to see it all on your screen, and copy and paste it onto your computer. The file as is (WPAall.names) can be read into R as a table with headers: 
.e.g
```
WPA.names <- read.table("WPAall.names", header=T)
```



## Population structure

PCAdapt vignette:

```
https://cran.r-project.org/web/packages/pcadapt/vignettes/pcadapt.html
```
### Code that works (08/08/2018)
```
library(pcadapt)
setwd("C:/Users/Vilhelmiina/Desktop/WPA") 
path_to_file <- "C:/Users/Vilhelmiina/Desktop/WPA"
vcf <- read.pcadapt("WPAall.s3.recode.vcf", type="vcf")
X <- pcadapt(input = vcf, K=12)
plot(X, option = "screeplot") # plots a scree plot
```
 ```
WPA.names <- read.table("WPAall.names", header=T)
plot(X, option = "scores") 
```
plots a score plot but without assigning colors/IDs to any of the clusters, which is no good
```
plot(X, option = "scores", pop =) 
```
  would add IDs to the clusters but using WPA.names for pop is giving an error, "Aesthetics must be either length 1 or the same as the data (69): colour, x, y"
```
plot(X, option = "scores", pop = WPA.names$species) 
plot(X, option = "qqplot") # gives a Q-Q plot showing how well the observed p-values conform to the expected
hist(X$pvalues, xlab = "p-values", main = NULL, breaks = 50, col = "purple") #creates a histogram of the p-values of X
X <- pcadapt(input = vcf, K=5) #changes K to the optimum of the data set (in this case, 5)
```
```
library(adegenet)
library(hierfstat)
library(reshape)

SEall.132 <- read.structure("WPA_structure.stru") #created a genind object - PAUSE and fill in required data before continuing

pop.factor <- as.factor(WPA.names$species)
SEall.132@pop <- pop.factor #creates additional column in SEall.132 which is the species names
hier.SEall <- genind2hierfstat(SEall.132)
SEall.fst <- pairwise.fst(SEall.132, pop=NULL, res.type=c("dist", "matrix")) #takes a while to execute

m <- as.matrix(SEall.fst)
m2 <- melt(m)[melt(upper.tri(m))$value,]
names(m2)<- c("c1","c2", "distance")

library(gplots)
shadesOfGrey <- colorRampPalette(c("grey100", "grey0"))
Dend <- read.table("WPAall.names", header=T) 
Dend.colours <- as.character(Dend$colour) #pulls in the colours I set as an extra column (thru Excel) into WPAall.names to use for the heatmap later on ((this line may be redundant??))
Dend.col <- read.table("WPAcol.names", header=T) #pulls in the colours for use in the heatmap
Dend.col <- as.vector(Dend.col$colour) #chooses only to colors column of Dend.col and makes it into a vector for use in the heatmap

heatmap.2(as.matrix(SEall.fst), trace="none", RowSideColors=Dend.col, ColSideColors=Dend.col, col=shadesOfGrey, labRow=F, labCol=F, key.ylab=NA, key.xlab=NA, key.title="Fst Colour Key", keysize=0.9, main="Pairwise FST 69 individuals, 2049 loci") #creates a heatmap but without a key to match the colors to species?

Dend.col.species <- read.table("WPAcol.names", header=T) #pulls out just the species for use in the labels of the heatmap
Dend.col.species <- Dend.col.species$species #pulls out just the species for use in the labels of the heatmap
heatmap.2(as.matrix(SEall.fst), trace="none", RowSideColors=Dend.col, ColSideColors=Dend.col, col=shadesOfGrey, labRow=Dend.col.species, labCol=F, key.ylab=NA, key.xlab=NA, key.title="Fst Colour Key", keysize=0.9, main="Pairwise FST 69 individuals, 2049 loci") #creates a heatmap with a key, which is being cut off??
```

