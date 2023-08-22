
# Data Tidiness

### Questions:
- "What metadata should I collect?"
- "How should I structure my sequencing data and metadata?"
- "Which ethical considerations involve microbiome data and metadata?" 
### Objectives:
- "Think about and understand the types of metadata a sequencing experiment will generate."
- "Understand the importance of metadata and potential metadata standards."
- "Explore common formatting challenges in spreadsheet data."
- "Consider confidentiality issues that may arise from storing and sharing the metadata."
### Keypoints:
- "Assigning and keeping track of appropriate unique ID identifiers must be a well-thought process."
- "Metadata is key for you and others to work with your data."
- "Tabular data needs to be structured to work with it effectively."
- "Human microbiome data requires informed consent and confidentiality."

# Data Tidiness

When we think about the data for a sequencing project, we often start by thinking about 
the sequencing data that we get back from the sequencing center, but just as important, 
if not more so, is the data you have generated about the sequences before they ever go to 
the sequencing center. Data about the data are often called the metadata. 
The sequence data itself is useless without the information about what you sequenced.  

> ## Discussion 1
> Think/Pair/Share: With the person next to you, discuss:
> What kinds of data and information should you generate before you send your DNA/RNA off for sequencing?
>
<details>
  <summary markdown="span">Solution</summary>
Types of files and information you have generated:  
<ul>
<li>Spreadsheet or tabular data with the data from your experiment and 
whatever you were measuring for your study.</li>
<li>Lab notebook notes about how you conducted those experiments.</li>
<li>Spreadsheet or tabular data about the samples you sent for sequencing.Sequencing centers often have a particular format they need 
with the name of the sample, DNA concentration, and other information.</li>
<li>Lab notebook notes about how you prepared the DNA/RNA for sequencing and what type of sequencing you are doing, e.g., paired-end Illumina HiSeq.</li>
<br>
There likely will be other ideas here too.
Was this more information and data than you were expecting?
</details>
<br>

All the data and information discussed can be considered metadata, 
i.e., data about the data. We want to follow a few guidelines for metadata.

## Notes about your experiment

