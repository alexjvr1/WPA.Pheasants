# Polyplectron inbreeding

Aim: Estimate individual inbreeding for Polyplectron samples based on RAD data


Step1: Look at fastq data


Step2: Filter the data in linux


Step3: Summary stats in R




## Step1: Look at fastq data


You can either open these files in a textpad (open with -> TextEdit), or look at them using command line. Mac use a type of linux, so the following code should let you see the start of a file (replace filename with the file you want to look at - e.g. PHE079.R1.fastq)
```
#This command shows you the first few lines of a document
head filename 

#This opens a page at a time. You can scroll through using spacebar, and quit this view with q
less filename
```

See if you can work out how good the first two sequences are. Is the base quality the same between the forward and reverse read? Is it uniform across the sequence? 


## Step2: Filter the data in linux

Now we skipped ahead to the vcf output file that I generated using a de novo variant caller. I used iPyRAD, which in addition to the vcf file, generates a bunch of files that will be useful later on. 

Have a look at some of them using the same commands as above. Can you work out what the .loci file contains? Why are there two lines per individual? 

You can find some clues about the output files I've sent you from the (iPyRAD documentation)[https://ipyrad.readthedocs.io/output_formats.html]. You don't need to know all of these, but it's a useful resource. 

Scroll to the bottom of the first block of text and look at the last line. This should be something like (if you make the page wide enough, you should see individual name, space, sequence. The last line has some different characters): 

```
less WPAallMerged.loci

PHE153_0 GAATGGTTTGCACGCTCACCTGGGCTGCAAGCATTGTTTGGGGTTGGTTTGGTGCTTTATTACTACTTTCTCAGGTACACTTCCCATTGGGCTTCCTAATAAAGCTGCCATTATAAAGAGAACAAATGCACTGACAGAGG
PHE153_1              GAATGGTTTGCACGCTCACCTGGGCTGCAAGCATTGTTTGGGGTTGGTTTGGTGCTTTATTACTACTTTCTCAGGTACACTTCCCATTGGGCTTCCTAATAAAGCTGCCATTATAAAGAGAACAAATGCACTGACAGAGG
PHE154_0              GAATGGTTTGCACGCTCACCTGGGCTGCAAGCATTGTTTGGGGTTGGTTTGGTGCTTTATTACTACTTTCTCAGGTACACTTCCCATTGGGCTTCCTAATAAAGCTGCCATTATAAAGAGAACAAATGCACTGACAGAGG
PHE154_1              GAATGGTTTGCACGCTCACCTGGGCTGCAAGCATTGTTTGGGGTTGGTTTGGTGCTTTATTACTACTTTCTCAGGTACACTTCCCATTGGGCTTCCTAATAAAGCTGCCATTATAAAGAGAACAAATGCACTGACAGAGG
PHE158_0              GAATGGTTTGCACGCTCACCTGGGCTGCAAGCATTGTTTGGGGTTGGTTTGGTGCTTTATTACTACTTTCTCAGGTACACTTCCCACTGGGCTTCCTAATAAAGCTGCCATTATAAAGAGAACAAATGCACTGACAGAGG
PHE158_1              GAATGGTTTGCACGCTCACCTGGGCTGCAAGCATTGTTTGGGGTTGGTTTGGTGCTTTATTACTACTTTCTCAGGTACACTTCCCACTGGGCTTCCTAATAAAGCTGCCATTATAAAGAGAACAAATGCACTGACAGAGG
//                                                                                                          *                                                     |0|


```



## Step3: Summary stats in R

```


```
