# Metagenome Binning

### Questions:
- How can we obtain the original genomes from a metagenome?
### Objectives: 
- Obtain Metagenome-Assembled Genomes from the metagenomic assembly. 
### Keypoints:
- Metagenome-Assembled Genomes (MAGs) sometimes are obtained from curated contigs grouped into bins.
- Use MAXBIN to assign the contigs to bins of different taxa.


## Metagenomic binning  
Contigs in the assembly can be separated into bins with a process called binning. This process allows for a separate analysis of each species contained in the metagenome with enough reads to reconstruct a genome. Genomes reconstructed from metagenomic assemblies are called MAGs (Metagenome-Assembled Genomes).
In this process, the assembled contigs from the metagenome will be assigned to different bins (FASTA files that contain certain contigs). Ideally, each bin corresponds to only one original genome (a MAG).

<a href="../fig/03-05-01.png">
  <img src="../fig/03-05-01.png" width="435" height="631" alt="Diagram depicts the DNA sequences in the original sample as circular chromosomes of three different taxa. After sequencing, the DNA sequences of the three different taxa are mixed as small linear reads; after the assembly, we have contigs, each corresponding to a single taxon, except for the ones with a bad assembly that has sequences of different taxa in the same contig, after the binning taxa separate the contigs."/>
</a>

An obvious way to separate contigs that correspond to a different species is by their taxonomic assignation, but 
there are more reliable methods that do the binning using 
characteristics of the contigs, such as their GC content, the use of tetranucleotides (composition), or their coverage (abundance).

[Maxbin](https://sourceforge.net/projects/maxbin/files/) is a binning algorithm 
that distinguishes between contigs that belong to different bins according to their 
coverage levels and the tetranucleotide frequencies they have.


> ## Discussion 1: Relation between MAGs and depth 
> The sequencing center has returned a file to you with 18,412 reads. Given that the bacterial genome size range
>  between 4Mbp and 13Mbp (Mb=10^6 bp) and that the size of the reads in this run is 150bp. With these data, 
>  how many complete bacterial genomes can you reconstruct?

<details>
  <summary markdown="span">Solution</summary>
  <ul> 
None, because 18,412 reads of 150bp gives a total count of 2,761,800 bp (~2Mbp). Even if no read maps to the same region, the amount of base pairs is less than the size of a bacterial genome. A “typical” bacterial genome is around 5 million bp.
</details>

<br>

Let's bin the samples we assembled. The command for running MaxBin is `run_MaxBin.pl`, and the arguments it needs are the FASTA file of the assembly, the FASTQ with the forward and reverse reads, the output directory, and the name. 

Let's get started.

```
$ cd /xdisk/bhurwitz/bh_class/YOUR_NETID/exercises/08_assembly/assembly_JC1A
$ mkdir ../../09_metag_binning/assembly_JC1A
$ /contrib/singularity/shared/bhurwitz/maxbin2:2.2.7--hdbdd923_5.sif run_MaxBin.pl \
-thread 8 -contig contigs.fasta \
-reads ../../06_qc_trimming/JC1A_R1.trim.fastq.gz \
-reads2 ../../06_qc_trimming/JC1A_R2.trim.fastq.gz \
-out ../../09_metag_binning/assembly_JC1A
$ mv ../../09_metag_binning/assembly_JC1A.* ../../09_metag_binning/assembly_JC1A
```

```
MaxBin 2.2.7
Thread: 8
Input contig: contigs.fasta
Located reads file [../../06_qc_trimming/JC1A_R1.trim.fastq.gz]
Located reads file [../../06_qc_trimming/JC1A_R2.trim.fastq.gz]
out header: ../../09_metag_binning/assembly_JC1A
Running Bowtie2 on reads file [../../06_qc_trimming/JC1A_R1.trim.fastq.gz]...this may take a while...
Running Bowtie2 on reads file [../../06_qc_trimming/JC1A_R2.trim.fastq.gz]...this may take a while...
Searching against 107 marker genes to find starting seed contigs for [contigs.fasta]...
Try harder to dig out marker genes from contigs.
Marker gene search reveals that the dataset cannot be binned (the medium of marker gene number <= 1). Program stop.
```

Oh No! It's impossible to bin our assembly because the number of marker genes is less than 1. 
We could have expected this as we know it is a small sample.

Let's try this on another sample that is bigger.

We will perform the binning process with the other sample from the same study that is larger. We have the assembly precomputed in the `/xdisk/bhurwitz/bh_class/mags/` directory that you can copy over and use.

```
$ cd ..
$ mkdir assembly_JP4D
$ cd assembly_JP4D
$ cp /xdisk/bhurwitz/bh_class/mags/JP4D_contigs.fasta .
$ mkdir ../../09_metag_binning/assembly_JP4D
$ /contrib/singularity/shared/bhurwitz/maxbin2:2.2.7--hdbdd923_5.sif run_MaxBin.pl \
-thread 8 -contig JP4D_contigs.fasta \
-reads ../../06_qc_trimming/JP4D_R1.trim.fastq.gz \
-reads2 ../../06_qc_trimming/JP4D_R2.trim.fastq.gz \
-out ../../09_metag_binning/assembly_JP4D
```

It will take ~7 min minutes to run. Moreover, it will finish with an output like this:

```
========== Job finished ==========
Yielded 4 bins for contig (scaffold) file JP4D_contigs.fasta

Here are the output files for this run.
Please refer to the README file for further details.

Summary file: ../../09_metag_binning/assembly_JP4D.summary
Genome abundance info file: ../../09_metag_binning/assembly_JP4D.abundance
Marker counts: ../../09_metag_binning/assembly_JP4D.marker
Marker genes for each bin: ../../09_metag_binning/assembly_JP4D.marker_of_each_gene.tar.gz
Bin files: ../../09_metag_binning/assembly_JP4D.001.fasta - ../../09_metag_binning/assembly_JP4D.004.fasta
Unbinned sequences: ../../09_metag_binning/assembly_JP4D.noclass

Store abundance information of reads file [../../06_qc_trimming/JP4D_R1.trim.fastq.gz] in [../../09_metag_binning/assembly_JP4D.abund1].
Store abundance information of reads file [../../06_qc_trimming/JP4D_R2.trim.fastq.gz] in [../../09_metag_binning/assembly_JP4D.abund2].

========== Elapsed Time ==========
0 hours 6 minutes and 18 seconds.

```

With the `.summary` file, we can quickly look at the bins that MaxBin produced. First we will move all of the files into the assembly_JP4D directory.

```
$ mv assembly_JP4D.* assembly_JP4D
$ cat ./assembly_JP4D/assembly_JP4D.summary
```

```
Bin name	Completeness	Genome size	GC content
assembly_JP4D.001.fasta	57.9%	3141556	55.5
assembly_JP4D.002.fasta	87.9%	6186438	67.3
assembly_JP4D.003.fasta	51.4%	3289972	48.1
assembly_JP4D.004.fasta	77.6%	5692657	38.9
```
 
