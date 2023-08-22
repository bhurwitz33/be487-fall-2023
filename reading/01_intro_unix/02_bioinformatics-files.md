# Bioinformatics Files

```
In order to be able to work with bioinformatic files, we first need to understand their format
Most bioinformatic files we work with are text-based files that contain a great length of information. 

It is very important to understand how they are formatted and observe them before working with them, so that we can parse through the files in the most efficient way.
```

## FastQ
[FastQ format](https://en.wikipedia.org/wiki/FASTQ_format#:~:text=FASTQ%20format%20is%20a%20text,single%20ASCII%20character%20for%20brevity.) is a text file in which each entry is composed of a set of four lines:

Line 1 begins with @ and is followed by the sequence identifier.

Line 2 is the raw sequence nucleotide letters

Line 3 is a + character alone (in some cases, it may be followed again by the sequence identifier)

Line 4 encodes the quality values for each letter in the sequence (Line 2 and 4 must be the same length)

For example:
```

@A00428:110:HKJFMDSXX:3:1101:28248:1000 1:N:0:TGCTTCCA+NTCGATCG

ANCTCACGCTCATCAATAAATTCTGTAAACAAGCACAATTTTCCTCCCACTCTGTTTCCCAACTACTTCCCACCCTGTGAAGCTGGCGGAAACATCCTGATGAAGCACAAAGTATTTCTGGCCCCCGGAGCTGCCCTGGGTCACTGACCAC

+

F#FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF

```

# FastA
[FastA format](https://en.wikipedia.org/wiki/FASTA_format) is a text file in which each entry is composed of a set of two lines:

Line 1 begins with > and is followed by the sequence identifier.

Line 2 is the sequence nucleotide letters.

For example:

```

>NC_054070.1 Falco naumanni isolate bFalNau1 chromosome 17, bFalNau1.pat, whole genome shotgun sequence

aaccctaagagcctgagtctaaccctaaccctcaccctaagagcctgagtctaaccctcaccctaagagcctgagtctaaccctaacccgaagagcctga

```

## BED
[BED format](https://en.wikipedia.org/wiki/BED_%28file_format%29) is a text file in which each entry is composed of one line with genomic coordinates and any associated annotations. 

A BED file contains a minimum of three columns, with the first three columns being the chromosome, start, and stop coordinates of the regions considered.

The start coordinate is zero-based, and the stop coordinate is non-inclusive.

For example:
```
NC_054069.1     12      4897    -8209981        +       (CTAACC)n       Simple_repeat   1       4516    0       119312
```
In this file, the first three columns refer to the genomic region, and the remaining columns provide additional annotations specific to this region.

## GTF/GFF
GTF or GFF formats are text files in which each entry is composed of one line, used for describing genes and other features of DNA, RNA and protein sequences.

Both formats contain 9 columns, which are tab-separated.  

Column 1 contains the seqID, which gives the name of the sequence.

Column 2 contains the source, which is the procedure that generated the feature.

Column 3 contains the type of feature, such as 'gene' or 'exon.'

Column 4 contains the start position of the feature, which is 1-based (different from BED format).

Column 5 contains the stop position of the feature.

Column 6 contains the score, which gives the confidence from the source in the annotated feature.

Column 7 contains the strand, which can be '+'  '-' or '?' for an unknown orientation. 

Column 8 contains the phase, which will be 0, 1 or 2 for CDS features or '.' for anything else.

Column 9 contains attributes, which are semicolon separated and provide additional information about the feature. The format and information contained in this column differs between GFF3 and GTF formats.

For example, a GFF file may look like this:
```
NC_054069.1     RefSeq  region  1       8214878 .       +       .       ID=NC_054069.1:1..8214878;Dbxref=taxon:148594;Name=16;chromosome=16;collected-by=Diego Rubolini;collection-date=2016-06-25;country=Italy: Matera;dev-stage=juvenile;gbkey=Src;genome=chromosome;isolate=bFalNau1;lat-lon=40.67 N 16.60 E;mol_type=genomic DNA;sex=female;specimen-voucher=P51P2 - H187058 (nest 2016-P51);tissue-type=blood
```

## VCF
[VCF format](https://en.wikipedia.org/wiki/Variant_Call_Format) is a text file in which each entry is composed of one line and is used for storing sequence variation. 

A VCF file generally starts with a header that provides metadata describing the content of the file, which is denoted by starting with #  and special keywords denoted by ##. 

After the header follows the body of the VCF file, which contains a mandatory 8 columns, and unlimited optional columns.

Column 1 contains the name of the sequence where the variant is located, usually the chromosome.

Column 2 contains the position of the variant, which is 1-based.

Column 3 contains the identifier of the variant, which will be '.' if unknown.

Column 4 contains the reference sequence at the variant position.

Column 5 contains a list of alternative alleles at the variant position. 

Column 6 contains the quality score associated with the inference of the given alleles.

Column 7 contains a flag giving information on filters that the variant has failed to pass, or PASS if the variant has passed filters.

Column 8 contains a list of fields with information describing the variant. The fields may vary depending upon the method of variant detection, with fields separated by semicolons.

Column 9 is optional, but included if there are sample columns included in the VCF.  This column provides a list of fields describing the information contained in the samples.

After column 9 is an unlimited number of columns describing the samples described in the file.

For example, a VCF file including one sample:
```
NC_054069.1     86611   521906:71:-     A       G       .       PASS    NS=93;AF=0.038;SF=0,1   GT:DP:AD:GQ:GL  0/0:49:49,0:40:-0,-15.74,-125.17
```

## Adapted
[Adapted from the Evomics Workshop](https://sites.google.com/view/wg2023unix/bio-info-files?authuser=0)