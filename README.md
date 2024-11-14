# Mosaic-Variants-Project

## Introduction 
MosDetection is a python script implemented to rapidly detect **potential mosaicism in trios**, distinguishing inherited heterozygosity from de novo variants, based on read depth count and percentage of each nucleotide at a specific variant position.

We utilized [bam-readcount](https://github.com/genome/bam-readcount?tab=readme-ov-file) v1.0.1 to determine the count of each nucleotide in each variant position.

The purpose of the implemented script is to reprocess the output from [bam-readcount](https://github.com/genome/bam-readcount?tab=readme-ov-file), retaining only the relevant count data. Additionally, it incorporates supplementary information, including pedigree details, unique identifiers, and the gene symbol associated with each variant. This approach allows for faster detection, saving time compared to manual count analysis in IGV.

The script execution produces an Excel file composed of four sheets, dedicated to the analysis of **SNVs**, **deletions**, **insertions**, and **delins** being explored, respectively. 

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

## Analysis 

#### Script execution: 

```
python3 MosDetection.py bam-readcount referenceSequence MAPQ CSV BAMsDirectory baseAnalysisExtensionValue 
```
#### Parameters:
- **`bam-readcount`**: path to bam-readcount executable file 
- **`referenceSequence`**: path to reference sequence in fasta format 
- **`MAPQ`**: minimum mapping quality of reads used for counting
- **`CSV`**: CSV file with informations about trio identifiers and pedigree along with variant coordinates and bam files name
- **`BAMsDirectory`**: path to the directory containing all the BAMs to be investigated
- **`baseAnalysisExtensionValue`**: variant upstream and downstream analysis extension value (for INDEL analysis)
- **`thresholdValue`**: threshold value to highlight parent alternative allele percentage associated to potential mosaicism
- **`lowerThresholdValue`**: lower threshold value to highlight proband/parent alternative allele percentage associated to potential mosaicism
- **`upperThresholdValue`**: upper threshold value to highlight proband/parent alternative allele percentage associated to potential mosaicism


For each sample, _bam-readcount_ is executed by providing as parameters: 
* the hg19 reference genome in the fasta format 
* MAPQ
* the BAM file name associated to each individual
* the genomic coordinate of the variant. 


#### Notes:
Bam-readcount output is reprocessed. For each trio member, general information on genomic coordinates, sequencing depth, and the count of reads for each nucleotide and INDEL are captured. 

In the SNV/DELINS output is present the count and the percentage of abundance for each base type (A, C, G, T, N) at the variant position. In addition, the background noise is calculated to discriminate between false positive and potential cases of mosaicism.
We defined the background noise as the maximum percentage among bases that were neither reference nor alternative within all trio's members.
In the specific case of DELINS, we display only the aboundance related to the given position, since we assume is sufficient for mosaicism detection. 

In the INDEL-specific output are present 2 additional columns: the first displayed the count of the identified target INDEL at the given position, while the second the percentage of aboundance of the target INDEL. The deletion analysis sheet displays the counts in a column marked “DEL:-deletion”. Similarly, for insertions, the data appears under “INS:+insertion”. 

The four different Excel sheets are generated only when all variants type are available in the CSV file. 














## To collocate
MosDetection requires specific user-provided parameters. The first one defines a threshold value to highlight parent's alternative allele percentage in SNV analysis. The second parameter serves to extend by a value the INDEL upstream and downstream base analysis to investigate depth sequencing distribution flanking variant location.
