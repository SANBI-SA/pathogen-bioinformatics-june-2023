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





