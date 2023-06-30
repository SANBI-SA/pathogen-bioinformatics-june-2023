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

#### Analysing your own data

To analyse your own data you need to:

1. Upload your data into a Galaxy history and organise it into a collection. See the notes in the [Galaxy Tips](galaxy-techniques/) on how to organise data into collections. Each sample should be in its own file.

2. Ensure that you have uploaded a copy of the SARS-CoV-2 MN908947 Reference Genome to your history. The link to upload is [https://www.ebi.ac.uk/ena/browser/api/fasta/MN908947.3?download=true](https://www.ebi.ac.uk/ena/browser/api/fasta/MN908947.3?download=true).

3. Ensure that you have the correct primer BED file that matches you sequencing primers. You can choose from the table below. Copy the
link and paste it into the Galaxy upload form. Before you upload the dataset choose _bed_ as your format because Galaxy often cannot automatically
detect that a file is in BED format and will assign it to _tabular_ format. If you find that your uploaded data is not in BED format, edit it 
(with the Pencil icon), go to the _Convert_ tab and change the datatype in the bottom section of the form (in the _New Type_ section, not the
_Target datatype_ section).


<table>
  <tr>
   <th>Amplicon Scheme
   </th>
   <th>Amplicon Size
   </th>
   <th>Notes
   </th>
  </tr>
  <tr>
   <td><a href="https://raw.githubusercontent.com/pha4ge/primer-schemes/main/sars-cov-2/artic/v3/scheme.bed">ARTIC v3</a>
   </td>
   <td>400
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td><a href="https://raw.githubusercontent.com/pha4ge/primer-schemes/main/sars-cov-2/artic/v4/scheme.bed">ARTIC v4</a>
   </td>
   <td>400
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td><a href="https://github.com/pha4ge/primer-schemes/blob/main/sars-cov-2/artic/v4.1/scheme.bed">ARTIC v4.1</a>
   </td>
   <td>400
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td><a href="https://raw.githubusercontent.com/pha4ge/primer-schemes/main/sars-cov-2/midnight/v1/scheme.bed">Midnight v1</a>
   </td>
   <td>1200
   </td>
   <td>ONT kit MRT001.10
   </td>
  </tr>
  <tr>
   <td><a href="https://github.com/pha4ge/primer-schemes/blob/main/sars-cov-2/midnight/v2/scheme.bed">Midnight v2</a>
   </td>
   <td>1200
   </td>
   <td>ONT kit MRT001.20
   </td>
  </tr>
  <tr>
   <td><a href="https://raw.githubusercontent.com/pha4ge/primer-schemes/main/sars-cov-2/midnight/ont-v3/scheme.bed">ONT Midnight v3</a>
   </td>
   <td>1200
   </td>
   <td>ONT kit MRT001.30
   </td>
  </tr>
</table>

If you are using Midnight primers for Oxford Nanopore data, the minimum size should be 150 and the maximum size 1200 (this is because the Nanopore
Midnight use the rapid library preparation chemistry and thus tagmentation, see [this post](https://labs.epi2me.io/sarscov2-midnight-analysis/)).
 


