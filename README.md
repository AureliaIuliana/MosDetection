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














## To collocate
MosDetection requires specific user-provided parameters. The first one defines a threshold value to highlight parent's alternative allele percentage in SNV analysis. The second parameter serves to extend by a value the INDEL upstream and downstream base analysis to investigate depth sequencing distribution flanking variant location.
