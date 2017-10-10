# Pheasant Genomics Project

## Aims: 

Firstly to improve the SNP calling for the dataset by testing pyRAD. 

1. Mountain and Malaysian Peacock Pheasants 

- Identify pure and hybrid individuals
  
2. Bornean Peacock Pheasants

  - Identify hybrids

  - Relatedness
  

## Data

Paired-End ddRAD data sequenced on MiSeq. 

Restriction enzymes: 

SbfI (8bp recognition sites) TGCAGG-3'
 
SphI (7bp recognition site) CATGC-3'

39 individuals in total (40 samples). 

4 Species. 


I first need to split the file into fwd and reverse reads. 
Then I need to check for Illumina adapters
Then I need to reverse complement read2 and concatenate to the first read. (I think this has already been done)

Since I have paired-end reads I can also look for PCR duplicates which I couldn't do with any of my other data so far. 

  - Interesting test: How much PCR duplication in this dataset? 
  - Can we compare between species/data quality/Illumina lanes. 

The data we got wasn't raw data. They are single .fq files per individual, with _1 and _2 reads (not interleaved). I think the reverse reads have already been reverse complemented here, but I'm not sure. 

```
Info from Gill Murray-Dickson about the process-radtags script: 

process_radtags -E phred33 --filter_illumina -s 20 -i fastq -y fastq -1 XXXX_R1_001.fastq -2 XXXX_R2_001.fastq -o samples1/ -e sbfI --renz_2 sphI -t 135 -c -q --inline_inline -b XXX_barcodes.txt

I've figured out that: 

Reverse reads have been reverse complemented

Restriction enzyme sites are still included in R1 and R2 sequences

the -c option above cleans the data by removing any reads with uncalled bases. This is potentially a problem - I think it will remove
too much of the data!
```

Side note: All 8 query individuals have the lines annotated /1 and /2 for fwd and rev. (PHE126, 127, 133-138). 

First I'll need to split the file into fwd and reverse reads. The data from Gill contains the concatenated data. It looks like it's R1 + R2 + R1.rem + R2.rem. (where .rem are the non paired reads). 
After looking at the file further, I realised that they've run the file through one of the Stacks pre-processing pipelines, so all the reads are in one folder. Also, after splitting the reads (see below) I found now .rem sequences, so this has already been cleaned up. Is this done by stacks??


This is to split the data into 4 files: 

>pwd
/Users/alexjvr/2016RADAnalysis/WPA/rawdata.WPA/others

1. first run through all the files and split all reads with _1 or _2 into different files
```
#-A takes 3 lines after first line, and -B takes lines before. 

for f in $(ls *fq); do grep '_1$' -A3 $f > $f._1; done 
for f in $(ls *fq); do grep '_2$' -A3 $f > $f._2; done
```

And split the query individuals (see side note above): 

Use sed to replace /1 and /2 with _1 and _2. Cool trick: sed can read any symbol as the separator in the command. So replacing / command doesn't become unreadable. The / has to be escaped with \

I can condition sed so that it only substitutes on the lines starting with "^@1_" because / is a PHRED symbol i.e. appears in the quality scores. 

```
cat PHE126.fq | sed '/^@1_/s/\/1$/_1/' > PHE126.fq.2

for f in $(ls PHE13*); do cat $f | sed '/^@1_/s/\/1$/_1/' > $f.2; done
for f in $(ls PHE12*); do cat $f | sed '/^@1_/s/\/1$/_1/' > $f.2; done

## and replace /2 in these 
for f in $(ls *.2); do cat $f | sed '/^@1_/s/\/2$/_2/' > $f.3; done


###split files as above


rename
for i in *.2.3._1; do mv "$i" "${i/.fq.2.3._1/_1.fq}" ; done
for i in *.2.3._2; do mv "$i" "${i/.fq.2.3._2/_2.fq}" ; done
```

The next steps are all manipulating the files so that I can compare the fwd and rev reads and identify any .rem reads

2. rename these files
```
for i in *.fq._1; do mv "$i" "${i/.fq._1/_R1.fq}" ; done
for i in *.fq._2; do mv "$i" "${i/.fq._2/_R2.fq}" ; done
```


3. get all the names for these sequences into .names files

```
for f in $(ls *_1.fq); do grep "_1$" $f > $f.names; done
for f in $(ls *_2.fq); do grep "_2$" $f > $f.names; done

```


4. substitute the _1 and _2 so that the names are exactly the same.
```
for f in $(ls *_1.fq.names); do cat $f | sed 's/_1$/e/' > $f.1; done
for f in $(ls *_2.fq.names); do cat $f | sed 's/_2$/e/' > $f.1; done
```

5. Compare the files

The number of lines in fwd and rev for one individual was the same for my spot check of ~5 indivs. This makes me suspicious that there are no .rem sequences
```
cat PHE127_1.fq.names.1 | wc -l
403219

cat PHE127_2.fq.names.1 | wc -l
403219
```

