# Identification of somatic and germline variants from tumor and normal sample pairs

## Introduction




![Graphical Abstract](Graphic_Abstract-Genomics-Two-A.png)

This tutorial was reproduced on the Galaxy platform and with Linux Pipeline

# Section one:  `GALAXY WORKFLOW`

<Lets add the galaxy sections here>

## Data Preparation:

The sequencing reads that were used for analysis were obtained from a cancer patient's normal and tumor tissues.
There were a total of four samples. A forward reads sample and a reverse reads sample was obtained for both the normal and tumor tissue. A human reference genome, hg19 version was also used for analysis.

The first step is to create a new history and rename it in the galaxy window. To import files via links, click on the 'Upload' button on the left side toolbar and select 'Paste/Fetch Data'. Paste the link of your samples here and select datatype as 'fastqsanger.gz'. Click on Start and close the import window.  For the reference genome, paste the link to the file and select the datatype a 'fasta'.

Once the files are uploaded in the history, click on the pencil icon to edit its attributes. Change the names of the samples such that we can easily differentiate normal tissue samples from tumorous tissue samples. Click on save changes and then click on the renamed dataset. Use the 'Edit Dataset tags' option to add tags to your sample using # before the tag name. This step makes it easier to keep a track of the analysis.

##  Quality Control & Check:

•	FastQC:  is a quality control tool for high throughput sequence data that gives a summary report about the sequence.

•	MultiQC: A modular tool to aggregate results from bioinformatics analyses across many samples into a single report.


The MultiQC Output & Report:

For the quality control analysis, the following figures show that the chosen dataset is of high quality for both Normal R1 & R2 datasets and both Tumor R1 & R2 datasets even the tumor ones are of poorer quality than the normal ones:
o   All nucleotides have high-quality scores, (the forward and reverse reads of normal and tumor patient’s tissues), as they all are present in high/good quality region.

