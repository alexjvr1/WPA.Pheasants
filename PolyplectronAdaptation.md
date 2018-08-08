# Adaptation to environment in pheasants. 

Questions

1. What is the population structure of the peacock pheasants in SE Asia? 

2. Can we resolve the demographic history of peacock pheasants in SE Asia? 

3. Can we identify signals of adaptation to environment in P. malacense and P. impoinatum? 

4. How do these loci fit in with what we know from other species? 


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
setwd("C:/Users/Vilhelmiina/Desktop/WPA") 
path_to_file <- "C:/Users/Vilhelmiina/Desktop/WPA"
vcf <- read.pcadapt("WPAall.s3.recode.vcf", type="vcf")
X <- pcadapt(input = vcf, K=12)
plot(X, option = "screeplot") # plots a scree plot
```
 ```
plot(X, option = "scores") 
```
plots a score plot but without assigning colors/IDs to any of the clusters, which is no good
```
plot(X, option = "scores", pop =) 
```
  would add IDs to the clusters but using WPA.names for pop is giving an error, "Aesthetics must be either length 1 or the same as the data (69): colour, x, y"
```
