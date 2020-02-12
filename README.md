# sRNASP
small RNA Sequencing Pipeline


cpan
install Archive::Extract
exit

wget https://www.smallrnagroup.uni-mainz.de/software/unitas_1.7.5.zip

unzip unitas_1.7.5.zip
wget http://www-personal.umich.edu/~jianghui/seqmap/download/seqmap-1.0.12-linux-64.zip

rename it to seqmap.exe and move into unitas directory


#!/bin/sh
for path in ../UPitt-Tolerance_CANAN-sRNA-seq/sRNAseq_DATA/*.gz
do
	FILE=$(echo $path| cut -d'.' -f 3)
	gunzip ..$FILE.fastq.gz -c >..$FILE.fastq
	echo "gunzip done!"
	#Trim Illumina adaptor sequences
  perl unitas_1.7.5.pl -input ..$FILE.fastq -species homo_sapiens -trim_minlength 18 -trim AGATCGGAAGAG -threads 64 -silent
	echo "unitas reference genome mapping to human genome and trmming of adaptor sequences is completed!"
	gzip -c ..$FILE.fastq >..$FILE.fastq.gz
	echo "gz is done!"
done

#Data Quality
find . -name '*.fastq.fas' -print0|xargs -0 -n 2 grep -c '^>'>totalReads.csv
find . -name '*.trimmed' -print0|xargs -0 -n 2 grep -c '^>'>mappedReads.csv

