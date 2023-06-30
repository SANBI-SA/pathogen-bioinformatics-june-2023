---
title: Workflows for SARS-CoV-2 Sequencing
---

### Workflows for SARS-CoV-2 amplicon sequencing

As previously described, SARS-CoV-2 is often sequenced using the amplicon sequencing method. This amplifies the
viral genome using two pools of PCR primers and identifies variation in the genome by mapping against a reference
genome. The reference genome used is typically the Wuhan Hu-1 sequence (MN908947.3 aka NC_045512.2). There
are workflows available for Galaxy for both Illumina and Oxford Nanopore data.

#### Importing a workflow into Galaxy

To import a workflow into your Galaxy account, select the _Workflow_ menu and click the _Import_ button in the top right. Then select
the _GA4GH servers_ tab and search for the SANBI workflows by entering _name:SANBI_ in the search box.

Select either the Illumina or ONT workflows and click on them to display the workflow details. Import the latest version of the workflow
that you are going to use.

#### Getting Reference Data for your workflow

You will need the SARS-CoV-2 reference genome (MN908947.3) and ARTIC v4 primers by coping this history:

[SARS-CoV-2 reference data](https://usegalaxy.eu/u/pvanheus/h/sars-cov-2-reference-data)

#### Running the SARS-CoV-2 Illumina workflow

Import the reads from the [Illumina Reads](https://usegalaxy.eu/u/pvanheus/h/sars-cov-2-sequencing-reads) history. Create a _list of pairs_ collection from these reads
by selecting all of the read files and using the _Build List of Dataset Pairs_ option. Then:

1. Select _Unpair all_ to un-pair the reads

2. Enter _\_1.fastq.gz_ as the filter on the left and _\_2.fastq.gz_ as the filter on the right.

3. Select _Auto-pair_ to pair the reads while trimming the read names.

4. Choose a name for your samples (e.g. Samples)

Once you have the reads organised into a list of pairs and you also have the _reference genome_ and _primer BED file_ in your history, you can run the 
SARS-CoV-2 Illumina Amplicon pipeline. Choose your sequence reads (e.g. the collection called _Samples_) as your _Paired read collection for samples_ input
and then choose the right datasets for the _Reference FASTA_ and _Primer BED_ files. Leave other parameters at their defaults and then _Run Workflow_

#### Running the SARS-CoV-2 Nanopore workflow

Import the reads from the [Oxford Nanopore](https://usegalaxy.eu/u/pvanheus/h/ont-sars-cov-2-kenya) history. Create a _list_ collection by selecting the 
all the read files and using the _Build Dataset List_ option. Give the list a name (e.g. _Samples_).

#### Interpreting the results

Coming Soon



