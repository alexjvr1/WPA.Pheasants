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

relatedness <- within(relatedness, WPA1[INDV1 == 'PHE079'] <- '1836/Ring2221K.21306.AV.Leg') 
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE088'] <- '663K.1005/06')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE089'] <- '2136/904K/07')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE090'] <- 'SB081322K2136/65031')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE091'] <- '13K/1005/11')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE092'] <- '2011/65/HJT')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE116'] <- '1743/Band.18543K.2136.AV.04.10.Left.leg')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE135'] <- 'B101625')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE136'] <- 'B101626')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE137'] <- '12B10')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE138'] <- '55B10')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE141'] <- 'AO.78-08-47.NL10')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE142'] <- 'AO2171-15-19.NL')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE143'] <- 'B10.0029-08-9')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE144'] <- '10.012.1085')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE149'] <- 'AO.0220.15.3.B10')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE150'] <- 'AO.4405.04.2.B10')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE153'] <- '65/HTJ/2011')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE154'] <- 'B12/16/138')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE155'] <- 'B10/16/47')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE158'] <- 'APHOLLAND/FA26')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE159'] <- 'APHOLLAND/FA27')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE161'] <- 'TH.RP112')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE162'] <- 'TH.RP113')

#Do the same for P.malacense
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE080'] <- '296L') 
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE081'] <- '14K.1005-11')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE082'] <- '2137/233K.AVIO')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE083'] <- '2137/2334K.AVIO')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE084'] <- 'AO.18/12/78')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE085'] <- 'AO.27/12/78')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE086'] <- '101LAV11')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE087'] <- '1600.K2136-02')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE100'] <- 'MPP')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE112'] <- '')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE126'] <- '11NL10PP-78.16')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE127'] <- '22NL10AO-78.16')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE133'] <- 'B1016120')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE134'] <- '53B1216')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE139'] <- 'AO.78-05-1-NL10')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE140'] <- 'AO.5875-05-15-NL10')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE145'] <- 'EE.D10.14.nk.268')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE146'] <- 'AO.144.08.16')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE147'] <- 'AO.4405.04.16.B10')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE148'] <- 'BA.335.EE.D10.15')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE156'] <- 'AO-18-12-78')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE157'] <- 'B10/WD/13/83')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE160'] <- '')


##Grey PP
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE105'] <- 'CDF2012-1')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE106'] <- 'Surrey')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE107'] <- 'Crisp')


##Bronze-tailed PP
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE113'] <- 'TH.MP104')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE151'] <- '4405/15/51')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE152'] <- '14ZB10')



##Bornean PP
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE101'] <- 'BPP1')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE102'] <- 'BPP1')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE103'] <- 'BPP1')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE104'] <- 'BPP2')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE111'] <- '')



##Germains PP
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE093'] <- '113K.AV')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE109'] <- '42K2532/MKAITT')


##Palawan PP
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE094'] <- '0440B4405')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE095'] <- '855K1005.AV10')
relatedness <- within(relatedness, WPA1[INDV1 == 'PHE108'] <- 'Blue.ring')




##########################

relatedness <- within(relatedness, WPA2[INDV2 == 'PHE079'] <- '1836/Ring2221K.21306.AV.Leg') 
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE088'] <- '663K.1005/06')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE089'] <- '2136/904K/07')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE090'] <- 'SB081322K2136/65031')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE091'] <- '13K/1005/11')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE092'] <- '2011/65/HJT')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE116'] <- '1743/Band.18543K.2136.AV.04.10.Left.leg')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE135'] <- 'B101625')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE136'] <- 'B101626')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE137'] <- '12B10')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE138'] <- '55B10')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE141'] <- 'AO.78-08-47.NL10')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE142'] <- 'AO2171-15-19.NL')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE143'] <- 'B10.0029-08-9')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE144'] <- '10.012.1085')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE149'] <- 'AO.0220.15.3.B10')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE150'] <- 'AO.4405.04.2.B10')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE153'] <- '65/HTJ/2011')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE154'] <- 'B12/16/138')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE155'] <- 'B10/16/47')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE158'] <- 'APHOLLAND/FA26')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE159'] <- 'APHOLLAND/FA27')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE161'] <- 'TH.RP112')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE162'] <- 'TH.RP113')

#Do the same for P.malacense
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE080'] <- '296L') 
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE081'] <- '14K.1005-11')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE082'] <- '2137/233K.AVIO')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE083'] <- '2137/2334K.AVIO')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE084'] <- 'AO.18/12/78')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE085'] <- 'AO.27/12/78')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE086'] <- '101LAV11')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE087'] <- '1600.K2136-02')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE100'] <- 'MPP')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE112'] <- '')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE126'] <- '11NL10PP-78.16')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE127'] <- '22NL10AO-78.16')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE133'] <- 'B1016120')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE134'] <- '53B1216')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE139'] <- 'AO.78-05-1-NL10')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE140'] <- 'AO.5875-05-15-NL10')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE145'] <- 'EE.D10.14.nk.268')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE146'] <- 'AO.144.08.16')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE147'] <- 'AO.4405.04.16.B10')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE148'] <- 'BA.335.EE.D10.15')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE156'] <- 'AO-18-12-78')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE157'] <- 'B10/WD/13/83')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE160'] <- '')


##Grey PP
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE105'] <- 'CDF2012-1')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE106'] <- 'Surrey')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE107'] <- 'Crisp')


##Bronze-tailed PP
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE113'] <- 'TH.MP104')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE151'] <- '4405/15/51')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE152'] <- '14ZB10')



##Bornean PP
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE101'] <- 'BPP1')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE102'] <- 'BPP1')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE103'] <- 'BPP1')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE104'] <- 'BPP2')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE111'] <- '')



##Germains PP
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE093'] <- '113K.AV')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE109'] <- '42K2532/MKAITT')


##Palawan PP
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE094'] <- '0440B4405')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE095'] <- '855K1005.AV10')
relatedness <- within(relatedness, WPA2[INDV2 == 'PHE108'] <- 'Blue.ring')







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
