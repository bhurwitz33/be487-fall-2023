
# Project Organization

## Questions:
- How can I organize my file system for a new bioinformatics project?
- How can I document my work?

Objectives:
- Create a file system for a bioinformatics project.
- Use the `history` command and a text editor like `nano` to document your work on your project.

Keypoints:
- Spend the time to organize your file system when you start a new project. Your future self will thank you!

## Getting your project started

Project organization is one of the most important parts of a sequencing project, and yet is often overlooked amidst the
excitement of getting a first look at new data. Of course, while it is best to get yourself organized before you even begin your analyses,
it is never too late to start, either.  

You should approach your sequencing project similarly to how you do a biological experiment and this ideally begins with experimental design. We're going to assume that you've already designed a beautiful sequencing experiment to address your biological question, collected appropriate samples, and that you have enough statistical power to answer the questions you're interested in asking. These steps are all incredibly important, but beyond the scope of our course. For all of those steps (collecting specimens, extracting DNA, prepping your samples) you've likely kept a lab notebook that details how and why you did each step. However, the process of documentation doesn't stop at the sequencer!  

Genomics projects can quickly accumulate hundreds of files across tens of folders. Every computational analysis you perform over the course of your project is going to create many files, which can especially become a problem when you'll inevitably want to run some of those analyses again. For instance, you might have made significant headway into your project, but then have to remember the PCR conditions you used to create your sequencing library months prior. 

Other questions might arise along the way: 
- What were your best alignment results?
- Which folder were they in: Analysis1, AnalysisRedone, or AnalysisRedone2?
- Which quality cutoff did you use?
- What version of a given program did you implement your analysis in?

Good documentation is key to avoiding this issue, and luckily enough, recording your computational experiments is even easier than recording lab data. Copy/Paste will become your best friend, sensible file names will make your analysis understandable by you and your collaborators, and writing the methods section for your next paper will be easy! Remember that in any given project of yours, it's worthwhile to consider a future version of yourself as an entirely separate collaborator. The better your documenation is, the more this 'collaborator' will feel indebted to you!

With this in mind, let's have a look at the best practices for documenting your genomics project. Your future self will thank you.  

In this exercise we will look at/set up file system for the project we will be working on during this class.  

We will start by looking at all of the directories under "exercises". 

```
$ cd /xdisk/bhurwitz/bh_class/your_netid/exercises
$ ls
```

You should see the output: 

```
01_intro_unix      07_contam_removal  13_microviz          19_pca_taxon_stats
02_bash_scripting  08_assembly        14_alpha_diversity   20_kmer_comparisons
03_cat_n           09_metag_binning   15_beta_diversity_1  21_functional_annot
04_intro_hpc       10_assembly_qc     16_beta_diversity_2  
05_getting_data    11_taxonomy        17_ordination
06_qc_trimming     12_phyloseq        18_abundance_trans
```

> ## Organizing your Directories
> This class was set up to walk you through a complete set of
> metagenomic analyses for a given project. Each of our steps in this 
> process are numbered according to their order of operation and have
> their own directory associated with them. For example, after we
> create genomes from our metagenomes (08_assembly), we will bin our assemblies
> (09_metag_binning) and look at the quality of the assemblies (10_assembly_qc). 
> Each of the analyses
> will be in the project directory for that step (e.g 09_metag_binning),
> but may refer back to data from a prior step (e.g 08_assembly). 
> This helps us to find the results for each step easliy.

> ## Exercise 1: Directory Organization
>
> Can you think of another reason we might want to set up an ordered 
> set of directories for a given project?
<details>
  <summary markdown="span">Solution</summary>
  <ul> Oftentimes as bioinformaticists, we will want to automate multiple steps in an analysis. We call this a workflow. Each step in the workflow usually runs one "task", which is a computation that runs on a single node in a "reasonable" amount of time. In other words, we need the task to finish before we run out of time on that cluster node. A task can be comprised of multiple shorter steps. For example, I typically run fastqc and trimming as a single task (or step in the workflow). The fastqc program runs very quickly, so I can easily pair it with a trimming step. I usually set up my output directories by task, and in order of operation for the complete workflow.  
</details>

## Organizing your files

To kick off our project, we will download sequence data from the sequence read archive (SRA). These data are considered our "raw" data, and represent the data that came off the sequencer. The raw data should never be changed. Regardless of how sure you are that you want to carry out a particular data cleaning step, there's always the chance that you'll change your mind later or that there will be an error in carrying out the data cleaning and you'll need to go back a step in the process. Having a raw copy of your data that you never modify guarantees that you will always be able to start over if something goes wrong with your analysis. As a result, all of the analyses performed on the "raw" data will be in a different folder. 

Do you remember how to prevent overwriting your raw data files by setting restrictive file permissions?

## File names

Sometimes you will see bioinformaticists use file extensions to indicate what analyses have been run on the file. For example, you might see a raw data file called ERR2198720.fastq, and then the quality trimmed file called ERR2198720.trimmed.fastq. This is a bad idea! The reason is that file names can get very long (e.g. ERR2198720.trimmed.human_removed.phix_removed.fastq). Yikes! This is also a problem for automating tasks in an HPC pipeline where you have to remember all those extensions when referring back to the file. Instead, we can just save the files (with the same name, ERR2198720.fastq) in different directories according to what analysis was run on them. You will come to appreciate this later!

## Documenting your activity on the project

When carrying out wet-lab analyses, most scientists work from a written protocol and keep a hard copy of written notes in their lab notebook, including any things they did differently from the written protocol. This detailed record-keeping process is just as important when doing computational analyses. Luckily, it's even easier to record the steps you've carried out computational than it is when working at the bench.

The `history` command is a convenient way to document all the
commands you have used while analyzing and manipulating your project
files. Let's document the work we have done on our project so far. 

View the commands that you have used so far during this session using `history`:

```
$ history
```

The history likely contains many more commands than you have used for the current project. Let's view the last several commands that focus on just what we need for this project.   

View the last n lines of your history (where n = approximately the last few lines you think relevant). For our example, we will use the last 7:

```   
$ history | tail -n 7
```

> ## Exercise 2: Creating a record of the used commands 
> 
> Using your knowledge of the shell, use the append redirect `>>` to create a file called
> `log_XXXX_XX_XX.sh` (Use the four-digit year, two-digit month, and two digit day, e.g.
> `log_2023_09_12.sh`)  

<details>
  <summary markdown="span">Solution</summary>
```
$ history | tail -n 7 >> log_2023_09_12.sh
```

Note we used the last 7 lines as an example, the number of lines may vary.

</details>

<br>

Congratulations! You've finished your introduction to using the shell for metagenomics projects. You now know how to navigate your file system, create, copy, move, and remove files and directories, and automate repetitive tasks using scripts and wildcards. With this solid foundation, you're ready to move on to apply all of these new
skills to carrying out more sophisticated bioinformatics
analysis work. Don't worry if everything doesn't feel perfectly comfortable yet. We're going to have many more opportunities for practice as we move forward on our bioinformatics journey!
