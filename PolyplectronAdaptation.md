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



## Population structure

PCAdapt vignette:

```
https://cran.r-project.org/web/packages/pcadapt/vignettes/pcadapt.html
```


