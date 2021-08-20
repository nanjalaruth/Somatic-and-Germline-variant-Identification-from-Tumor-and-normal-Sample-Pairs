# Identification of somatic and germline variants from tumor and normal sample pairs

## Introduction




![Graphical Abstract](Graphic_Abstract-Genomics-Two-A.png)

This workflow was produced both on the Galaxy platform as well as a Linux Pipeline

# Section one:  `GALAXY WORKFLOW`

<Lets add the galaxy sections here>

 ---
 
# Section Two: `Linux Pipeline`

<Lets add the Linux Section here>


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
- @Olamide ``
- @NadaaHussienn
- @Christabel
- @Marvellous  
 **ADDING ADDITIONAL ANNOTATION TO THE GENE-CENTERED REPORT:** 
The aim of including extra annotations to the GEMINI-generated gene report (that is, the output of the last GEMINI query) is to make interpreting the final output easier. While GEMINI-annotate allowed us to add specific columns to the table of the database we created, it does not allow us to include additional annotations into the tabular gene report. By simply using the Join two files tools on Galaxy, this task can be achieved. After which, irrelevant columns are removed by specifying the columns that are needed. Three step wise process are involved here: One, pull the annotations found in Uniprot cancer genes dataset; second, using the output of the last Join operation, annotate the newly formed gene-centered report with the CGI biomarkers datasets; and three, using the output of the second Join operation, add the Gene Summaries dataset. Lastly, run Column arrange by header name to rearrange the fully-annotated gene-centered report and eliminate unspecified columns. The last output of the Join operation must be select in the “file to arrange” section. The columns to be specified by name are: gene, chrom, synonym, hgnc_id, entrez_id, rvis_pct, is_TS, in_cgi_biomarkers, clinvar_gene_phenotype, gene_civic_url, and description. The result will be a tabular gene rport that is easy to understand and interpret.

2. Linux Workflow
- @Praise 
- @Fredrick
- @RuthMoraa
- @Kauthar
- @Gladys
- @Nanje


