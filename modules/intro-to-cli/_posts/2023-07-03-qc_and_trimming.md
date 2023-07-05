---
title: Quality Control and Trimming on the Command Line
----

### Quality Control and Trimming

(this practical is adapted from Hadrien Gourle's [original](https://www.hadriengourle.com/tutorials/qc/))

In this practical you will learn to import, view and check the quality of raw high thoughput sequencing sequencing data.

The first dataset you will be working with is from an Illumina MiSeq dataset.
The sequenced organism is an enterohaemorrhagic E. coli (EHEC) of the serotype O157, a potentially fatal gastrointestinal pathogen.
The sequenced bacterium was part of an outbreak investigation in the St. Louis area, USA in 2011.
The sequencing was done as paired-end 2x150bp.

#### Using the training cluster

**Important**: We will be using the SANBI training cluster that can be accessed at [https://cluster.sanbi.ac.za]. When you login there you are on
the login or "head node" of the cluster. To do the work for this tutorial, you need to be on a "worker node". Run this command:

```bash
srun --pty bash
```

This will start a `bash` shell on of the cluster worker nodes. Your prompt will change, e.g.

```bash
(base) user1@head:~$ srun --pty bash
(base) user1@worker-1:~$
```

Each time you get logged out from the cluster you need to run `srun --pty bash` again to reconnect with the cluster.

Next you need to activate the _conda environment_ that contains the software we are using for this tutorial. To understand
conda and conda environments, read about [software installation](..). Run:

```bash
conda activate qc
```

To activate the `qc` conda environment that has been preconfigured with the software that we will be using. Once again the prompt will
change:

```bash
(base) user1@worker-1:~$ conda activate qc
(qc) user1@worker-1:~$
```

If you see a prompt that looks like `(qc) user1@worker-1:~$` (the hostname might be `worker-2` or `worker-3`) you are ready for the rest
of the tutorial.

#### Downloading the data

The raw data were deposited at the European Nucleotide Archive, under the accession number SRR957824.
You could go to the ENA [website](http://www.ebi.ac.uk/ena) and search for the run with the accession SRR957824.

However these files contain about 3 million reads and are therefore quite big.
We are only going to use a subset of the original dataset for this tutorial.

First create a `data/` directory in your home folder

```bash
mkdir ~/data
```

now let's download the subset

```bash
cd ~/data
curl -O -J -L https://www.dropbox.com/s/tj8dx30yjrohycf/SRR957824_50K_R1.fastq.gz
curl -O -J -L https://www.dropbox.com/s/2vukpj842zwi829/SRR957824_50K_R2.fastq.gz
```

Let’s make sure we downloaded all of our data using md5sum.

```bash
md5sum SRR957824_50K_R1.fastq.gz SRR957824_50K_R2.fastq.gz
```

you should see this

```
a47630c4f803ce4d0dbdc3374463b2f7  SRR957824_50K_R1.fastq.gz
4020c98370fe52ea08559cf64fdab70f  SRR957824_50K_R2.fastq.gz
```

and now look at the file names and their size

```bash
ls -l
```

```
total 97M
-rw-r--r-- 1 hadrien 4.8M Nov 19 18:44 SRR957824_50K_R1.fastq.gz
-rw-r--r-- 1 hadrien 5.0M Nov 19 18:53 SRR957824_50K_R2.fastq.gz
```

There are 50 000 paired-end reads taken randomly from the original data

One last thing before we get to the quality control: those files are writeable.
By default, Linux makes things writeable by the file owner.
This poses an issue with creating typos or errors in raw data.
We fix that before going further

```bash
chmod u-w *
```

#### Working Directory

First we make a work directory: a directory where we can play around with a copy of the data without messing with the original

```bash
mkdir ~/work
cd ~/work
```

Now we make a link of the data in our working directory. A link created an _alias_ for a file, in other words, a shortcut
that refers to the files that we have downloaded. You can read more about links [here](https://medium.com/nerd-for-tech/linux-unix-links-and-its-types-ad7a67f16531).

```bash
ln -s ~/data/* .
```

The files that we've downloaded are FASTQ files. Take a look at one of them with

```bash
zless SRR957824_50K_R1.fastq.gz
```

!!! tip
Use the space bar to scroll down, and type ‘q’ to exit ‘less’

You can read more on the FASTQ format [here](https://training.galaxyproject.org/training-material/topics/sequence-analysis/tutorials/quality-control/slides.html).

!!! question
Where does the filename come from?

!!! question
Why are there 1 and 2 in the file names?

#### FastQC

To check the quality of the sequence data we will use a tool called FastQC.

FastQC has a graphical interface and can be downloaded and run on a Windows or Linux computer without installation.
It is available [here](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/).

However, FastQC is also available as a command line utility on the training server you are using.
To run FastQC on our two files

```bash
fastqc SRR957824_50K_R1.fastq.gz SRR957824_50K_R2.fastq.gz
```

and look what FastQC has produced

```
ls *fastqc*
```

For each file, FastQC has produced both a .zip archive containing all the plots, and a html report.

Download and open the html files with your favourite web browser.

Alternatively you can look a these copies of them:

- [SRR957824_50K_R1_fastqc.html](../uploads/before/SRR957824_50K_R1_fastqc.html)
- [SRR957824_50K_R2_fastqc.html](../uploads/before/SRR957824_50K_R2_fastqc.html)

!!! question
What should you pay attention to in the FastQC report?

!!! question
Which file is of better quality?

Pay special attention to the per base sequence quality and sequence length distribution.
Explanations for the various quality modules can be found [here](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/Help/3%20Analysis%20Modules/).
Also, have a look at examples of a [good](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/good_sequence_short_fastqc.html) and a [bad](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/bad_sequence_fastqc.html) illumina read set for comparison.

You will note that the reads in your uploaded dataset have fairly poor quality (<20) towards the end. There are also outlier reads that have very poor quality for most of the second half of the reads.

#### Adapter trimming

Now we'll do some trimming!

There are several tools (including flexar, trimmomatic, trimgalore, fastp and more) that remove Illumina sequencing adapters from sequences and
perform quality trimming. Before doing this analysis, you might enjoy reading this explanation of adapter trimming from
[Illumina](https://knowledge.illumina.com/software/general/software-general-reference_material-list/000002905). 

We will use two tools, `flexbar` and `trimmomatic`, with `flexbar` doing the adapter trimming and `trimmomatic` doing the quality trimming.

The first thing we need is the adapters to trim off

```bash
curl -O -J -L https://osf.io/v24pt/download
```

Now we run `flexbar` on both our read files

```bash
flexbar -z GZ -a adapters.fasta --reads SRR957824_50K_R1.fastq.gz --reads2 SRR957824_50K_R2.fastq.gz 
```

!!! question
What adapters do you use? You can find out more about the adapters used with different Illumina kits [here](https://knowledge.illumina.com/library-preparation/general/library-preparation-general-reference_material-list/000001314)

!!! question
Which adapter was found most often in the reads? Did you spot it in the FastQC results?

#### Trimmomatic

Most modern sequencing technologies produce reads that have deteriorating quality towards the 3'-end and some towards the 5'-end as well.
Incorrectly called bases in both regions negatively impact assembles, mapping, and downstream bioinformatics analyses.

We will trim each read individually down to the good quality part to keep the bad part from interfering with downstream applications.

To do so, we will use `trimmomatic. Trimmomatic is a flexible read-trimming tool that can apply several different filters. You can read
more about Trimmomatic's filters [here](https://github.com/usadellab/Trimmomatic#description). The filters that we will use are the
 _sliding window_ filter that looks for where the average (mean) quality of bases drops below a certain value in a window that gets moved
 across the reads from start to end, and the _minimum length_ filter which will discard all reads shorter than a minimum length. 

The `trimmomatic` command line is long, so you might want to cut and paste it.

```bash
trimmomatic PE flexbarOut_1.fastq.gz flexbarOut_2.fastq.gz trimmed_paired_r1.fastq.gz trimmed_unpaired_r1.fastq.gz trimmed_paired_r2.fastq.gz trimmed_unpaired_r2.fastq.gz SLIDINGWINDOW:4:20 MINLEN:20
```

The output of `trimmomatic` should be:

```
TrimmomaticPE: Started with arguments:
 flexbarOut_1.fastq.gz flexbarOut_2.fastq.gz trimmed_paired_r1.fastq.gz trimmed_unpaired_r1.fastq.gz trimmed_paired_r2.fastq.gz trimmed_unpaired_r2.fastq.gz SLIDINGWINDOW:4:20 MINLEN:20
Multiple cores found: Using 4 threads
Quality encoding detected as phred33
Input Read Pairs: 43132 Both Surviving: 41616 (96.49%) Forward Only Surviving: 1025 (2.38%) Reverse Only Surviving: 265 (0.61%) Dropped: 226 (0.52%)
TrimmomaticPE: Completed successfully
```

Examine the output files with

```bash
ls -lh *.fastq.gz
```

!!!question
Why are the unpaired outputs smaller than the paired outputs? And if the reads were originally all paired, why are there now unpaired reads?

## FastQC again

Run fastqc again on the filtered reads

```bash
fastqc trimmed_paired_r*
```

and look at the reports

- [trimmed_paired_r1_fastqc.html](../uploads/after/trimmed_paired_r1_fastqc.html)
- [trimmed_paired_r2_fastqc.html](../uploads/after/trimmed_paired_r2_fastqc.html)

## MultiQC

[MultiQC](http://multiqc.info) is a tool that aggregates results from several popular QC bioinformatics software into one html report.

Let's run MultiQC in our current directory

```bash
multiqc .
```

You can download the report or view it by clicking on the link below

- [multiqc_report.html](../uploads/multiqc_report.html)

!!! question
What did the trimming do to the per-base sequence quality, the per sequence quality scores and the sequence length distribution?

#### Combining the analyses in a shell script

We often want to combine different analyses into a bioinformatics _workflow_. Writing a shell script is a simple
way to produce such a workflow.

Using nano create a file called `qc.sh` with these contents:

```bash
#!/bin/bash
# the line above is a "shebang line" that tells the shell how to run this script

# tell bash to exit immediately if there are any errors
set -e

# check that we get two parameters, READ1 and READ2
if [ $# != 2 ] ; then
  echo "Usage: qc.sh <READ1> <READ2>\n" >&2
  exit 1
fi

# check that we have the adapter sequences
if [ ! -f adapters.fasta ] ; then
  echo "Adapter sequences missing" >&2
  exit 1
fi

READ1=$1
READ2=$2

for filename in $READ1 $READ2 ; do
  if [ ! -f $filename ] ; then
    echo "Read filename $filename is missing" >&2
    exit 1
  fi
done

# use the first part of READ1 name for the base name of the reads
# this assumes that the reads are called something ending with _R1.fastq.gz and _R2.fastq.gz
BASE_READ_NAME=$(echo $READ1 |sed 's/_R1.fastq.gz//')

WORKING_DIR="${BASE_READ_NAME}_tmp"

# delete the working directory if it already exists
if [ -d $WORKING_DIR ] ; then
  rm -rf $WORKING_DIR
fi

mkdir $WORKING_DIR
cd $WORKING_DIR

echo Running FastQC on untrimmed reads
mkdir before
fastqc -o before ../$READ1 ../$READ2

echo Trimming adapters with flexbar
# this assumes that we are running the script from a directory with the ad
flexbar -z GZ -a ../adapters.fasta --reads ../$READ1 --reads2 ../$READ2

echo Quality trimming with trimmomatic
trimmomatic PE flexbarOut_1.fastq.gz flexbarOut_2.fastq.gz trimmed_paired_r1.fastq.gz trimmed_unpaired_r1.fastq.gz trimmed_paired_r2.fastq.gz trimmed_unpaired_r2.fastq.gz SLIDINGWINDOW:4:20 MINLEN:20

mv trimmed_paired_r1.fastq.gz ${BASE_READ_NAME}_trimmed_R1.fasta.gz
mv trimmed_paired_r2.fastq.gz ${BASE_READ_NAME}_trimmed_R2.fasta.gz

echo Running FastQC on trimmed reads
mkdir after
fastqc -o after ${BASE_READ_NAME}_trimmed_R1.fasta.gz ${BASE_READ_NAME}_trimmed_R2.fasta.gz

for directory in ../before ../after ; do
  if [ ! -d $directory ] ; then
    mkdir $directory
  fi
done

cp before/* after/* .

echo Running multiqc
multiqc --filename ${BASE_READ_NAME}_multiqc.html .

cp before/* ../before
cp after/* ../after
cp ${BASE_READ_NAME}_multiqc.html ..
cp ${BASE_READ_NAME}_trimmed_R1.fasta.gz ${BASE_READ_NAME}_trimmed_R2.fasta.gz ..

echo Deleting working directory
rm -rf $WORKING_DIR

echo DONE
```

You can download this script with:

```bash
curl -J -O -L 'https://www.dropbox.com/s/hvgirjqememlo92/qc.sh'
```

Then make this script executable with:

```bash
chmod a+x qc.sh
```

and run it with:

```bash
./qc.sh SRR957824_50K_R1.fastq.gz SRR957824_50K_R2.fastq.gz
```
