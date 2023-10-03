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

Advances in DNA sequencing and bioinformatics have dramatically increased the rate of recovery of microbial genomes from metagenomic data. Assessing the quality of metagenome-assembled genomes (MAGs) is a critical step prior to downstream analysis. [CheckM2](https://www.nature.com/articles/s41592-023-01940-w?utm_source=twitter&utm_medium=social&utm_campaign=nmeth) is an improved method of predicting the completeness and contamination of MAGs using machine learning. CheckM2 has universally trained machine learning models it applies regardless of taxonomic lineage to predict the completeness and contamination of genomic bins. This allows it to incorporate many lineages in its training set that have few - or even just one - high-quality genomic representatives, by putting it in the context of all other organisms in the training set. As a result of this machine learning framework, CheckM2 is also highly accurate on organisms with reduced genomes or unusual biology, such as the Nanoarchaeota or Patescibacteria.

CheckM2 uses two distinct machine learning models to predict genome completeness. The 'general' gradient boost model is able to generalize well and is intended to be used on organisms not well represented in GenBank or RefSeq (roughly, when an organism is novel at the level of order, class or phylum). The 'specific' neural network model is more accurate when predicting completeness of organisms more closely related to the reference training set (roughly, when an organism belongs to a known species, genus or family). CheckM2 uses a cosine similarity calculation to automatically determine the appropriate completeness model for each input genome, but you can also force the use of a particular completeness model, or get the prediction outputs for both. There is only one contamination model (based on gradient boost) which is applied regardless of taxonomic novelty and works well across all cases.

If your workflow involves metagenome assembled genomes (MAGs), then CheckM2 QC is likely one of the first things you will want to perform (i.e. prior to annotation of the AssemblySet). This information will indicate which genome bins should be discarded (i.e. rendered as unbinned) prior to analyses of the bins (e.g. Taxonomic Classification).

The main use of CheckM2 is to predict the completeness and contamination of metagenome-assembled genomes (MAGs) and single-amplified genomes (SAGs), although it can also be applied to isolate genomes.

Input and Parameters:

Assembly, Genome, or BinnedContigs: A user may submit a single genome Assembly object, an AssemblySet, a Genome, a GenomeSet, or multiple "binned" genomes. You can give it a folder with FASTA files using --input and direct its output with --output-directory:

checkm2 predict --threads 30 --input <folder_with_bins> --output-directory <output_folder> 

Database:

The --database_path can be used with checkm2 predict to provide an already downloaded checkm2 database during a single predict run:

checkm2 predict -i ./folder_with_MAGs -o ./output_folder --database_path /path/to/database/CheckM2_database/uniref100.KO.1.dmnd. 

We will use this database that is downloaded on the HOC for you.

Output:

Output Report: By default, the output folder will have a tab-delimited file quality_report.tsv containing the completeness and contamination information for each bin. Contamination in MAGs may come from the binning together of closely-related strain or species, but may potentially also contain divergent DNA from other lineages or even domains.

Improved genome quality predictions by CheckM2 are the result of considering fully annotated genomes in its machine learning models, as opposed to CheckM1’s requirement for single-copy marker gene sets for each lineage. An additional advantage of the CheckM2 approach is that its models can be easily and rapidly updated to incorporate additional high quality genomic representation for novel lineages, further increasing the accuracy of its genome quality predictions.

Let's run CheckM2. This will take 20 minutes to run on a node with 24G of memory and 24 cores.

```
$ interactive -t 04:30:00 -m 24G -a bh_class  # be sure to get a node with enough memory
$ export MAXBIN=/xdisk/bhurwitz/bh_class/YOUR_NETID/exercises/09_metag_binning/assembly_JP4D
$ export CHECKM=/xdisk/bhurwitz/bh_class/YOUR_NETID/exercises/10_assembly_qc/assembly_JP4D
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


