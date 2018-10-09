# Polyplectron inbreeding

Aim: Estimate individual inbreeding for Polyplectron samples based on RAD data


Step1: Look at fastq data


Step2: Filter the data in linux


Step3: Summary stats in R



The directions I outline below assume that you'll be working in the mac command line or linux environment. You can access a command line window either through the (Applications folder)[https://blog.teamtreehouse.com/introduction-to-the-mac-os-x-command-line] or using the Desktop search (control-spacebar) and typing in "Terminal". I'd suggest anchoring this to your taskbar. 

## Step1: Look at fastq data


You can either open these files in a textpad (open with -> TextEdit), or look at them using command line. Mac use a type of linux, so the following code should let you see the start of a file (replace filename with the file you want to look at - e.g. PHE079.R1.fastq)
```
#Usually the files are gzipped to save space. You can unzip them with the following command: 

gunzip filename.gz

#The following commands assume that you're unzipped the files. 
#This command shows you the first few lines of a document
head filename 

#This opens a page at a time. You can scroll through using spacebar, and quit this view with q
less filename

#shows the last few lines of a file
tail filename
```

See if you can work out how good the first two sequences are. Is the base quality the same between the forward and reverse read? Is it uniform across the sequence? 


## Step2: Filter the data in linux

Now we skipped ahead to the vcf output file that I generated using a de novo variant caller. I used iPyRAD, which in addition to the vcf file, generates a bunch of files that will be useful later on. 

Have a look at some of them using the same commands as above. Can you work out what the .loci file contains? Why are there two lines per individual? 

You can find some clues about the output files I've sent you from the [iPyRAD documentation](https://ipyrad.readthedocs.io/output_formats.html). You don't need to know all of these, but it's a useful resource. 

Scroll to the bottom of the first block of text and look at the last line. This should be something like (if you make the page wide enough, you should see individual name, space, sequence. The last line has some different characters): 

```
less WPAallMerged.loci

#use spacebar to scroll through. 
....
PHE153_0             GAATGGTTTGCACGCTCACCTGGGCTGCAAGCATTGTTTGGGGTTGGTTTGGTGCTTTATTACTACTTTCTCAGGTACACTTCCCATTGGGCTTCCTAATAAAGCTGCCATTATAAAGAGAACAAATGCACTGACAGAGG
PHE153_1              GAATGGTTTGCACGCTCACCTGGGCTGCAAGCATTGTTTGGGGTTGGTTTGGTGCTTTATTACTACTTTCTCAGGTACACTTCCCATTGGGCTTCCTAATAAAGCTGCCATTATAAAGAGAACAAATGCACTGACAGAGG
PHE154_0              GAATGGTTTGCACGCTCACCTGGGCTGCAAGCATTGTTTGGGGTTGGTTTGGTGCTTTATTACTACTTTCTCAGGTACACTTCCCATTGGGCTTCCTAATAAAGCTGCCATTATAAAGAGAACAAATGCACTGACAGAGG
PHE154_1              GAATGGTTTGCACGCTCACCTGGGCTGCAAGCATTGTTTGGGGTTGGTTTGGTGCTTTATTACTACTTTCTCAGGTACACTTCCCATTGGGCTTCCTAATAAAGCTGCCATTATAAAGAGAACAAATGCACTGACAGAGG
PHE158_0              GAATGGTTTGCACGCTCACCTGGGCTGCAAGCATTGTTTGGGGTTGGTTTGGTGCTTTATTACTACTTTCTCAGGTACACTTCCCACTGGGCTTCCTAATAAAGCTGCCATTATAAAGAGAACAAATGCACTGACAGAGG
PHE158_1              GAATGGTTTGCACGCTCACCTGGGCTGCAAGCATTGTTTGGGGTTGGTTTGGTGCTTTATTACTACTTTCTCAGGTACACTTCCCACTGGGCTTCCTAATAAAGCTGCCATTATAAAGAGAACAAATGCACTGACAGAGG
//                                                                                                          *                                                     |0|


```

For the moment we're interested in the vcf file. This contains all of the possible loci that were called by ipyRAD based on the number of sequences found within and between individuals, and the similarity between sequences. We know that these aren't all real - some will be spurious because of errors during the PCR or during the sequencing process. Some are uninformative because they occur in really low frequencies.

