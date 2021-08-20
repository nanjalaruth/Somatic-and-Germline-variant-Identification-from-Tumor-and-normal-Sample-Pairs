# Identification of somatic and germline variants from tumor and normal sample pairs

## Introduction




![Graphical Abstract](Graphic_Abstract-Genomics-Two-A.png)

This workflow was produced both on the Galaxy platform as well as a Linux Pipeline

# Section one:  `GALAXY WORKFLOW`

<Lets add the galaxy sections here>

 ---
 
# Section Two: `Linux Pipeline`

<Lets add the Linux Section here>
 ## **Mapped read postprocessing**
 After mapping of the sample sequences against the reference genome with the aim of determining the most likey source of the observed sequencing read. A sAM(sequence alignment/map)format output is generated. The file has a single unified format for storing read alignments to a reference genome.
 # Conversion of the SAM file to BAM file
 A BAM(Binary Alignment/Map) format is an equivalent to sam but its developed for fast processing and indexing. It stores every read base, base quality and uses a single conventional technique for all types of data.
 The commands :** samtools view -O BAM SLGFSK35.sam -o SLGFSK55.bam and samtools view -O BAM SLGFSK36.sam -o SLGFSK56.bam** was used to produce the BAM file output for subsequent step analysis.
 
 


--- 
##  List of team members according to the environment used:

1. Galaxy Workflow:
- @Rachael 
- @Mercy
- @Orinda
- @Heshica
- @Kauthar
- @VioletNwoke
- @AmaraA
- **@Amarachukwu - Gemini annotate(CGI) and Gemini query**
- @Mallika
- @Olamide 
- @Marvellous
- @NadaaHussienn
- @Christabel

2. Linux Workflow
- @Praise 
- @Fredrick
- @RuthMoraa
- @Kauthar
- @Gladys
- @Nanje


