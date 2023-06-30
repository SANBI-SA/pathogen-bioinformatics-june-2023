---
title: SARS-CoV-2 Practice Exercises
---

### Starting Point: The Workflows

Please see the notes on [SARS-CoV-2 sequence analysis](../sars-cov-2-workflows) for information on getting the workflows
you need to analyse this data.


#### Exercise 1: Illumina Data

Import the [Illumina data history](https://usegalaxy.eu/u/pvanheus/h/sars-cov-2-illumina-practice1). This has all the read data and the reference data
prepared, ready for analysis. 

#### Exercise 2: Nanopore Data

Import the [Nanopore data history](https://usegalaxy.eu/u/pvanheus/h/sars-cov-2-ont-practice1). This has the read data and reference data for
the ONT analysis.

#### Questions related to the Practice Analysis

1. In which file format were the initial input files?

2. Why do we do quality trimming on Illumina read data?

3. What does it mean if the symbol in the BED file is negative or positive for ARTIC v3 primers?

4. For the Illumina samples, what do we do to the samples to make sure they were all grouped together?

5. Name two reasons why grouping samples together are advantageous.

6. Why are primer BED files needed when doing an amplicon sequencing analysis?

7. Looking at the Quality Control Report in the Illumina samples, what does the cumulative genome coverage mean? 

8. Why do we do primer trimming and adapter trimming?

9. Use the correct output file in the output of the Illumina workflow, to locate a variant in sample SRR11954075, at position 4. What was the reference allele and what was the variant/alternate allele. Can we trust that this is a variant?

10. Describe how you will go about uploading a dataset from a link?

11. How would you run a workflow imported from the GA4GH TRS servers?

12. How do you edit an imported workflow and change the parameters on one of the tools?

13. What information do we find in a BAM file?

14. What information do we get from aligning a read against a genome?

15. How is a BAM file different to MAFFT output file?

16. In what format is a genome consensus file?
    
18. How is a BAM file different to MAFFT output file?
   
20. If we look into a consensus file, what do we typically see inside?
    
22. In what format is a consensus file?

23. What is the average read lengths of your Nanopore sample SRR23801000 before filtering
    
25. What percentage of reads had a quality score above 12 for sample SRR23800470 after filtering?

26. Why do we need metadata for a phylogenetics tree?
    
28. Name two types of phylogenetics tree formats. Hint: Look under Microreact -> Upload.
   
30. What is the correct basic Bioinformatics pipeline for SARS CoV-2?
    a) Run Quality control on input files -> create an alignment ->  trim your reads -> create a consensus file -> do variant calling -> build a phylogenetics tree
    b) Run Quality control on input files ->  trim your reads -> create an alignment -> create a consensus file -> do variant calling -> build a phylogenetics tree
    c) Run Quality control on input files ->  trim your reads -> create an alignment -> do variant calling -> create a consensus file -> build a phylogenetics tree

31. What are the steps to sharing a workflow you created with a colleague?
  

