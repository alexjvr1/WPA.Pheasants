# Map to Genome


Got the latest chicken genome from NCBI. Find where the latest version is on NCBI 

https://www.ncbi.nlm.nih.gov/genome/genomes/111

The ftp address on the table (right hand most column)
```
pwd 

  /srv/kenlab/alexjvr_p1795/WPA/MaptoGallusgallus/

wget -r ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/000/002/315/GCA_000002315.5_GRCg6a 
```


### Test if the pheasant samples can be mapped to the genome

1. rename the sequences in the reverse reads. These were demultiplexed with process_radtags, so the fwd and rev reads have different endings to their names (_1 and _2) 
```
sed -i 's/_2$/_1/g' /srv/kenlab/alexjvr_p1795/WPA/Jan2018/demultiplexed/lib1.demultiplexed_20180123/PHE079renamed_2.2.fq
```

2. Align with bwa mem
```
/usr/local/ngseq/packages/Aligner/BWA/0.7.15/bin/bwa mem -t 10 GCA_000002315.5_GRCg6a_genomic.fna.gz /srv/kenlab/alexjvr_p1795/WPA/Jan2018/demultiplexed/lib1.demultiplexed_20180123/PHE079_2.1.fq /srv/kenlab/alexjvr_p1795/WPA/Jan2018/demultiplexed/lib1.demultiplexed_20180123/PHE079renamed_2.2.fq > aln-pe.PHE079.sam
```


3. Index genome
```
gunzip GCA_000002315.5_GRCg6a_genomic.fna.gz

/usr/local/ngseq/packages/Tools/samtools/1.5/bin/samtools faidx GCA_000002315.5_GRCg6a_genomic.fna
```


4. Convert sam to bam and sort bam file
```
/usr/local/ngseq/packages/Tools/samtools/1.5/bin/samtools import GCA_000002315.5_GRCg6a_genomic.fna.fai aln-pe.PHE079.sam aln-pe.PHE0179.bam

/usr/local/ngseq/packages/Tools/samtools/1.5/bin/samtools sort aln-pe.PHE0179.bam -o PHE079.bam.sorted
```


5. stats
```
/usr/local/ngseq/packages/Tools/samtools/1.5/bin/samtools flagstat PHE079.bam.sorted 
176831 + 0 in total (QC-passed reads + QC-failed reads)
0 + 0 secondary
3153 + 0 supplementary
0 + 0 duplicates
173535 + 0 mapped (98.14% : N/A)
173678 + 0 paired in sequencing
86839 + 0 read1
86839 + 0 read2
163598 + 0 properly paired (94.20% : N/A)
169244 + 0 with itself and mate mapped
1138 + 0 singletons (0.66% : N/A)
4340 + 0 with mate mapped to a different chr
2796 + 0 with mate mapped to a different chr (mapQ>=5)

```

### Map all sequences to genome and call SNPs

Since all individuals were sequenced across 3 lanes, data was merged and trimmed using pyRAD. 
These data can be found here: /srv/kenlab/alexjvr_p1795/WPA/Jan2018/WPAall_20180124/WPAallMerged_edits/*trimmed_*fastq.gz