Notes about your experiment, including how you prepared your samples for sequencing, 
should be in your lab notebook, whether a physical lab notebook or an electronic one. 
For guidelines on good lab notebooks, see the Howard Hughes Medical Institute 
"Making the Right Moves: A Practical Guide to Scientifıc Management for Postdocs and New Faculty" 
the section on [Data Management and Laboratory Notebooks](http://www.hhmi.org/sites/default/files/Educational%20Materials/Lab%20Management/Making%20the%20Right%20Moves/moves2_ch8.pdf).


Including dates on your lab notebook pages, the samples themselves, and in
any records about those samples help you associate everything with each
other later. Using dates also helps create unique identifiers because even
if you process the same sample twice, you do not usually do it on the same
day, or if you do, you are aware of it and give them names like A and B.

> ## Unique identifiers
> Unique identifiers are unique names for a sample or set of sequencing data.
> They are names for that data that only exist for that data. Having these
> unique names makes them much easier to track later.


## Metadata: Data about the experiment

Data about the experiment is usually collected in **spreadsheets**, like Excel. 
Alongside the spreadsheet, creating a text file called **README** is convenient. 
This file holds information about the research project, how the samples were generated, 
and how to read the metadata spreadsheet. 
This information is very convenient when working on a team, and more
people join the project even at different 
experiment steps. This order may help better your collaborators to easily understand 
what the data is about.

## Metadata standards
The kind of data you would collect depends on your experiment; often, there are guidelines from metadata standards.
Many fields have particular ways that they structure their metadata, so it is consistent and can be used across the area. The Digital Curation Center maintains 
[a list of metadata standards](http://www.dcc.ac.uk/resources/metadata-standards/list) and some that are particularly relevant for genomics data are available from the [Genomics Standards Consortium](http://gensc.org/projects/). In particular, assembly quality and an estimate of genome completeness and contamination are [standards for Metagenome-Assambled-Genomes](https://www.nature.com/articles/nbt.3893) (MAGs).  

Cornell University gives us a useful [guide](https://data.research.cornell.edu/content/readme) and template [file](https://github.com/nselem/organization-metagenomics/blob/master/files/AUTHOR_DATASET_ReadmeTemplate.txt) to write README-style metadata.

> ## Discussion 2. What information would you write in your README file? 
> Propose a solution: Suppose that in your field, there are no metadata standards yet. Think about the 
> minimum amount of information that someone would need to be able to work with your data without talking to you. 
> What type of information would you put in your README file? 
>
<details>
  <summary markdown="span">Solution</summary>
Some examples of clarifications that need to be written in the README are:
<ul>
<li>Date format (mm-dd-yyyy or dd-mm-yyyy, for example).</li>
<li>Meaning of abbreviations.</li>
<li>Meaning or pattern followed to construct the unique IDs of samples.</li>
<li>Details about the methodology.</li>
<li>Contact information about the persons who performed the collection and/or experiments.</li>
<li>Meaning of each variable name.</li>
</ul>
</details>

> ## Discussion 3: Ethical considerations in microbiome studies
> Case Study: The microbiomes knowledge is human's global heritage and must be ruled by ethical principles such as do good, do not harm, respect, and act justly.
> While studying the human microbiome, there are particular concerns. For example, if you discover an infection such as HIV in the blood microbiome, would you inform the participants? What If they did not want to know? Following these principles, what other ethical considerations can you think about human microbiomes? 
<details>
  <summary markdown="span">Solution</summary>
  <ul>
<li>Respect: Ask participants to sign explicit prior informed consent.</li>  
<li>Do good: Share the data but protect participant's privacy</li>
<li>Do no harm: establish a policy about results communications.</li>
<li>Do no harm: Consider the Invasiveness of sampling and minimize the risk</li>
<li>Act justly: Favor diversity of subjects and justice</li> 
</ul>
</details>

<br>


### Structuring data in spreadsheets

Independent of the type of data you are collecting, there are standard ways to enter that data 
into the spreadsheet to make it easier to analyze later. We often enter data that makes it easy 
for us as humans to read and work with it because we are human! Computers need data structured in 
a way that they can use it. So to use this data in a computational workflow, we need to think 
like computers when we use spreadsheets.

The cardinal rules of using spreadsheet programs for data:

- Leave the raw data raw - do not change it!
- Put each observation in its own row. An observation is each of our samples, 
the subjects for which we store information in the spreadsheet. 
- Put all your variables in columns. The variables are the different pieces of information 
that we have about our sample (its genotype, phenotype, treatment, etc.). 
- Have column names explanatory but without spaces. Use '-', '_' or 
[camel case](https://en.wikipedia.org/wiki/Camel_case) instead of a space. 
For instance, 'library-prep-method' or 'LibraryPrep'is better than 'library preparation method' 
or 'prep', because computers interpret spaces in particular ways.
- Do not combine multiple pieces of information in one cell. Sometimes it just seems 
like one thing, but think if that is the only way you will want to be able to use or sort that data. 
For example, instead of having a column with species and strain name (e.g., *E. coli* 
K12) you would have one column with the species name (*E. coli*) and another with 
the strain name (K12). Depending on the type of analysis you want to do, you may
even separate the genus and species names into distinct columns.
- Export the cleaned data to a text-based format like CSV (comma-separated values). 
This format ensures that anyone can use the data required by most data repositories.


> ## Discussion 4. Spreadsheet organization.
> Think/Pair/Share: This is a potential spreadsheet data generated about a sequencing experiment. With the person next to you, for about 2 minutes, discuss some problems with the spreadsheet data shown below. You can look at the image or download the file to your computer via this [link](https://github.com/datacarpentry/organization-genomics/raw/gh-pages/files/Ecoli_metadata_composite_messy.xlsx) and open it in a spreadsheet reader like Excel. 
> [![Messy spreadsheet](../fig/01_tidiness_datasheet_example_messy.png)](https://github.com/datacarpentry/organization-genomics/raw/gh-pages/files/Ecoli_metadata_composite_messy.xlsx)
>
<details>
  <summary markdown="span">Solution</summary>
  <ul>
There are a few potential errors to be on the lookout for in your own data and data from collaborators or the Internet. Suppose you are aware of the errors and the possible negative effect on downstream data analysis and result interpretation. In that case, it might motivate you and your project members to try and avoid them. Making small changes to how you format your data in spreadsheets can greatly impact efficiency and reliability when it comes to data cleaning and analysis.
</ul>

<li>Using multiple tables
<li>Using multiple tabs
<li>Not filling in zeros
<li>Using problematic null values
<li>Using formatting to convey information
<li>Using formatting to make the datasheet look pretty
<li>Placing comments or units in cells
<li>Entering more than one piece of information in a cell
<li>Using problematic field names
<li>Using special characters in data
<li>Inclusion of metadata in the data table
<li>Date formatting
</details>
<br>

You can keep exploring issues on the metadata file in the [Data Carpentry Ecology spreadsheet lesson](http://www.datacarpentry.org/spreadsheet-ecology-lesson/02-common-mistakes/). Not all are present in this example. Discuss with the group what they found. Some problems include not all data sets having the same columns, datasets split into their own tables, color to encode information, different column names, and spaces in some columns names. Here is a "clean" version of the same spreadsheet:
[Cleaned spreadsheet](https://raw.githubusercontent.com/datacarpentry/wrangling-genomics/gh-pages/files/Ecoli_metadata_composite.tsv)
Download the file using right-click (PC)/command-click (Mac). 


## Further notes on data tidiness

Data organization at this point of your experiment will help facilitate 
your analysis later, as well as prepare your data and notes for data deposition
now often required by journals and funding agencies. If this is a collaborative
the project, as most projects are now, it is also information that collaborators will
need to interpret your data and results and is very useful for communication and
efficiency.

Fear not! If you have already started your project and it is not set up this way, 
there are still opportunities to make updates. One of the biggest challenges is 
tabular data that is not formatted so computers can use it or has inconsistencies
that make it hard to analyze.

When working in the command line, it is problematic to put spaces in the names of directories and files. 

More practice on how to structure data is outlined in our 
[Data Carpentry Ecology spreadsheet lesson](http://www.datacarpentry.org/spreadsheet-ecology-lesson/02-common-mistakes/)

Tools like [OpenRefine](http://www.datacarpentry.org/OpenRefine-ecology-lesson/) can help you clean your data.
