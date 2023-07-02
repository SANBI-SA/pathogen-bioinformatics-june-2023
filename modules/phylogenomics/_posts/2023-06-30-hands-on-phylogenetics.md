---
title: Hands on Phylogenetics With Galaxy and Microreact
---

### Introduction to Phylogenetics - Hands On

To investigate the relatedness of sequences and compute a phylogeny, we need:

* Consensus sequences from each of our samples. We can compute these from sequencing reads combined with the reference genome.

* An "outgroup" to root the phylogeny. This should be either something closely related to our sample of interest (e.g. _M. cannetti_ reference sequence if
we are working with _M. tuberculosis_ or an early outbreak sample like the Wuhan Hu-1 reference sequence for SARS-CoV-2)

The phylogenetic methods that we will be using most often are based on Maximum Likelihood methods of tree inference. Such methods used
the columns of a multiple sequence alignment as evidence for a particular tree typology. This genetic information is combined with
an evolution model to compute the likelihood that a particular tree can be generated from a set of sequences.

#### Inputs Required

To start this analysis, import this history:

[https://usegalaxy.eu/u/pvanheus/h/sars-cov-2-phylogeny---input-data](https://usegalaxy.eu/u/pvanheus/h/sars-cov-2-phylogeny---input-data)

It contains:

* Patient Samples: a list of 20 FASTA consensus sequences from samples taken from COVID-19 patients

* MN908947.3 - the Wuhan Hu1 reference sequence


#### Combine the Datasets and Compute a Multiple Sequence Alignment

Using the _Concatenate Datasets tail-to-head (cat)_ tool, select:

* MN908947.3 

and then _Insert Dataset_ and select

* Patient Samples collection

Then run the tool. Rename the generated output to _combined samples_.

#### Compute a Multiple Sequence Analysis and view a Multiple Sequence Alignment

Phylogenetic trees are computed from molecular sequence data but they require that data to be aligned in a Multiple Sequence Alignment (MSA).
MAFFT is a commonly used multiple sequence analysis program. Choose the _MAFFT_ tool from the tool menu and run it with the _combined samples_
as the input. It will generate an output called _MAFFT on data X_ where X is a number. This is the multiple sequence alignment output.

To view the multiple sequence alignment output, click on the name of the dataset and click on the _Visualize_ button (it looks like a tiny bar
graph). Then select the Multiple Sequence Alignment Viewer - you can scroll left and right to see the differences between the sequences.

#### Compute a Phylogenetic Tree using IQ-TREE

IQ-TREE is a fast and flexible phylogeny computation tool. Select _IQ-TREE_ from the menu and choose the _MAFFT on data X_ output as its
input. **Note** that the _IQ-TREE_ tool will not do this for you.

Then scroll down to the _Bootstrap parameters_ section of the tool settings, open it and select the _Ultrafast bootstrap parameters_ option.
Specify _1000_ for the number of bootstrap replicates.

Now run the _IQ-TREE_ tool. It will take some time to run and generate 6 outputs.

#### Calculate a SNP distance matrix and visualise it using heatmap2

It is often useful to see how many differences there are between sequences. The _SNP Distance Matrix_ tool can help us see that.
Run _SNP Distance Matrix_ with the _MAFFT on data X_ tool output as its input. The output produced will make a tab separated matrix
of the number of SNPs different between different samples. Use this as input to the _heatmap2_ tool to generate a heat map.

### Visualising Case and Phylogenetic Data using Microreact

[Microreact](https://microreact.org/) is a flexible tool for visualising data from phylogenetic trees alongside associated metadata. It can
plot points on maps, produce graphs of events (e.g. cases) over time and visualise phylogenetic trees. Microreact is a project of
the [Centre for Genomic Pathogen Surveillance (CGPS)](https://www.pathogensurveillance.net/) which is part of Oxford University's Big Data
Institute. Documentation for Microreact can be found [here](https://docs.microreact.org/).

The tree data computed by IQ-TREE is in the _MaxLikelihood Tree_ output dataset. It is in Newick format, a computer-oriented
representation of phylogenetic tree data and looks like this:

```
(MN908947.3:0.0001338371,(((((((((sample18:0.0000010000,sample9:0.0000334422)60:0.0000010000,((sample14:0.0000010000,sample19:0.0000010000)54:0.0000010000,sample6:0.0000010000)23:0.0000010000)74:0.0000010000,sample2:0.0000010000)80:0.0000010000,(sample12:0.0000010000,sample22:0.0000010000)61:0.0000010000)74:0.0000010000,sample20:0.0000010000)61:0.0000010000,sample5:0.0000010000)74:0.0000010000,(sample8:0.0000010000,sample23:0.0000010000)71:0.0000010000)57:0.0000010000,sample21:0.0000010000)65:0.0000334782,(sample15:0.0000010000,(sample4:0.0000334422,sample24:0.0000010000)59:0.0000010000)100:0.0001339029)89:0.0000669261,((sample30:0.0001672263,(sample28:0.0000668860,sample26:0.0000668860)66:0.0000010000)98:0.0001004702,sample25:0.0001337780)66:0.0000010000);
```

In addition to Microreact, tree views like [Figtree](http://tree.bio.ed.ac.uk/software/figtree/) and [iTOL](https://itol.embl.de/) can display data like this. It can
also be previewed using the _Phylogenetic Tree Visualisation_ and _Phyloviz_ visualisation plugins in Galaxy.

#### Visualising tree data in Microreact

To view the tree data that we generated in Microreact, download the _MaxLikelihood Tree_ dataset and upload it to the [Microreact upload page](https://microreact.org/upload). If you upload only the tree data you can use the Microreact tree viewer to navigate the tree and visualise it in different layouts.

Metadata associated with the samples was prepared and shared in [this Google spreadsheet](https://docs.google.com/spreadsheets/d/1GSxYXR9P43fDGGYhQ88ewTnIpc_RmepaLjwba4SEFfI/edit#gid=0). To download the spreadsheet data, use the _File_ → _Download_ → _Tab Separated Value_ menu option. While Microreact can read Excel and Comma Separated Value (CSV) files in addition to Tab Separated Value (TSV) format, for this exercise it is easier to download in TSV format.

Upload both the metadata file and the tree file to the [Microreact upload page](https://microreact.org/upload). You will now see a map showing case data, a phylogenetic
tree and a timeline graph showing case data over time. If the metadata file is uploaded without the tree file, Microreact will display a map and timeline. In this way
Microreact can be used to prepare map and timeline visualisations, even if you lack genomic data.

The metadata was prepared in the format that Microreact accepts:

* An _id_ column is present with values that match the names of leaf nodes in the phylogenetic tree. Dr Nabil Ali-Khan has a useful introduction to [how to choose a
good identifier](https://youtu.be/G5YU5CLQaXw?t=1083) that is part of his larger talk on COVID-19 "bioinformatics gotchas".

* Dates are specified using _year_, _month_, and _day_ columns. An alternative to using _year_, _month_ and _day_ columns is to use a single _date_ column where the date is specified as a single value. Microreact supports a [range of date formats](https://docs.microreact.org/instructions/data/timeline-panel#date-formats) but ISO 8601 (yyyy-MM-dd) is preferred. Read more about timeline data formats [here](https://docs.microreact.org/instructions/data/timeline-panel).

* Geographic location is specified using decimal _latitude_ and _longitude_. Read more about specifying geographic location [here](https://docs.microreact.org/instructions/data/map-panel); if you simply want to specify country of origin data, ISO 3166-1 country codes (e.g. za for South Africa) can be used. In our example, precise
latitude and longitude was used. Such data would almost never be used in reality because of privacy concerns.

While the data for this exercise was prepared manually, in general data is integrated from multiple sources. The CGPS has another tool called [Data=flo](https://data-flo.io/)
that you might find useful for integrating and reformatting data from multiple sources.