And then we have to deal with missing data. It's impossible to get all of the loci sequenced for all individuals, so we need to decide which loci or individuals to filter out based on our questions. 

When we look at inbreeding, we'll want a pretty complete dataset with as many loci as possible. We also want info on all the individuals if possible (although if an individual has much less data than the rest, we will drop it for the time being). 

We'll use (vcftools)[http://vcftools.sourceforge.net/man_latest.html] for this part.

Have a look at the syntax of the commands I use here. You can also use the manual to see if there are other commands that might be interesting later on. 
```
#This command will let you look at the data. How many individuals are there? How many loci? 

vcftools --vcf filename

```

The file is currently really big and a bit unwieldy. We can get rid of a lot of the data that we know are not useful: 
```
vcftools --vcf filename --maf 0.05 --max-missing 0.5 --minQ 30 --recode --recode-INFO-all --out filename.s1

```
The flags are all in the vcftools manual. Can you work out what we've filtered from the dataset? 

Hint: look up --maf, --max-missing, and --minQ

The --recode and --recode-INFO-all flags are just to keep the formatting correct. 

--out allows you to specify the name of the output file. You can call it anything you like. I've called it file.s1 because this is the first filtering step. 


How much data is left after this? Note that the input file here is the output file from before. Although I've specified the name as filename.s1, vcftools adds the .recode.vcf extension. Remember to add the full name in your command. 
```
vcftools --vcf filename.s1.recode.vcf

```


Now we can see how much data we have per individual. First we'll use vcftools to tell us how much data is missing per individual, and then we'll plot it in the command line: 
```
vcftools --vcf filename.s1.recode.vcf --missing-indv

```

This creates an output called out.imiss. We can look at this like before: 
```
#this will write the whole file to your screen. You can scroll up and down using the scroll bar or up and down arrows. 
cat out.imiss
less out.imiss
head out.imiss
tail out.imiss
```

You can look up what the headers mean in the vcftools manual. What we're interested in is the F_MISS column. This tells us the proportion of missing data for that individual. 

You can just copy and paste this next bit in your command line. This is a bit of linux code to draw of histogram of the F_MISS column: 
```
mawk '!/IN/' out.imiss | cut -f5 > totalmissing
gnuplot << \EOF 
set terminal dumb size 120, 30
set autoscale 
unset label
set title "Histogram of % missing data per individual"
set ylabel "Number of Occurrences"
set xlabel "% of missing data"
#set yr [0:100000]
binwidth=0.01
bin(x,width)=width*floor(x/width) + binwidth/2.0
plot 'totalmissing' using (bin($1,binwidth)):(1.0) smooth freq with boxes
pause -1
EOF
```

What does this mean? 
Do we have some individuals with a lot of missing data? 


Now we need to decide whether some of these individuals should be removed. When we look at population structure (e.g with the PCA), we want to reduce the amount of missing data in our dataset. We'll reconsider this decision when we're looking at per individual inbreeding, because then we'll want most of the individuals represented. For now we can identify individuals with a lot of missing data and remove them. 
Decide on a cutoff based on your results from above. You can now use vcftools to identify these individuals by name: 
```
#This is a bit of linux code that first finds all the lines where F_MISS > 0.5. It then copies the names of these lines (i.e. the first column) to an output file called lowDP.indv

mawk '$5 > 0.5' out.imiss | cut -f1 > lowDP.indv

#we can look at this like before
cat lowDP.indv

#and we can count the number of indivs we'll lose
cat lowDP.indv |wc -l
```

Once you're happy with this cutoff, we can remove the individuals from the vcf file
```
vcftools --vcf filename.s1.recode.vcf --remove lowDP.indv --recode --recode-INFO-all --out filename.final

#and see what your data look like now
vcftools --vcf filename.final.recode.vcf
```

Congrats! You've just filtered your first RAD data set! 


## Step3: Summary stats in R

The next step is to look at the data in R. We want to calculate some summary statistics and we want to look at the genetic variance in the data. 

We'll use (Adegenet)[http://adegenet.r-forge.r-project.org/files/tutorial-basics.pdf] for basic statistics and (pcadapt)[https://cran.r-project.org/web/packages/pcadapt/vignettes/pcadapt.html] to determine and visualise genetic variance in the data. 

```


```
