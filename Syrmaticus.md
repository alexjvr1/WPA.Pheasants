# Phylogeny of captive and wild Syrmaticus based on mtDNA sequences




1. Sequence alignment in R

2. Haplotype network

3. Calculate diversity statistics (Haplotype diversity, nucleotide diversity)

4. Phylogeny
  
  4.1. Bayesian (MrBayes?)
  
  4.2. Maximum likelihood (RAxML?)
  
5. Demographic history (Mismatch distribution). 

6. Divergence between datasets (wild vs captive & between species) using PCA. 


# Intro to R

The first thing you want to do is prepare your dataset for analyses. Currently you've downloaded all available Syrmaticus cytb and control region data from NCBI.

My code is for command line R, so there are some small differences when you're using Rstudio. 
When my code states install.packages() or library(), you should use the dropdown arrow in Rstudio 
to install or load the libraries you need.

You want to make sure that Rstudio is pointing to the correct directory. Make a folder on your computer where you'll keep all the Syrmaticus 
related data and files. Set the working directory in Rstudio as this folder. You can check where Rstudio is pointing with this command: 

```
getwd()

```

You can also set the path using command line (i.s.o the dropdown arrows in Rstudio). 
```
setwd("path/to/directory")  
```
where path/to/file is the path to your Syrmaticus directory. 


## 1. Multiple sequence alignment. 

Try to import your fasta data into R and create a sequence alignment using MSA. The following pdf will guide you through the process. 
Make sure you know what each command does.

https://bioconductor.org/packages/release/bioc/vignettes/msa/inst/doc/msa.pdf






