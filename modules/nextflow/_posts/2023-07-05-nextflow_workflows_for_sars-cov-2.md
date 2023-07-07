---
title: Nextflow workflows for SARS-CoV-2
---

### Running pre-built Nextflow workflows

Nextflow supports fetching workflows from Github repositories. Some commonly used ones include:

#### EPI2ME Labs wf-artic

The EPI2ME Labs wf-artic is a workflow from EPI2ME, a subsidiary of Oxford Nanopore, for analysis of SARS-CoV-2 amplicon sequencing data. It specifically addresses
an [issue](https://labs.epi2me.io/sarscov2-midnight-analysis/) with the original ARTIC fieldbioinformatics protocol caused by the tagmentation approach of the ONT Rapid library preparation kit. It is assumes that data is organised in the way it is produced by an Oxford Nanopore sequencer, i.e. directories containing multiple FASTQ files, with optional subdirectories for different samples barcodes. For example:

```
fastq_pass\
            barcode1\
                      file1.fastq
                      file2.fastq
            barcode2\
                      file1.fastq
                      file2.fastq
```

A sample sheet can be provided in CSV format associating the name of the samples with barcodes and also identifying whether a sample is a `test_samples`, `negative_control`, `positive_control` or `no_template_control`. Here is a example of what such a samplesheet looks like:

```csv
barcode,sample_id,alias,type
sample1,hCoV19/South Africa/SMN691,SAMN23469691,test_sample
sample2,hCoV19/South Africa/SMN685,SAMN23469685,test_sample
sample3,negative,negative,negative_control
```

Note that the sample alias (the 3rd column) should not contain spaces or `/` characters and that is is the alias that will be used as the sample name in workflow outputs. You can edit CSV files using a spreadsheet program like Microsoft Excel or [LibreOffice](https://www.libreoffice.org/) Calc.

The wf-artic workflow produces a HTML report along with its output files. An example can be seen [here](../uploads/wf-artic-report.html).


