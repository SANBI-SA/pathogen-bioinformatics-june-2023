---
title: Scientific Workflow Languages and Nextflow
---
### Scientific Workflow Languages

In our [Introduction to the Command Line](../../intro-to-cli/intro-to-cli) we introduced the idea of a "shell script" to collect
together commands into a single block of re-useable analysis. Such shell scripts are written in a shell language like Bash and
can also take parameters so that they act like custom-built commands. We also introduced the idea of variables and loops,
including parameter variables like `$1` and `$2`.

While shell scripts provide a first step into producing reproducible analyses, there are two significant issues that they don't
address:

1. The software used for analysis needs to be installed. This is known as _dependency management_. We have seen how packaging systems like _conda_ can help with dependency management and briefly mentioned the existence of the _Docker_ and _Singularity_ software container systems.

2. Analyses can run in many environments (e.g. on a laptop, on a computing cluster and in cloud resources) and tasks can sometimes be split up and run in parallel.

Scientific Workflow Languages and Scientific Workflow Management Systems extend computing capabilities to address these concerns. A scientific workflow language allows for the steps of analysis to be specified in modular units and a workflow management system takes such a specification, manages the installation of dependencies and executes the analysis.

In this course we have already seen Galaxy, a web based bioinformatics environment that contains a workflow management system. Galaxy workflows are created from blocks of analysis known as _tools_ and edited in a graphical workflow editor. They can also be shared either as downloaded files (in a JSON-based format) or via workflow publishing services like Dockstore or WorkflowHub.EU.

In addition to Galaxy, several command-line oriented workflow management systems exist. Some commonly used examples are [_Nextflow_](https://www.nextflow.io/), [_Snakemake_](https://snakemake.readthedocs.io/en/stable/), the [_Common Workflow Language (CWL)_](https://www.commonwl.org/) and the [_Workflow Description Language (WDL)_](https://openwdl.org/).

We will be focusing on _Nextflow_ because it is widely used, has an activate community of workflow-publisher (e.g. the [nf-core](https://nf-co.re/) project) and has useful features such as dependency management and support for resuming workflows.

Our training was drawn from the [Nextflow Basic Training](https://training.nextflow.io/basic_training/) materials. After you have installed Nextflow (see the notes on that [here](https://training.nextflow.io/basic_training/setup/#local-installation)), start with the training [Introduction]https://training.nextflow.io/basic_training/intro/).

#### Key points on Nextflow

Nextflow workflows are written in a language called Groovy and are composed out of modules called _processes_ that are combined into a _workflow_ using _channels_. Each process has input and output channels that consume and produces values such as file system paths and text strings. For more on channels [read here](https://training.nextflow.io/basic_training/channels/) and on processes read [here](https://training.nextflow.io/basic_training/processes/). Finally, operators (such as `mix()` and `collect()`) are used to transform the contents of channels and thereby change how workflows execute. Read more about operators [here](https://training.nextflow.io/basic_training/operators/).

Each Nextflow workflow can accept parameters via the special purpose _params_ variables. These parameters can either be set on the command line or provided via a _nextflow.config_ file that is typically in the directory that you run your workflow from. If they are set on the command line they are set using double `-`, e.g.

```bash
nextflow run -resume mapping.nf --reads data/reads/*_{R1,R2}.fastq.gz --reference data/reference.fasta
```

In the above example `-resume` is a flag passed to the Nextflow runner and `--reads` and `--reference` set the variables `params.reads` and `params.reference` respectively. If parameters are set in a `nextflow.config` file they are in a _params block_. For example, such a file could look like this:

```nextflow
params {
    reads = "$projectDir/data/gut_{1,2}.fq"
    reference = "$projectDir/data/reference.fasta"
}
```

Dependencies in Nextflow can be managed using either the _Docker_ or _Singularity_ container system or using _Conda_. Container management and execution systems bundle software together into a _container_ that provides a _filesystem_ with all the necessary software to support an analysis. Information on installing Docker can be found on the [here](https://docs.docker.com/engine/install/). Singularity differs from Docker in that container images are supplied as files and are run as the currently logged-in user. On shared computing resources, many IT admins prefer Singularity over Docker because of the security benefits. Instructions on installed Singularity are [here](https://apptainer.org/docs/admin/main/installation.html) on the Apptainer website (Apptainer is a development project providing Singularity software). For Linux, the installation from pre-built packages (described [here](https://apptainer.org/docs/admin/main/installation.html#install-from-pre-built-packages)) is recommended.

The bioconda project automatically builds Docker and Singularity containers for each conda package that they provide packages for. There are currently more than 10,000 packages available this way. StaPH-B also provides some 150 [containers for commonly used bioinformatics tools](https://github.com/StaPH-B/docker-builds#docker-image-repositories--hosting). (StaPH-B is the State Public Health Bioinformatics Group, an association of bioinformaticians working in Public Health labs in states of the USA)

Dependencies can either be associated with the Nextflow environment as a whole (and thus configured in the `nextflow.config`) or they can be specified for a particular process. Read more about how this is done in [this section](https://training.nextflow.io/basic_training/containers/#biocontainers) of the training.

#### Nextflow communities

The [nf-core](https://nf-co.re/) community is an especially active community using Nextflow. It provides online [training](https://nf-co.re/events/2023/training-march-2023) and also runs a Slack discussion forum.

