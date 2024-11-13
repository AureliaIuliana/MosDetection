# Mosaic-Variants-Project

## Introduction 
MosDetection is a python script implemented to rapidly detect **potential mosaicism**, distinguishing inherited heterozygosity from de novo variants, based on read depth count and percentage of each nucleotide at a specific variant position.

We utilized [bam-readcount](https://github.com/genome/bam-readcount?tab=readme-ov-file) v1.0.1 to determine the count of each nucleotide in each variant position.

The script execution produces an Excel file composed of four sheets, dedicated to the analysis of SNVs, deletions, insertions, and DelIns being explored, respectively. 

## Dependencies

bam-readcount [installation](https://github.com/genome/bam-readcount?tab=readme-ov-file)
```
  Python  3.7.8
  openpyxl 3.0.9
  pandas 1.4.3
```

## Input
The input data comprises **BAM** files of the trios being studied, along with a **CSV** file that details the trio identifiers and pedigree, along with the coordinates of the variants. 

This is the ordered content of the CSV file: 

| Column | Description |
| ------| -----------|
| sampleID   | unique ID associated to each member of the family trio. |
| projectID | unique ID which associated each sample to a specific trio research project. |
| sampleType    | "I" for proband, "F" for father, "M" for mother. |
| geneSymbol | symbol of the gene where the variant is detected. |
| coordinates | genomic coordinates pinpointing the location of the variant along with the reference and alternative alleles. |
| BAMname| BAM name of the trio member. |

CSV example for SNV, deletion, insertion, and delins respectively: 
```
sampleID,projectID,sampleType,geneSymbol,coordinates,BAMname
2076R,p649,I,DYNC1H1,14:102461022:T/C,OPTI111
2077R,p649,F,DYNC1H1,14:102461022:T/C,OPTI112
2078R,p649,M,DYNC1H1,14:102461022:T/C,OPTI113
19N3158,p705,I,DYNC1H1,14:102452883:TTTACCCGT/-,OPTI135
19N3159,p705,F,DYNC1H1,14:102452883:TTTACCCGT/-,OPTI136
19N3160,p705,M,DYNC1H1,14:102452883:TTTACCCGT/-,OPTI137
19N2563,P133,I,TSC1,9:135787826:-/A,OPTIIH
19N2564,P133,F,TSC1,9:135787826:-/A,OPTIFH
19N2565,P133,M,TSC1,9:135787826:-/A,OPTIMH
20N0349,p714,I,COL4A1,13:110826811:CCTG/GCAC,OPTI141
20N0350,p714,F,COL4A1,13:110826811:CCTG/GCAC,OPTI142
20N0351,p714,M,COL4A1,13:110826811:CCTG/GCAC,OPTI143
```

To streamline access and analysis, we have stored the BAM files for all the trios we've examined into a single directory. 














## To collocate
MosDetection requires specific user-provided parameters. The first one defines a threshold value to highlight parent's alternative allele percentage in SNV analysis. The second parameter serves to extend by a value the INDEL upstream and downstream base analysis to investigate depth sequencing distribution flanking variant location.
