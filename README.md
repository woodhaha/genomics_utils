# genomics_utils
- [Kmer Diagnostics of De Novo Genome Assembly](#kmer-diagnostics-of-de-novo-genome-assembly)
- [Gene Name to ID](#gene-to-id)
- [Gene ID to Transcript ID](#id-to-transcript)
- [Process TCGA miRNASeq isoform quantifications](#process-tcga-mirnaseq-isoform-quantifications)

## Kmer Diagnostics of De Novo Genome Assembly
The script visualizes the kmer spectrum of the raw sequencing data to reveal potential QC problems such as sequencing bias, sequencing error rate, and contamination. It produces two interactive plots in a HTML file that can be opened by a browser such as Chrome.

Dependencies: khist.sh included in bbmap by JGI
https://sourceforge.net/projects/bbmap/

```
Usage: Rscript kmer_plot.r <path to bbmap khist.sh> <path to fastq> <output file>
```

![KSpectra](kmer_plot.png?raw=true)

Sources:
http://software.broadinstitute.org/allpaths-lg/blog/wp-content/uploads/2014/05/KmerSpectrumPrimer.pdf
https://jgi.doe.gov/data-and-tools/bbtools/bb-tools-user-guide/bbnorm-guide/

## Gene to ID

fillID.py: given a column of gene names, fills in a column of corresponding ensembl IDs.
```
Usage: python fillID.py <File name> <Column name of gene names> <Column name of gene IDs>
```

Dependencies: biomart, pandas, argparse

Example: geneTable.tsv

| Gene_Name     | Gene_ID        |
| ------------- |:-------------:|
| CDC42      |  |
| HULC     |      |
| HOTAIR |      |

```
python fillID.py geneTable.tsv Gene_Name Gene_ID
```

| Gene_Name     | Gene_ID      |
| ------------- |:-------------:|
| CDC42      | ENSG00000070831 |
| HULC       |  ENSG00000251164  |
| HOTAIR     |  ENSG00000228630    |

## ID to Transcript
Convert ID to Transcript ID

fillTR.py: given a column of ensemble gene IDs, fills in a column of corresponding cononical transcript IDs.

```
Usage: python fillTR.py <File name> <Column name of gene IDs> <Column name of transcript IDs>
```

Dependencies: biomart, pandas, argparse, multiprocessing, collections, re

Example: geneTable.tsv

| Gene_ID     | Transcript_ID   |
| ------------- |:-------------:|
| ENSG00000070831      |  |
| ENSG00000251164     |      |
| ENSG00000228630 |      |
```
python fillTR.py geneTable.tsv Gene_ID Transcript_ID
```

| Gene_ID     | Transcript_ID      |
| :-------------: |:-------------:|
| ENSG00000070831      | ENST00000344548 |
| ENSG00000251164       |  ENST00000503668  |
| ENSG00000228630     |  ENST00000414790    |


## Process TCGA miRNASeq isoform quantifications

process_isoform_quantification.py: get mature miRNA sequence expression from a folder of TCGA miRNASeq isoform quantification files (*mirbase21.isoforms.quantification.txt). Place and run this script in folder containing the TCGA miRNASeq isoform quantification files.

```
Usage: python process_isoform_quantification.py
```

Dependencies: glob, pandas, argparse, multiprocessing

Example:

|miRNA_ID|isoform_coords|read_count|RPM|cross-mapped|miRNA_region|
| :-------: |:--------:|:---------:|:--------:|:---------:|:--------:|
|hsa-let-7a-1|hg38:chr9:94175961-94175982:+|1|0.259630|N|mature,MIMAT0000062|
|hsa-let-7a-1|hg38:chr9:94175961-94175983:+|8|2.077043|N|mature,MIMAT0000062|
|hsa-let-7a-1|hg38:chr9:94175961-94175984:+|8|2.077043|N|mature,MIMAT0000062|
|hsa-let-7a-1|hg38:chr9:94175962-94175981:+|182|47.252722|N|mature,MIMAT0000062|
|hsa-let-7a-1|hg38:chr9:94175962-94175982:+|4753|1234.022998|N|mature,MIMAT0000062|
|hsa-let-7a-1|hg38:chr9:94175962-94175983:+|12037|3125.170382|N|mature,MIMAT0000062|
|hsa-let-7a-1|hg38:chr9:94175962-94175984:+|28516|7403.618728|N|mature,MIMAT0000062|
|hsa-let-7a-1|hg38:chr9:94175962-94175985:+|1728|448.641225|N|mature,MIMAT0000062|
|hsa-let-7a-1|hg38:chr9:94175962-94175986:+|147|38.165660|N|mature,MIMAT0000062|
```
python process_isoform_quantification.py
```
||file 2|file 3|
| :-------: |:--------:|:---------:|
|MIMAT0000062|26367|18700|20404|
|MIMAT0000063|14256|20311|8296|
|MIMAT0000064|20115|12265|9691|
|MIMAT0000065|472|563|519|
|MIMAT0000066|2402|1798|2309|
|MIMAT0000067|14404|7863|11078|
|MIMAT0000068|347|193|213|
|MIMAT0000069|625|543|713|
|MIMAT0000070|870|839|1354|
