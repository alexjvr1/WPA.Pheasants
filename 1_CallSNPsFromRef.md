# Variant calling with reference genome

Pipeline from raw data to SNP calling using a reference genome (Here Gallus gallus) 


### 1. Assess quality of raw data

### 2. Demultiplex & trim (& combine duplicated samples)

### 3. Map to Genome (BWA-mem)

### 4. Prepare mapped reads for variant calling: 
        
        - add Reading Groups, sort, remove PCR duplicates
        
### 5. Call variants on individuals (g.vcf), which includes local alignment

### 6. Join all g.vcf files together (GenomesDBImport)

### 7. Call variants on joing dataset

### 8. Filter the variants. 




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



## 1. 
