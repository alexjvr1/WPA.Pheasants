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

Add columns with species info and WPA sample numbers
```
relatedness <- within(relatedness, species1[INDV1 == 'PHE079'] <- 'P.inopinatum') 
relatedness <- within(relatedness, species1[INDV1 == 'PHE088'] <- 'P.inopinatum')
relatedness <- within(relatedness, species1[INDV1 == 'PHE089'] <- 'P.inopinatum')
relatedness <- within(relatedness, species1[INDV1 == 'PHE090'] <- 'P.inopinatum')
relatedness <- within(relatedness, species1[INDV1 == 'PHE091'] <- 'P.inopinatum')
relatedness <- within(relatedness, species1[INDV1 == 'PHE092'] <- 'P.inopinatum')
relatedness <- within(relatedness, species1[INDV1 == 'PHE116'] <- 'P.inopinatum')
relatedness <- within(relatedness, species1[INDV1 == 'PHE135'] <- 'P.inopinatum')
relatedness <- within(relatedness, species1[INDV1 == 'PHE136'] <- 'P.inopinatum')
relatedness <- within(relatedness, species1[INDV1 == 'PHE137'] <- 'P.inopinatum')
relatedness <- within(relatedness, species1[INDV1 == 'PHE138'] <- 'P.inopinatum')
relatedness <- within(relatedness, species1[INDV1 == 'PHE141'] <- 'P.inopinatum')
relatedness <- within(relatedness, species1[INDV1 == 'PHE142'] <- 'P.inopinatum')
relatedness <- within(relatedness, species1[INDV1 == 'PHE143'] <- 'P.inopinatum')
relatedness <- within(relatedness, species1[INDV1 == 'PHE144'] <- 'P.inopinatum')
relatedness <- within(relatedness, species1[INDV1 == 'PHE149'] <- 'P.inopinatum')
relatedness <- within(relatedness, species1[INDV1 == 'PHE150'] <- 'P.inopinatum')
relatedness <- within(relatedness, species1[INDV1 == 'PHE153'] <- 'P.inopinatum')
relatedness <- within(relatedness, species1[INDV1 == 'PHE154'] <- 'P.inopinatum')
relatedness <- within(relatedness, species1[INDV1 == 'PHE155'] <- 'P.inopinatum')
relatedness <- within(relatedness, species1[INDV1 == 'PHE158'] <- 'P.inopinatum')
relatedness <- within(relatedness, species1[INDV1 == 'PHE159'] <- 'P.inopinatum')
relatedness <- within(relatedness, species1[INDV1 == 'PHE161'] <- 'P.inopinatum')
relatedness <- within(relatedness, species1[INDV1 == 'PHE162'] <- 'P.inopinatum')

#Do the same for P.malacense
relatedness <- within(relatedness, species1[INDV1 == 'PHE080'] <- 'P.malacense') 
relatedness <- within(relatedness, species1[INDV1 == 'PHE081'] <- 'P.malacense')
relatedness <- within(relatedness, species1[INDV1 == 'PHE082'] <- 'P.malacense')
relatedness <- within(relatedness, species1[INDV1 == 'PHE083'] <- 'P.malacense')
relatedness <- within(relatedness, species1[INDV1 == 'PHE084'] <- 'P.malacense')
relatedness <- within(relatedness, species1[INDV1 == 'PHE085'] <- 'P.malacense')
relatedness <- within(relatedness, species1[INDV1 == 'PHE086'] <- 'P.malacense')
relatedness <- within(relatedness, species1[INDV1 == 'PHE087'] <- 'P.malacense')
relatedness <- within(relatedness, species1[INDV1 == 'PHE100'] <- 'P.malacense')
relatedness <- within(relatedness, species1[INDV1 == 'PHE112'] <- 'P.malacense')
relatedness <- within(relatedness, species1[INDV1 == 'PHE126'] <- 'P.malacense')
relatedness <- within(relatedness, species1[INDV1 == 'PHE127'] <- 'P.malacense')
relatedness <- within(relatedness, species1[INDV1 == 'PHE133'] <- 'P.malacense')
relatedness <- within(relatedness, species1[INDV1 == 'PHE134'] <- 'P.malacense')
relatedness <- within(relatedness, species1[INDV1 == 'PHE139'] <- 'P.malacense')
relatedness <- within(relatedness, species1[INDV1 == 'PHE140'] <- 'P.malacense')
relatedness <- within(relatedness, species1[INDV1 == 'PHE145'] <- 'P.malacense')
relatedness <- within(relatedness, species1[INDV1 == 'PHE146'] <- 'P.malacense')
relatedness <- within(relatedness, species1[INDV1 == 'PHE147'] <- 'P.malacense')
relatedness <- within(relatedness, species1[INDV1 == 'PHE148'] <- 'P.malacense')
relatedness <- within(relatedness, species1[INDV1 == 'PHE156'] <- 'P.malacense')
relatedness <- within(relatedness, species1[INDV1 == 'PHE157'] <- 'P.malacense')
relatedness <- within(relatedness, species1[INDV1 == 'PHE160'] <- 'P.malacense')

#Then for species2 column for Mountain
relatedness <- within(relatedness, species2[INDV2 == 'PHE079'] <- 'P.inopinatum') 
relatedness <- within(relatedness, species2[INDV2 == 'PHE088'] <- 'P.inopinatum')
relatedness <- within(relatedness, species2[INDV2 == 'PHE089'] <- 'P.inopinatum')
relatedness <- within(relatedness, species2[INDV2 == 'PHE090'] <- 'P.inopinatum')
relatedness <- within(relatedness, species2[INDV2 == 'PHE091'] <- 'P.inopinatum')
relatedness <- within(relatedness, species2[INDV2 == 'PHE092'] <- 'P.inopinatum')
relatedness <- within(relatedness, species2[INDV2 == 'PHE116'] <- 'P.inopinatum')
relatedness <- within(relatedness, species2[INDV2 == 'PHE135'] <- 'P.inopinatum')
relatedness <- within(relatedness, species2[INDV2 == 'PHE136'] <- 'P.inopinatum')
relatedness <- within(relatedness, species2[INDV2 == 'PHE137'] <- 'P.inopinatum')
relatedness <- within(relatedness, species2[INDV2 == 'PHE138'] <- 'P.inopinatum')
relatedness <- within(relatedness, species2[INDV2 == 'PHE141'] <- 'P.inopinatum')
relatedness <- within(relatedness, species2[INDV2 == 'PHE142'] <- 'P.inopinatum')
relatedness <- within(relatedness, species2[INDV2 == 'PHE143'] <- 'P.inopinatum')
relatedness <- within(relatedness, species2[INDV2 == 'PHE144'] <- 'P.inopinatum')
relatedness <- within(relatedness, species2[INDV2 == 'PHE149'] <- 'P.inopinatum')
relatedness <- within(relatedness, species2[INDV2 == 'PHE150'] <- 'P.inopinatum')
relatedness <- within(relatedness, species2[INDV2 == 'PHE153'] <- 'P.inopinatum')
relatedness <- within(relatedness, species2[INDV2 == 'PHE154'] <- 'P.inopinatum')
relatedness <- within(relatedness, species2[INDV2 == 'PHE155'] <- 'P.inopinatum')
relatedness <- within(relatedness, species2[INDV2 == 'PHE158'] <- 'P.inopinatum')
relatedness <- within(relatedness, species2[INDV2 == 'PHE159'] <- 'P.inopinatum')
relatedness <- within(relatedness, species2[INDV2 == 'PHE161'] <- 'P.inopinatum')
relatedness <- within(relatedness, species2[INDV2 == 'PHE162'] <- 'P.inopinatum')

#And the same for Malaysian
relatedness <- within(relatedness, species2[INDV2 == 'PHE080'] <- 'P.malacense') 
relatedness <- within(relatedness, species2[INDV2 == 'PHE081'] <- 'P.malacense')
relatedness <- within(relatedness, species2[INDV2 == 'PHE082'] <- 'P.malacense')
relatedness <- within(relatedness, species2[INDV2 == 'PHE083'] <- 'P.malacense')
relatedness <- within(relatedness, species2[INDV2 == 'PHE084'] <- 'P.malacense')
relatedness <- within(relatedness, species2[INDV2 == 'PHE085'] <- 'P.malacense')
relatedness <- within(relatedness, species2[INDV2 == 'PHE086'] <- 'P.malacense')
relatedness <- within(relatedness, species2[INDV2 == 'PHE087'] <- 'P.malacense')
relatedness <- within(relatedness, species2[INDV2 == 'PHE100'] <- 'P.malacense')
relatedness <- within(relatedness, species2[INDV2 == 'PHE112'] <- 'P.malacense')
relatedness <- within(relatedness, species2[INDV2 == 'PHE126'] <- 'P.malacense')
relatedness <- within(relatedness, species2[INDV2 == 'PHE127'] <- 'P.malacense')
relatedness <- within(relatedness, species2[INDV2 == 'PHE133'] <- 'P.malacense')
relatedness <- within(relatedness, species2[INDV2 == 'PHE134'] <- 'P.malacense')
relatedness <- within(relatedness, species2[INDV2 == 'PHE139'] <- 'P.malacense')
relatedness <- within(relatedness, species2[INDV2 == 'PHE140'] <- 'P.malacense')
relatedness <- within(relatedness, species2[INDV2 == 'PHE145'] <- 'P.malacense')
relatedness <- within(relatedness, species2[INDV2 == 'PHE146'] <- 'P.malacense')
relatedness <- within(relatedness, species2[INDV2 == 'PHE147'] <- 'P.malacense')
relatedness <- within(relatedness, species2[INDV2 == 'PHE148'] <- 'P.malacense')
relatedness <- within(relatedness, species2[INDV2 == 'PHE156'] <- 'P.malacense')
relatedness <- within(relatedness, species2[INDV2 == 'PHE157'] <- 'P.malacense')
relatedness <- within(relatedness, species2[INDV2 == 'PHE160'] <- 'P.malacense')


##Grey PP
relatedness <- within(relatedness, species1[INDV1 == 'PHE105'] <- 'P.bicalcaratum')
relatedness <- within(relatedness, species1[INDV1 == 'PHE106'] <- 'P.bicalcaratum')
relatedness <- within(relatedness, species1[INDV1 == 'PHE107'] <- 'P.bicalcaratum')


relatedness <- within(relatedness, species2[INDV2 == 'PHE105'] <- 'P.bicalcaratum')
relatedness <- within(relatedness, species2[INDV2 == 'PHE106'] <- 'P.bicalcaratum')
relatedness <- within(relatedness, species2[INDV2 == 'PHE107'] <- 'P.bicalcaratum')


##Bronze-tailed PP
relatedness <- within(relatedness, species1[INDV1 == 'PHE113'] <- 'P.chalcurum')
relatedness <- within(relatedness, species1[INDV1 == 'PHE151'] <- 'P.chalcurum')
relatedness <- within(relatedness, species1[INDV1 == 'PHE152'] <- 'P.chalcurum')


relatedness <- within(relatedness, species2[INDV2 == 'PHE113'] <- 'P.chalcurum')
relatedness <- within(relatedness, species2[INDV2 == 'PHE151'] <- 'P.chalcurum')
relatedness <- within(relatedness, species2[INDV2 == 'PHE152'] <- 'P.chalcurum')


##Bornean PP
relatedness <- within(relatedness, species1[INDV1 == 'PHE101'] <- 'P.scheiermacheri')
relatedness <- within(relatedness, species1[INDV1 == 'PHE102'] <- 'P.scheiermacheri')
relatedness <- within(relatedness, species1[INDV1 == 'PHE103'] <- 'P.scheiermacheri')
relatedness <- within(relatedness, species1[INDV1 == 'PHE104'] <- 'P.scheiermacheri')
relatedness <- within(relatedness, species1[INDV1 == 'PHE111'] <- 'P.scheiermacheri')


relatedness <- within(relatedness, species2[INDV2 == 'PHE101'] <- 'P.scheiermacheri')
relatedness <- within(relatedness, species2[INDV2 == 'PHE102'] <- 'P.scheiermacheri')
relatedness <- within(relatedness, species2[INDV2 == 'PHE103'] <- 'P.scheiermacheri')
relatedness <- within(relatedness, species2[INDV2 == 'PHE104'] <- 'P.scheiermacheri')
relatedness <- within(relatedness, species2[INDV2 == 'PHE111'] <- 'P.scheiermacheri')


##Germains PP
relatedness <- within(relatedness, species1[INDV1 == 'PHE093'] <- 'P.germaini')
relatedness <- within(relatedness, species1[INDV1 == 'PHE109'] <- 'P.germaini')

relatedness <- within(relatedness, species2[INDV2 == 'PHE093'] <- 'P.germaini')
relatedness <- within(relatedness, species2[INDV2 == 'PHE109'] <- 'P.germaini')



##Palawan PP
relatedness <- within(relatedness, species1[INDV1 == 'PHE094'] <- 'P.napoleonis')
relatedness <- within(relatedness, species1[INDV1 == 'PHE095'] <- 'P.napoleonis')
relatedness <- within(relatedness, species1[INDV1 == 'PHE108'] <- 'P.napoleonis')


relatedness <- within(relatedness, species2[INDV2 == 'PHE094'] <- 'P.napoleonis')
relatedness <- within(relatedness, species2[INDV2 == 'PHE095'] <- 'P.napoleonis')
relatedness <- within(relatedness, species2[INDV2 == 'PHE108'] <- 'P.napoleonis')
```



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