![Quality Report](https://scontent.fcai20-3.fna.fbcdn.net/v/t39.30808-6/239887479_10225646820098581_7852584000625137608_n.jpg?_nc_cat=103&ccb=1-5&_nc_sid=730e14&_nc_eui2=AeEGg5POsRQ22z30bSKJKOHMNyXKaVeIx-Q3JcppV4jH5H7955fLdsp5_S5xl6vVh4A&_nc_ohc=qsVI3WWfM6MAX-9V1fz&_nc_ht=scontent.fcai20-3.fna&oh=0f87104f5f97008322df582eea23119f&oe=61249517)

 o	Good quality score distribution as the mean is high with a sharp, distinct peak.

![Quality Report](https://scontent.fcai20-3.fna.fbcdn.net/v/t39.30808-6/240386199_10225646820338587_8217938469652979046_n.jpg?_nc_cat=107&ccb=1-5&_nc_sid=730e14&_nc_eui2=AeHfYmVb7PRpPMCZZSub8LeiagTNa3_Pz8lqBM1rf8_PyQOIPyzQKNJrrF5grXk5bpU&_nc_ohc=G1o6LS9X_hYAX-478Tr&_nc_ht=scontent.fcai20-3.fna&oh=e2a9dbde12186e84f979a5391d58fa84&oe=6124BBAD)

o	The actual mean of the GC% content is lower than the theoretical one and that is a non-normal distribution which may indicate some contamination; however, this peculiar bimodal distribution is considered to be a hallmark of the captured method as using Agilent’s SureSelect V5 technology for exome enrichment.

![Quality Report](https://scontent.fcai20-3.fna.fbcdn.net/v/t39.30808-6/240332287_10225646820538592_1031475029124752916_n.jpg?_nc_cat=103&ccb=1-5&_nc_sid=730e14&_nc_eui2=AeF1xpsEQ-0f3Eb3Mqxtc-Npz8gRJUkIxxLPyBElSQjHEp63raX5oUlFIBOhFqte0o4&_nc_ohc=3X7mee3XrsMAX8VAWOt&_nc_ht=scontent.fcai20-3.fna&oh=dadab37eda7bc7095539a18260709433&oe=61251953)

o	The N content is zero, thus not indicating any bad base detection.

![Quality Report](https://scontent.fcai20-3.fna.fbcdn.net/v/t39.30808-6/239932544_10225646821178608_1804771247842536342_n.jpg?_nc_cat=111&ccb=1-5&_nc_sid=730e14&_nc_eui2=AeFJ5LZYvlO_zgrhPl4Qvqy0RC3ghuo8qpxELeCG6jyqnLVOoaNAXDmrf5lC-FTyqoA&_nc_ohc=TsZziKppm1gAX8l1FUW&tn=D8rHGaIJCwa8Og3I&_nc_ht=scontent.fcai20-3.fna&oh=3753222c1f2642ed1b8f013d8ae71218&oe=6123FF0F)

o	Sequence duplication level is good as well with almost no PCR biases when library was prepared as there is no over-amplified fragment.

![Quality Report](https://scontent.fcai20-3.fna.fbcdn.net/v/t39.30808-6/240452151_10225646820978603_4722043033758089712_n.jpg?_nc_cat=103&ccb=1-5&_nc_sid=730e14&_nc_eui2=AeFO1KjcfjwGSAM-xaMakh3a9bVbKhLaQoT1tVsqEtpChIiRWFZIA79Wijx9qGdCRHA&_nc_ohc=EVOa3QcTTFUAX-an6Ea&tn=D8rHGaIJCwa8Og3I&_nc_ht=scontent.fcai20-3.fna&oh=15a70d18a95a3ab36bec20f483ece380&oe=612406E1)

o	There is a little presence of adaptors in the sequence.

![Quality Report](https://scontent.fcai20-3.fna.fbcdn.net/v/t39.30808-6/240066890_10225646821418614_9064760851692879195_n.jpg?_nc_cat=107&ccb=1-5&_nc_sid=730e14&_nc_eui2=AeHvZIsBRpPCCRbJELOa_nlbsKsWUcxpgYmwqxZRzGmBid0AqMMgD3B6gqZWkCywp5c&_nc_ohc=fjLSS02SO9wAX9MNlaP&_nc_ht=scontent.fcai20-3.fna&oh=4fa0d302f28428aa9f67700fa89854d6&oe=612565FC)



## Read Trimming and Filtering

The next process after quality control is Trimming and Filtering. This process further helps to trim and filter raw reads to give a more improved quality.
° Tool - TRIMMOMATIC (Galaxy Version 0. 38. 0) is a fast, multithreaded command line tool that can be used to trim and crop low quality reads and remove the present adaptors in the reads to improve the quality of these processing datasets.

° Input data - The forward read FASTQ file (r1) and the reverse read FASTQ file (r2) of the normal tissue is run concurrently as paired-end by performing initial ILLUMINACLIP step.

° Process - Under the ILLUMINACLIP step, the adapter is set to standard and "TruSeq3 (paired-ended, for MiSeq and HiSeq)" set as adapter sequence to use.
The maximum mismatch count is set at "2".
The accuracy between two adapter ligated is set at "30".
The accuracy between any adapter is set as "10".
The minimum length of adapter to be detected is set as "8".
We select "yes" to always keep both reads.
These parameters are used to cut Illumina-specific adapter sequences from the reads.

° In performing the Trimmomatic Operation, three steps are involved.

In the first Trimmomatic operation, "Cut the specified number of bases from the start of the read (HEADCROP)" is selected as the Trimmomatic operation to perform.
"3" is set as Number of bases to remove from the start of the read.

In the second Trimmomatic operation, "Cut bases off the end of a read, if below a threshold quality (TRAILING)" is selected as the Trimmomatic operation to perform.
"10" is set as Minimum quality required to keep a base.

In the third Trimmomatic operation, "Drop reads below a specified length (MINLEN)" is selected as the Trimmomatic operation to perform.
"25" is set as minimum quality required to keep a base

We run this and generate four datasets. Two of the data sets are the trimmed forward and reverse reads of the normal tissue and the other two are empty (which should be deleted)

The process is repeated for the forward read FASTQ file (r1) and reverse read FASTQ file (r2) of the tumor tissue.



° The MultiQC Output & Report after Trimming:

The quality reads did not change much as the datasets were already of high quality although a small fraction of adapter is successfully removed as shown in the following report:

![Quality Report](https://scontent.fcai20-3.fna.fbcdn.net/v/t39.30808-6/239936305_10225646826698746_7439285328275684759_n.jpg?_nc_cat=104&ccb=1-5&_nc_sid=730e14&_nc_eui2=AeEHowE9hJvIdP00B3bXEG5QGNS2Mrs4sTEY1LYyuzixMaJmAJ4zP9daBQiwILofdJ8&_nc_ohc=tUKGyhg8GkgAX8GLGoh&_nc_ht=scontent.fcai20-3.fna&oh=9a6524fc18a614b7d92f3cd04231ef24&oe=6124AFE5)

![Quality Report](https://scontent.fcai20-3.fna.fbcdn.net/v/t39.30808-6/240397072_10225646826898751_7144027909249758575_n.jpg?_nc_cat=100&ccb=1-5&_nc_sid=730e14&_nc_eui2=AeGiMN88ld5QT1-kVw42WldImmCThDU_8KmaYJOENT_wqY4BeSIlAskfw6yiEsQrkqw&_nc_ohc=GsLoATZazrIAX9uuOWd&_nc_oc=AQkvTp0FpyMonETpOoyRdYs3gDhNhZRgJJY5mRPVZ5ZKlSDkl5-VHa_qi5bhfVcQDxM&tn=D8rHGaIJCwa8Og3I&_nc_ht=scontent.fcai20-3.fna&oh=215b57e346986a002f0d71d5ac061d18&oe=6124CD76)

![Quality Report](https://scontent.fcai20-3.fna.fbcdn.net/v/t39.30808-6/240390881_10225646827178758_2588812202487255242_n.jpg?_nc_cat=110&ccb=1-5&_nc_sid=730e14&_nc_eui2=AeH6iYY4sXoq-PtVR8WdrtGfKvLEkz25rGwq8sSTPbmsbI0W9PNZYH6qo6HWtw4s8wU&_nc_ohc=zSrgQ62Q78EAX-Y-u1r&_nc_ht=scontent.fcai20-3.fna&oh=4cf0962b0311512e5bc0348ebc787e5a&oe=61254AF6)

![Quality Report](https://scontent.fcai20-3.fna.fbcdn.net/v/t39.30808-6/240169098_10225646827578768_8014971172963904116_n.jpg?_nc_cat=104&ccb=1-5&_nc_sid=730e14&_nc_eui2=AeGFQKCezKwD92wndqrvfaeDG5H1YPTmNIwbkfVg9OY0jPmwsfygta-ncYjVKHNB7hY&_nc_ohc=D2HaXxam1p4AX9Fajg2&_nc_ht=scontent.fcai20-3.fna&oh=3a60075320e6cdaffa5a4d466a2aa254&oe=612423E1)

![Quality Report](https://scontent.fcai20-3.fna.fbcdn.net/v/t39.30808-6/239987436_10225646827778773_1891502870608261843_n.jpg?_nc_cat=105&ccb=1-5&_nc_sid=730e14&_nc_eui2=AeGnbdwBY8IClJwm7Pt5IMyEgm4s8KvpWKSCbizwq-lYpEBMKxp3jTzE1uCc4A4hPZU&_nc_ohc=dNsRndgpP9cAX_E12j8&_nc_ht=scontent.fcai20-3.fna&oh=f8ef18b98ef5f7e9300616fe67585935&oe=6124D5BA)

![Quality Report](https://scontent.fcai20-3.fna.fbcdn.net/v/t39.30808-6/239939775_10225646828178783_4978182995878171958_n.jpg?_nc_cat=102&ccb=1-5&_nc_sid=730e14&_nc_eui2=AeEbSPFnKNZSQ1lwLKiEAJN7PU9sMEthmiU9T2wwS2GaJaw4KG3CdEHZIDmkt9PEA10&_nc_ohc=LCgTxenAqUQAX8ba8w4&_nc_ht=scontent.fcai20-3.fna&oh=fe03893345cfc2a4f0382c2c40544e91&oe=6125378E)

	
## Read Mapping
Once the sequence reads have been filtered and trimmed, read mapping or alignment is the next step in the bioinformatic pipeline. Read mapping is the process of aligning a set of reads to a reference genome to determine their specific genomic location. Several tools are available for read mapping but BWA-MEM (Galaxy Version 0.7.17.2) was used for our analysis as it is faster, accurate and supports paired-end reads. Read mapping was performed independently for the normal and tumor tissue using thesame parameters except otherwise stated.

Paramaters
- Locally cached human hg19 reference genome, Paired end reads, Forward and reverse trimmed reads (output of trimmomatic), Set read groups (SAM/BAM specification), auto-assign (no), Read Group Identifier (231335 for normal tissue and 231336 for tumor tissue), Read group sample name (normal for normal tissue and tumor for tumor tissue),
- For parameters not listed, default setting was used.

## Mapped Read Postprocessing

### Duplicate Reads Removal
Tool:![](https://i.imgur.com/OPq6wgU.png)

#### Significance
RmDup is a tool that identifies PCR duplicates by identifying pairs of reads where multiple reads align to the same exact start position in the genome. PCR duplicates arise from multiple PCR products from the same template binding on the flow cell.These are usually removed because they can lead to false positives<br>The read pair with the highest mapping quality score is kept and the other pairs are discarded.<br>It is important to note that this tool does not work for unpaired reads(in paired end mode) or reads that would pair where each maps to different chromosomes.<br>Note: It does not work for unpaired reads.<br>We used filtered reads datasets(BAM file) from the normal and the tumor tissue data- *outputs of Filter BAM datasets on a variety of attributes* .<br>We run RmDup on the following parameters:![](https://i.imgur.com/b2IeqaC.png)and ![](https://i.imgur.com/3m1NMRc.png)The result was two new datasets in BAM format.The duplicate rate for both sets was well below 10% which is considered good.The tool standard error reflected the results below of unmatched pairs on chr5 and chr12 that otherwise were not included in the output data!

	

#Left-align reads around indels

The first Step in this is running the BamLeftAlign tool from the Tools set available on Galaxy. Then we have chosen the source for the reference genome as Locally cached and selected the filtered and dedicated reads datasets from the normal and the tumor tissue data which were the outputs of RmDup. Then we used Human: hg19 aa the genome reference  and set the maximum number of iterations as 5, keeping all other settings as default and finally this will generate two new datasets, that is,one for each of the normal and tumor data.

#Recalibrate read mapping qualities

The next step after Left aligning the reads around indels is  Recalibrating the read mapping qualities.

•RECALIBRATE READ QUALITY SCORES :
The first Step in Recalibrating read mapping qualities is running CalMD tool from Galaxy tool set. Firstly we have selected the left-aligned datasets from the normal and the tumor tissue data; the outputs of BamLeftAlign tool as the input for the BAM file to recalculate. Them we chose the source of reference genome as Use a built in genome as the required hg 19 reference genome was already in built in the Galaxy version we were using. We chose Advanced options as the choice for Additional options and we selected 50 as the Coefficient to cap the mapping quality of poorly mapped reads. And finally this step would produce two new datasets, that is one for each of the normal and tumor data.

#Refilter reads based on mapping quality

Eliminating reads with undefined mapping quality
We ran Filter BAM datasets on a variety of attributes tool using some parameters.          The  recalibrated datasets from the normal and the tumor tissue data which were the outputs of CalMD were selected as the BAM datasets to filter. Then we applied certain conditions as the options , in Filter, we selected the MapQuality as the BAM property to Filter. Then set the value of less than or equal to 254 (<=254) as the Filter on read mapping quality (phred scale).

## Variant Calling and Classification using VarScan Somatic

The purpose of this step is to use the tool "VarScan somatic" to detect variant alleles in tumor or normal sample pairs, call sample genotypes at variant sites, as well as classify variants into germline, somatic and LOH event variants using solid classical statistics even in the presence of non-pure samples like those obtained from biopsies.

After generating high quality set of mapped read pairs, we ran the "VarScan Somatic" with some parameters using the Human: h19 genome as our reference genome.
The mapped and filtered CalMD outputs of the normal and tumor tissue datasets were aligned to be read, and the estimated purity content for the normal and tumor samples were set to 1 and 0.5 respectively. Then we customized the settings for variant calling and set the "Minimum Base Quality" to 28. This was done to increase the base quality of our sequence data without throwing away a significant portion of the data. 

The "Minimum Mapping Quality" was then assigned to 1, in order to filter our sequence reads with a mapping quality of at least one as CalMD might have lowered some mapping qualities to zero.

We set the "Settings for Posterior Variant Filtering" as default values as well as leaving other settings to their default values before executing. The result of this was an oufput file in a VCF format.


## Adding genetic and clinical evidence_based annotation : Creating a GEMINI database for variants

The next step after adding functional annotation to the called variants was to add "genetic and clinical based annotations"
The processes of the step will help observe more information on the variants like: the clinical and genetic aspects, prevalence in the population and the frequency of occurency.
Firstly, the output from function annotation was loaded into the GEMINI database so as to create a Gemini database where further annotations can be effectively carried
out.
The following optional contents of the variant were loaded as well: GERP scores- these are scores gotten from the constrained elements in multiple alignments by
quantifying the substitution deficits, CAAD scores, [N:B-high CAAD and GERP scores were observed in all pathogenicity components], gene tables, sample genotypes and
variant INFO fields.
These loaded variants in the gemini database are then annoatated by GEMINI annotate; this tool adds more explicit information on the output of VarScan that could not be indentified by Gemini load.

## Making variant call statistics accessible

Hence, we used Gemini annotate to extract three values : Somatic Status(SS), Germline p-value (GPV) and Somatic p-value(SPV) from the info generated by VarScan and added them to the Gemini database.
In order to crosscheck if all information extracted by the GEMINI database are in relation to variants observed in the population, we decided to annote by adding more information from the Single Nucleotide Polymorphism Database(dbSNP), also from Cancer Hotspots, links to CIViC database as well as more information from the Cancer Genome Interpreter (CGI)

For dbSNP: the last output from Gemini annotate was annotated with the imported dbSNP using the Gemini annotate tool. This process extrated dbSNP SNP Allele Origin (SAO) and adds it as "rs_ss" column to the existing database.

For Cancerhotspots: the last ouput generated from annotating dbSNP information was then annotated using GEMINI annotate tool, using the imported cancer hotspots as annotation source to extract "q-values" of overlapping cancerhotspots and add them as "hs_qvalue" column to the existing database.

Links to CIVic: the output of the last Gemini annoate for cancerhotspots was annotated using the imported CIViC bed as annotation source. We extracted 4 elements from this source and again added them as a list of "overlapping_civic_urls" to the existing Gemini database.

For the Cancer Genome Interpreter: the last output with the extracted infomation linking to CIViC was further annotated using the tool Gemini annotate with the imported CGI variants as an annotation source. the information extracted was recorded in the Gemini database as "in_cgidb" being used as the column name.


 ## Reporting Selected Subsets of Variants with GEMINI Query

GEMINI query syntax is built on the SQLite dialect of SQL. This query language enables users express different ideas when exploring variant datasets, for this analysis four (4) stepwise GEMINI queries were carried out.

1.	A query to obtain the report of bona fide somatic variants

“GEMINI database”: the fully annotated database created in the last GEMINI annotate step i.e., Cancer Genome Interpreter(CGI)

“Build GEMINI query using”: *Basic variant query constructor*

“Insert Genotype filter expression”: *gt_alt_freqs.NORMAL <= 0.05 AND gt_alt_freqs.TUMOR >= 0.10*

This genotype filter aims to read only variants that are supported by less than 5% of the normal sample, but more than 10% of the tumor sample reads collectively

 “Additional constraints expressed in SQL syntax”: *somatic_status = 2*

This somatic status called by VarScan somatic is one of the information stored in the GEMINI database.

By default, the report of this run would be output in tabular format; and a column header is added to it output.

The following columns were selected

*	“chrom”
* “start”
*	“ref”
*	“alt”

*	“Additional columns (comma-separated)”: *gene, aa_change, rs_ids, hs_qvalue, cosmic_ids*
 These columns are gotten from the variants table of the GEMINI database.

2.	This second step has the same settings as the above step except for:

*	“Additional constraints expressed in SQL syntax”: *somatic_status = 2 AND somatic_p <= 0.05 AND filter IS NULL*

3.	Run GEMINI query with same settings as step two, excepting:

*	In “Output format options”

“Additional columns (comma-separated)”: *type, gt_alt_freqs.TUMOR, gt_alt_freqs.NORMAL, ifnull(nullif(round(max_aaf_all,2),-1.0),0) AS MAF, gene, impact_so, aa_change, ifnull(round(cadd_scaled,2),'.') AS cadd_scaled, round(gerp_bp_score,2) AS gerp_bp, ifnull(round(gerp_element_pval,2),'.') AS gerp_element_pval, ifnull(round(hs_qvalue,2), '.') AS hs_qvalue, in_omim, ifnull(clinvar_sig,'.') AS clinvar_sig, ifnull(clinvar_disease_name,'.') AS clinvar_disease_name, ifnull(rs_ids,'.') AS dbsnp_ids, rs_ss, ifnull(cosmic_ids,'.') AS cosmic_ids, ifnull(overlapping_civic_url,'.') AS overlapping_civic_url, in_cgidb*

## Generating Reports of Genes Affected by Variants

In this step, gene-centred report is generated based on the same somatic variants we selected above.
As in the previous step we run GEMINI query but in advanced mode

•	“Build GEMINI query using”: *Advanced query constructor*

•	“The query to be issued to the database”: *SELECT v.gene, v.chrom, g.synonym, g.hgnc_id, g.entrez_id, g.rvis_pct, v.clinvar_gene_phenotype FROM variants v, gene_detailed g WHERE v.chrom = g.chrom AND v.gene = g.gene AND v.somatic_status = 2 AND v.somatic_p <= 0.05 AND v.filter IS NULL GROUP BY g.gene*

However the “Genotype filter expression”: *gt_alt_freqs.NORMAL <= 0.05 AND gt_alt_freqs.TUMOR >= 0.10* remains the same








## Adding additional Annotation to the Gene-Centered Report
The aim of including extra annotations to the GEMINI-generated gene report (that is, the output of the last GEMINI query) is to make interpreting the final output easier. While GEMINI-annotate allowed us to add specific columns to the table of the database we created, it does not allow us to include additional annotations into the tabular gene report.

By simply using the Join two files tools on Galaxy, this task was  achieved. After which, irrelevant columns were removed by specifying the columns that are needed. Three step wise process were involved here: One, we pulled the annotations found in Uniprot cancer genes dataset; second, we used the output of the last Join operation, annotated the newly formed gene-centered report with the CGI biomarkers datasets; and three, we used the output of the second Join operation, add the Gene Summaries dataset. Lastly, we ran Column arrange by header name to rearrange the fully-annotated gene-centered report and eliminate unspecified columns.

The last output of the Join operation was selected in the “file to arrange” section. The columns to be specified by name are: gene, chrom, synonym, hgnc_id, entrez_id, rvis_pct, is_TS, in_cgi_biomarkers, clinvar_gene_phenotype, gene_civic_url, and description. The result gotten was a tabular gene report which was easy to understand and interpret.


# Section Two: `Linux Pipeline`

<Lets add the Linux Section here>

## Dataset Description

The datasets used in this analysis were gotten from a real-world data from a cancer patient’s tumor and normal tissue samples.The datasets only includes reads from human chromosomes 5, 12 and 17. The tumor tissue and human genome couldn't be used only to identify somatic and germline variant in tumor cells. This is because healthy tissue contains many thousands of variants compared to the reference genome and every individual inherits a unique pattern of many variants from their parents. Four data/sequences were used in this analysis. Two were from normal tissue and the other were from  tumor tissue. The data are in pairs of forward and reverse reads sequence.

The datasets were downloaded from Zenodo using the wget command and the link to the data.
For example, for the forward read sequence for the normal tissue, the command used to download the data to the linux environment is:
```
wget https://zenodo.org/record/2582555/files/SLGFSK-N_231335_r1_chr5_12_17.fastq.gz
```
## Pre-processing and Trimming

i) Quality check.

The quality of the reads was examined using fastqc and an aggregate report of all the fastqc reports generated by multiqc.

```
#Qc on reads
mkdir Fastqc
fastqc *.fastq.gz -o Fastqc -t 8

mkdir Multiqc
multiqc Fastqc -o Multiqc
```

The multiqc report can be examined from [here](multiqc_report_linux.html). From the report we can see the quality of the reads are good however there are few adapters observed.

ii) Trimming with trimmomatic
	
The reads were trimmed with Trimmomatic with the parameters shown below to remove adapter sequences and improve the quality of reads.

```
for i in `ls -1 *r1_chr5_12_17.fastq.gz | sed 's/\_r1_chr5_12_17.fastq.gz//'`
do
trimmomatic PE \
-phred33 \
$i\_r1_chr5_12_17.fastq.gz \
$i\_r2_chr5_12_17.fastq.gz \
$i\_r1_chr5_12_17_paired.fastq.gz \
$i\_r1_chr5_12_17_unpaired.fastq.gz \
$i\_r2_chr5_12_17_paired.fastq.gz \
$i\_r2_chr5_12_17_unpaired.fastq.gz \
ILLUMINACLIP:TruSeq3-PE.fa:2:10:8 HEADCROP:3 TRAILING:10 MINLEN:25
done
```

iii) Post trimming Quality Control.

An assesement of the quality of the trimmed reads was carried out using Fastqc and Multiqc.

```
#Post trimming qc
mkdir Post_trim_fastqc
fastqc *_paired.fastq.gz -o Post_trim_fastqc -t 8

mkdir  Post_trim_multiqc
multiqc Post_trim_fastqc -o Post_trim_multiqc
```

The post trimming multiqc report can be found [here](post_trim_multiqc_report_linux.html). It is evident from the report that the quality of the reads improved having per base quality scores above 35 and no adapters observed. After trimming an average of 0.73% normal reads and 1.24% tumor reads were lost.

**NB: To view the multiqc html reports download the files and view them from your browser.**

## Mapped read postprocessing
 After mapping of the sample sequences against the reference genome with the aim of determining the most likey source of the observed sequencing read. A sAM(sequence   alignment/map)format output is generated. The file has a single unified format for storing read alignments to a reference genome.

### Conversion of the SAM file to BAM file
A BAM(Binary Alignment/Map) format is an equivalent to sam but its developed for fast processing and indexing. It stores every read base, base quality and uses a single conventional technique for all types of data.
The commands : **samtools view -O BAM SLGFSK35.sam -o SLGFSK55.bam and samtools view -O BAM SLGFSK36.sam -o SLGFSK56.bam was used to produce the BAM file output for subsequent step analysis.**

### Sorting and indexing of the BAM file
The produced BAM files were sorted by read name and indexing was done for faster or rapid  retrieval.
The commands: **samtools sort -T temp -O bam -o SLGFSK35.sorted.bam SLGFSK35.bam and samtools sort -T temp -O bam -o SLGFSK36.sorted.bam SLGFSK36.bam** to perform sorting. Indexing of the sorted files was done using the **samtools index  SLGFSK35.sorted.bam  and SLGFSK36.sorted.bam.**  At the end of the every BAM file,  a special end of file (EOF) marker is usually written, the samtools index command also checks for this and produces an error message if its not found.

### Mapped reads filtering
To filter for the mapped reads the command **samtools view -b -F SLGFSK35.bam >SLGFSK35.bam and samtools view -b -F SLGFSK36.bam >SLGFSK36.bam.**
Their were 21200965 mapped reads for  SLGFSK35.bam  and 32643682 mapped reads for SLGFSK36.bam

### Duplicates removal
During library construction sometimes there's introductio of PCR (Polymerase Chain Reaction) duplicates, these duplicates usually can result in false SNPs (Single Nucleotide Polymorphisms), whereby the can manifest themselves as high read depth support. A low number of duplicates (<5%) in good libraries is considered standard.
The commands **samtools rmdup SLGFSK35.sorted.bam  SLGFSK35.rdup and samtools rmdup SLGFSK36.sorted.bam  SLGFSK36.rdup.**  



---
##  List of team members according to the environment used:

1. Galaxy Workflow:
- @Rachael
- @Mercy
- @Orinda
- @Heshica
- @Kauthar
- @VioletNwoke - Read mapping [Link to galaxy workflow] (https://usegalaxy.eu/u/violet/w/workflow-constructed-from-history-hackbiogenomicstwoaviolet-4)
- @AmaraA
- @Amarachukwu -Gemini query
- @Mallika
- @Olamide - Read Trimming and Filtering
- @NadaaHussienn - Quality Control and Check
- @Christabel
- @Marvellous  


2. Linux Workflow
- @Praise
- @Fredrick
- @RuthMoraa
- @Kauthar - Preprocessing(pre/post trim qc) and read trimming.
- @Gladys
- @Nanje
