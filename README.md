# STAR.genome.index
Building a genome index with STAR

1. Download the data: fasta genome sequence and gtf annotation file

The following script (please do not run) would have downloaded the entire genome and the correct annotation for you:

```sh
mkdir genome
cd genome
# download the "Genome sequence, primary assembly (GRCh38)" fasta file
wget ftp://ftp.ebi.ac.uk/pub/databases/gencode/Gencode_human/release_29/GRCh38.primary_assembly.genome.fa.gz
# filter it as described in the note below
# download the annotations that correspond to it 
wget ftp://ftp.ebi.ac.uk/pub/databases/gencode/Gencode_human/release_29/gencode.v29.annotation.gtf.gz
```

With STAR you need to ungzip the genome files. In order to save space, It is recommanded recommend ungzipping both the fasta and the gtf files, 
and then re-gzipping the fasta once the genome generation step is done. We will need the unzipped gtf file later.

```sh
gunzip GRCh38.primary_assembly.genome.fa.chr19.gz
# wait
gunzip gencode.v29.primary_assembly.annotation.chr19.gtf.gz
# after genome has been generated
gzip GRCh38.primary_assembly.genome.fa.chr19
```

Now to make the genome index, you need to run the following script
```sh
dmukesha@fir160:/mnt/hive/resources/genomes$ sudo  STAR --runThreadN 23 --runMode genomeGenerate --genomeDir $GENOMEDIR/STAR --genomeFastaFiles $GENOMEDIR/STAR/GRCh38.primary_assembly.genome.fa --sjdbGTFfile $GENOMEDIR/STAR/gencode.v45.annotation.gtf
        /usr/lib/rna-star/bin/STAR-avx2 --runThreadN 23 --runMode genomeGenerate --genomeDir /mnt/hive/resources/genomes//STAR --genomeFastaFiles /mnt/hive/resources/genomes//STAR/GRCh38.primary_assembly.genome.fa --sjdbGTFfile /mnt/hive/resources/genomes//STAR/gencode.v45.annotation.gtf
        STAR version: 2.7.10a   compiled: 2022-01-16T16:35:44+00:00 <place not set in Debian package>
May 07 15:16:42 ..... started STAR run
May 07 15:16:42 ... starting to generate Genome files
        May 07 15:17:07 ..... processing annotations GTF
May 07 15:17:16 ... starting to sort Suffix Array. This may take a long time...
May 07 15:17:22 ... sorting Suffix Array chunks and saving them to disk...
May 07 17:15:01 ... loading chunks from disk, packing SA...
May 07 17:15:54 ... finished generating suffix array
May 07 17:15:54 ... generating Suffix Array index
May 07 17:18:40 ... completed Suffix Array index
May 07 17:18:40 ..... inserting junctions into the genome indices
May 07 17:27:52 ... writing Genome to disk ...
May 07 17:27:53 ... writing Suffix Array to disk ...
May 07 17:28:03 ... writing SAindex to disk
May 07 17:28:05 ..... finished successfully
```
