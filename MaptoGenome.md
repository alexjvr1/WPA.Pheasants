# Map to Genome


Got the latest chicken genome from NCBI. Find where the latest version is on NCBI 

https://www.ncbi.nlm.nih.gov/genome/genomes/111

The ftp address on the table (right hand most column)
```
pwd 

  /srv/kenlab/alexjvr_p1795/WPA/MaptoGallusgallus/

wget -r ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/000/002/315/GCA_000002315.5_GRCg6a 
```


Test if the pheasant samples can be mapped to the genome
```


```


stats
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
