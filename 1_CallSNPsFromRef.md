# Variant calling with reference genome

Pipeline from raw data to SNP calling using a reference genome (Here Gallus gallus) 


#### 1. Demultiplex & remove adapter

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

This was done [previously](https://github.com/alexjvr1/WPA.Pheasants/blob/44e16715f8d74f7b479c7c9fdf3bf0bca015857f/pyRADopt.Pheasants.Nov2017.md) 

At this point samples that were sequenced in multiple libraries can be combined (using cat). 


Demultiplexed and cleaned data can be found here: 

alexjvr@fgcz-c-047:/srv/kenlab/alexjvr_p1795/WPA/Jan2018/WPAall_20180124/WPAallMerged_edits/*trimmed*_.gastq.gz

bluecp3:/


### 2. Map to Genome

Once the fastq files are in order, they can be mapped to the genome. We will use BWA-mem for this, since this is the optimal aligner for short sequence reads. Several different aligners can be compared using a program called [Teaser](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-015-0803-1): get the software [here](https://github.com/Cibiv/Teaser). 

But we first have to get the genome and do a little pre-processing. 

#### 2.1 Download the reference genome to the current working directory on BlueCrystal

First find the latest version of the genome on Ensembl or [NCBI](https://www.ncbi.nlm.nih.gov/assembly/GCF_000002315.5)
The address bar shows the address of this assembly where the name is broken into triplets: ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/002/315/

Use wget to copy the entire folder to bluecp3. Or copy just the fasta file of the full genome: 
```
wget --recursive ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/002/315/GCF_000002315.5_GRCg6a/ .

OR

wget ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/002/315/GCF_000002315.5_GRCg6a/*genomic.fna.gz .
```


#### 2.2 Index and sort the genome

pwd: /panfs/panasas01/bisc/aj18951/WPA
```
module avail
module load apps/bwa-0.7.15 
module load apps/samtools-1.8  

gunzip 
```

Genome needs to be indexed by both samtools and bwa
```

samtools faidx GCF_000002315.5_GRCg6a_genomic.fna

bwa index GCF_000002315.5_GRCg6a_genomic.fna
```


#### 2.3 fastq sequence names

BWA expects that all the sequences for paired reads are given the same names. But process_radtags appends _1 and _2 to the forward and rev reads respectively. This can be corrected using sed. 

First check the names:
```
pwd
/panfs/panasas01/bisc/aj18951/WPA/demultiplexed

gunzip -c PHE080.trimmed_R1_.fastq.gz | paste - - - - | cut -f 1| head

@1_1101_17025_1521_1
@1_1101_16922_1522_1
@1_1101_15563_1533_1
@1_1101_17116_1536_1
@1_1101_17071_1553_1
@1_1101_15369_1557_1
@1_1101_15403_1561_1
@1_1101_17592_1571_1
@1_1101_15060_1573_1
@1_1101_15000_1599_1

gunzip -c PHE080.trimmed_R2_.fastq.gz | paste - - - - | cut -f 1| head
@1_1101_17025_1521_2
@1_1101_16922_1522_2
@1_1101_15563_1533_2
@1_1101_17116_1536_2
@1_1101_17071_1553_2
@1_1101_15369_1557_2
@1_1101_15403_1561_2
@1_1101_17592_1571_2
@1_1101_15060_1573_2
@1_1101_15000_1599_2

```

Then edit all the files: 
```
gunzip *gz

for i in *fastq
do sed -i '1~4 s/_[12]$//' $i
done
```

Check that the names are correct now and then gzip all files again. 

#### 2.4 Map with BWA mem

Script to map all pairs. This creates an array with all the file names, and then iterates through the array. 
An alteranative would be a separate array for the fwd and rev reads. 
```
#!/bin/bash
#PBS -N WPAMapnew  ##job name
#PBS -l nodes=2:ppn=16  #nr of nodes and processors per node
#PBS -l mem=16gb #RAM
#PBS -l walltime=2:00:00 ##wall time.  
#PBS -j oe  #concatenates error and output files (with prefix job1)

#run job in working directory
cd $PBS_O_WORKDIR 
pwd
cd WPA
pwd

#Load modules
module load apps/bwa-0.7.15

#Define variables

RefSeq=GCF_000002315.5_GRCg6a_genomic.fna
total_files=`find demultiplexed/ -name '*.fastq.gz' | wc -l`
arr=( $(demultiplexed/*.fastq.gz) )
echo "mapping started" >> map.log
echo "---------------" >> map.log

#bwa index $RefSeq

for ((i=0; i<$total_files; i+=2))
{
sample_name=`echo ${arr[$i]} | awk -F "." '{print $1}'`
echo "[mapping running for] $sample_name"
printf "\n"
echo "bwa mem -t 32 $RefSeq ${arr[$i]} ${arr[$i+1]} > $sample_name.sam" >> map.log
bwa mem -t 32 $RefSeq ${arr[$i]} ${arr[$i+1]} > $sample_name.sam
}
```