directly compare the files
```
diff PHE138_1.fq.names.1 PHE138_2.fq.names.1
```

I found no .rem files. 

Now I have 2 files per individual: fwd and reverse. 





### Check for merged reads

Deren Eaton recommends PEAR https://sco.h-its.org/exelixis/web/software/pear/

http://nbviewer.jupyter.org/gist/dereneaton/dc6241083c912519064e/tutorial_pairddRAD_3.0.4-merged.ipynb

pwd
/Users/alexjvr/2016RADAnalysis/WPA/rawdata.WPA/others
```
brew install pear
```

And run PEAR on all the files

```
%%bash
## then run PEAR on each data file
for gfile in *_R1.fq; 
  do pear -f $gfile \
          -r ${gfile/_R1.fq/_R2.fq} \
          -o ${gfile/_R1.fq/} \
          -n 33 \
          -j 4  >> pear.log 2>&1
done
```


This won't run. I'm getting an error about illegal characters.. 







#########################################
####### Method that didn't work  ########
1. Check at which line the reverse reads start (this is for all reference individuals). 
```
for i in *.fq
do grep -nr '_2$' $i > $i.lines
done

head -n 1 *lines

for i in *lines; do cat $i | cut -f1 -d":" > $i.linenr; done

##to rename multiple files in bash
for i in *.lines.firstline..linenr ; do mv "$i" "${i/lines.firstline..linenr/linenr}"; done 

###I did this as an intermediate step before
for i in *lines ; do cat $i | awk '{print $1;exit}' > $i.firstline; done
rm *lines
```

2. Split the data into two files according to the first line that we just calculated. I had to do this manually. This produces a .R1.fq file, ready for use, and a R2.fq file which still contains the .rem sequences and will be split further after this step. 
```
cat PHE116.fq.linenr
480705

awk 'NR < 480705 { print >> "PHE116.R1.fq"; next } {print >> "PHE116.merged.fq" }' PHE116.fq
```

3. Check at which line the R1.rem reads start. (For most of these, because they're paired, the line numbers should be the same as found in Step 1)
```
for i in *merged.fq
do grep -nr '_1$' $i > $i.lines
done

head -n 1 *merged.fq.lines

for i in *merged.lines; do cat $i | cut -f1 -d":" > $i.linenr; done
```

4. Split the data into R1.rem and R2. This has to be done manually
```
cat PHE116.fq.R2.linenr
480705

awk 'NR < 480705 { print >> "PHE116.R2.fq"; next } {print >> "PHE116.merged.2.fq" }' PHE116.fq
```

5. Repeat the process to split R1.rem into R1.rem and R2.rem

```
for i in *merged.2.fq
do grep -nr '_2$' $i > $i.lines
done

head -n 1 *merged.2.fq.lines

for i in *merged.2.fq.lines; do cat $i | cut -f1 -d":" > $i.linenr; done
```

And split the data
```
cat PHE116.fq.merged.2.linenr
480705

awk 'NR < 480705 { print >> "PHE116.R1.rem.fq"; next } {print >> "PHE116.R2.rem.fq" }' PHE116.merged.2.fq
```
#############################################
#############################################




Reverse complement the Reverse files so that the invariant RE overhang is at the end of the sequence
```
/usr/local/ngseq/bin/fastx_reverse_complement -z -v -i[input] -o[output]

for i in *R2.fq; do /usr/local/ngseq/bin/fastx_reverse_complement -v -i $i -o $i.rev.fq; done
```

Deren Eaton suggests that sequences should be merged if they are overlapping, otherwise it will cause problems with the de novo assembly. 
I'm checking this as suggested in his tutorial: 

http://nbviewer.jupyter.org/gist/dereneaton/dc6241083c912519064e/tutorial_pairddRAD_3.0.4-merged.ipynb

First I need to install PEAR on my laptop (in Applications)

```
1. git clone https://github.com/xflouris/PEAR.git
2. cd PEAR
3. ./autogen.sh
4. ./configure
5. make
6. sudo make install

```




##pyRAD

I'll test pyrad clustering using individuals from all the 4 species of Pheasant in the test runs. I'm using the samples specified as
reference individuals in the first report. 

I'll test clustering thresholds of 85-95% to start with. 


Chosen 7 individuals. I've tried to use reference birds with DNA taken from muscle/tissue where possible: 

Mountain: PHE088 & PHE089

Malaysian: PH086 & PHE087

Grey: PHE105 & PHE106

Bronze-tailed: PHE113


Running this on the FGCZ server. 

This is PE ddRAD, so I have to adjust the way I run pyRAD. 

I have to reverse complement the second read and concatenate it to the first read. So sequences are now longer per locus (~250bp)





