# Metagenome Quality Control

### Questions:
- How can we assess the quality of genomes from a metagenome (MAGs: Metagenome-Assembled Genomes)?
### Objectives: 
- Check the quality of the Metagenome-Assembled Genomes. 
### Keypoints:
- Use CheckM2 to evaluate the quality of each Metagenomics-Assembled Genome.

## Quality check 

The quality of a MAG is highly dependent on the size of the genome of the species, its abundance 
in the community and the depth at which we sequenced it.
Two important things that can be measured to know its quality are completeness (is the MAG a complete genome?) 
and if it is contaminated (does the MAG contain only one genome?). 

Advances in DNA sequencing and bioinformatics have dramatically increased the rate of recovery of microbial genomes from metagenomic data. Assessing the quality of metagenome-assembled genomes (MAGs) is a critical step prior to downstream analysis. [CheckM2](https://www.nature.com/articles/s41592-023-01940-w?utm_source=twitter&utm_medium=social&utm_campaign=nmeth) is an improved method of predicting the completeness and contamination of MAGs using machine learning. CheckM2 provides a set of tools for assessing the quality of genomes recovered from isolates, single cells, or metagenomes. It provides robust estimates of genome completeness and contamination by using collocated sets of genes that are ubiquitous and single-copy within a phylogenetic lineage. Assessment of genome quality can also be examined using plots depicting key genomic characteristics (e.g., GC, coding density) which highlight sequences outside the expected distributions of a typical genome. If you want to make comments about the functional potential of a genome, look for maximum genome completeness and minimal contamination. 

If your workflow involves metagenome assembled genomes (MAGs), then CheckM QC is likely one of the first things you will want to perform (i.e. prior to annotation of the AssemblySet). This information will indicate which genome bins should be discarded (i.e. rendered as unbinned) prior to analyses of the bins (e.g. Taxonomic Classification).

Input and Parameters:

Assembly, Genome, or BinnedContigs: A user may submit a single genome Assembly object, an AssemblySet, a Genome, a GenomeSet, or a BinnedContig object containing multiple "binned" genomes. For every input assemblies/genomes/bin, a separate evaluation of the genome completeness using the clade-specific phylogenetic marker genes will be performed.

Save all plots: The user has the option of generating and downloading all possible plots from the CheckM lineage workflow. Note that selecting this option will slow down the runtime (perhaps 10-20%).

Output:

Output Report: The output report offers both graphical and tabular representations of the phylogenetic marker completeness and contamination. CheckM generates clade-specific marker gene sets for each bin and reports the taxonomic resolution possible for each bin in the "Marker Lineage" column. Users may want to look at the "Marker Lineage" column to see what MAGs were classified with, for example, the "d__Bacteria" or "d__Archaea" marker sets. Instances where a broad (domain-level) marker set is used compared to a marker set from specific lineage (e.g. c__Alphaproteobacteria) can help one contextualize (and evaluate) the genome completeness and contamination estimates.

The number of Genomes that were used in generating each marker set is given, as is the number of markers generated. Marker genes are typically single-copy, so the occurrence of more than one in a given genome or bin may reveal contamination, which is indicated with yellow to red bars in the graphical depiction and by the columns "2" to "5+" in the table. As noted above in the article on assumptions, for incomplete genomes (e.g. 50-70%) the contamination measure is going to be an underestimate. In other words, be wary of a genome that is 50% complete with 0% contamination - contamination is present, this tool just doesn't detect it.

The fraction of marker genes that occur as duplicates is used to calculate the "Contamination" percentage in the table. Missing clade-specific phylogenetic markers are shown in gray in the plot and by the column "0" in the table, with the "Completeness" value obtained by the proportion of the missing markers to the total number of markers used. The presence of one and exactly one copy of a marker is indicated with a green bar in the plot and the tally in the "1" column of the table. Ideally, a perfect Genome will have all markers in exactly one copy assuming that the derivation of the markers was itself perfectly done and biology was perfectly predictable. Be sure to inspect results to ensure they are accurate. For example, for lineages not well-characterized in the CheckM database, the CheckM program will produce dubious results because marker gene assumptions are broken.

Files: Plots and data output files are produced by the CheckM lineage workflow. Additionally, a Tab-delimited TSV table in zipped text format that contains the CheckM assessment summary (matching that in the HTML CheckM Table report) for each bin is available.

Let's run CheckM2. Quick note that this will take 20 minutes to run on a node with 24G of memory and 24 cores.

```
$ interactive -t 04:30:00 -m 24G -a bh_class  # be sure to get a node with enough memory
$ export MAXBIN=/xdisk/bhurwitz/bh_class/YOUR_NETID/exercises/09_metag_binning/assembly_JP4D
$ export CHECKM=/xdisk/bhurwitz/bh_class/YOUR_NETID/exercises/10_assembly_qc/assembly_JP4D_v2
$ cd $MAXBIN
$ apptainer run /contrib/singularity/shared/bhurwitz/checkm2\:1.0.1--pyh7cba7a3_0.sif checkm2 \
       predict --threads 24 \
       --input $MAXBIN \
       -x fasta \
       --output-directory $CHECKM \
       --database_path /groups/bhurwitz/databases/checkm2_database/uniref100.KO.1.dmnd
      
```

Let's take a look at quality_report.tsv

```
$ cd $CHECKM
$ cat quality_report.tsv 
```

```
Name	Completeness	Contamination	Completeness_Model_Used	Translation_Table_Used	Coding_Density	Contig_N50 Average_Gene_Length	Genome_Size	GC_Content	Total_Coding_Sequences	Additional_Notes
assembly_JP4D.001	67.67	13.37	Gradient Boost (General Model)	11	0.891	1670	218.7291812456263	3141556	0.55	4287	None
assembly_JP4D.002	100.0	38.39	Gradient Boost (General Model)	11	0.894	2655	234.30446360639107	6186438	0.67	7886	None
assembly_JP4D.003	55.4	9.48	Gradient Boost (General Model)	11	0.885	1594	219.36060401171963	3289972	0.48	4437	None
assembly_JP4D.004	93.19	27.36	Gradient Boost (General Model)	11  0.868	2114	236.2645207439199	5692657	0.39	6990	None

```

Ideally, we would like to get only one contig per bin, with a length similar to the genome size of the corresponding taxa. Since this scenario is difficult to obtain, we can use parameters showing how good our assembly is. Here are some of the most common metrics:

Contig_N50:
If we arrange our contigs by size, from larger to smaller, and divide the whole sequence in half, N50 is the size of the smallest contig in the half that has the larger contigs; and L50 is the number of contigs in this half of the sequence. So we want big N50 and small L50 values for our genomes. Read [What is N50?](https://www.molecularecologist.com/2017/03/29/whats-n50/).

Contamination:
The question of how much contamination we can tolerate and how much completeness we need depends on the scientific question being tackled, Check out the [CheckM](https://genome.cshlp.org/content/25/7/1043) paper for more details.

> ## Discussion: The quality of MAGs
>
> Can we trust the quality of our bins only with the given information? 
> What else do we want to know about our MAGs to use for further analysis confidently?
> 
<details>
  <summary markdown="span">Solution</summary>
  <ul> 

**completeness** tells you how complete each genome is in the bin is. If the MAG is incomplete and highly fragmented, then you likely did not find that genome in your sample. 

**Genome size** and **GC content** are like genomic fingerprints of taxa, so you can know if you have the taxa you are looking for. Since we are working with the mixed genomes of a community when we try to separate them with binning.  

**contamination** to 
We want to know if we were able to separate each genome correctly. Contiamination tells use if we have more than one genome in our bin.
</details>

<br>

You will also notice that CheckM2 provides you with two other output directories:

diamond_output: Protein annotations from the program Diamond

protein_files: Genes detected on your contigs from the program prodigal

CheckM2 uses these outputs to determine how novel each of the genomes are in the bins based on known protein annotations.


