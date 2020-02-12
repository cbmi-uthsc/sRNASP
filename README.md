# sRNASP
small RNA Sequencing Pipeline


## Installation of dependencies
```
cpan
install Archive::Extract
exit
```
## Unitas Installation
```
wget https://www.smallrnagroup.uni-mainz.de/software/unitas_1.7.5.zip
unzip unitas_1.7.5.zip
wget http://www-personal.umich.edu/~jianghui/seqmap/download/seqmap-1.0.12-linux-64.zip
unzip seqmap-1.0.12-linux-64.zip
mv seqmap-1.0.12-linux-64.zip seqmap.exe
chmod 755 seqmap.exe
chmod 755 unitas
cd unitas
mv seqmap.exe unitas
```
## Unitas pipeline Execution
```
#!/bin/sh
for path in ../UPitt-Tolerance_CANAN-sRNA-seq/sRNAseq_DATA/*.gz
do
	FILE=$(echo $path| cut -d'.' -f 3)
	gunzip ..$FILE.fastq.gz -c >..$FILE.fastq
	echo "gunzip done!"
	#Trim Illumina adaptor sequences and reference mapping 
  	perl unitas_1.7.5.pl -input ..$FILE.fastq -species homo_sapiens -trim_minlength 18 -trim AGATCGGAAGAG -threads 64 -silent
	echo "unitas reference genome mapping to human genome and trmming of adaptor sequences is completed!"
	gzip -c ..$FILE.fastq >..$FILE.fastq.gz
	echo "gz is done!"
done
```
## Data Quality
```
find . -name '*.fastq.fas' -print0|xargs -0 -n 2 grep -c '^>'>totalReads.csv
find . -name '*.trimmed' -print0|xargs -0 -n 2 grep -c '^>'>mappedReads.csv
```

