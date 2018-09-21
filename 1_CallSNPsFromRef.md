# Variant calling with reference genome

Pipeline from raw data to SNP calling using a reference genome (Here Gallus gallus) 


#### 1. Demultiplex & trim (& combine duplicated samples)

#### 2. Map to Genome (BWA-mem)

#### 3. Prepare mapped reads for variant calling: 
        
        - add Reading Groups, sort, remove PCR duplicates
        
#### 4. Call variants on individuals (g.vcf), which includes local alignment

#### 5. Join all g.vcf files together (GenomesDBImport)

#### 6. Call variants on joing dataset

#### 7. Filter the variants. 




## Computing access

Analyses are conducted on the [University of Bristol's (UoB) High Performance Computing (HPC) machine](https://www.acrc.bris.ac.uk)

There is an additional facility for data storage here (1Tb total): /projects/Butterflies/alex

Guides about BlueCrystal usage are available [here](https://www.acrc.bris.ac.uk/acrc/resources.htm#howto). 


What software is installed? 
```
module avail 
```

Load needed software (this needs to be done for every new session and in every script) 
```
module load xx
```

If software is unavailable, either install in your home directory, or contact HPC help desk: IT Service Desk <service-desk@bristol.ac.uk>


Jobs are submitted via a queueing system. 

Submit a script
```
qsub script.sh
```

Check on status of run
```
qstat -u aj18951

OR 

showq | grep -B 10 "aj18951"
```



A nice overview of raw data to variants [here](https://informatics.fas.harvard.edu/whole-genome-resquencing-for-population-genomics-fastq-to-vcf.html#variantcalling)



### 1. Demultiplex & trim

Raw reads are received in fastq.gz format from genomics facilities. In this case they are multiple lanes of sequencing from a HiSeq2500, with multiple individuals pooled within each lane. Individuals were indiviually barcoded. In this step we will demultiplex the samples, trim Illumina adapter sequence, and combine data from individuals sequenced in multiple lanes. 

For demultiplexing I usually use process_radtags from the [STACKS](http://catchenlab.life.illinois.edu/stacks/comp/process_radtags.php) package. This is not available on BlueCrystal, but it I still have access to the Functional Genomics Center server, so I will demultiplex the samples there. 

