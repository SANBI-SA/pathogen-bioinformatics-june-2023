---
title: Installing Software in the Bash Shell
---

### Bioinformatics Software Packages

A typical bioinformatics workflow involves several tools, each of which may depend on other tools and
software libraries (known as _dependencies_). Modern operating systems come with _package managers_
(e.g. apt on Ubuntu Linux, dnf on Fedora Linux and Microsoft Store on Windows) but often they don't
include packages for bioinformatics software.

The _conda_ package management system has emerged to make it easier to install software packages,
including bioinformatics packages. Conda was originally developed by [Anaconda.org](https://anaconda.org/)
for installing Python packages, but it has now been adopted by several different communities.

#### Miniconda, mamba, channels, Mambaforge

The original _Anaconda_ package distribution contains a complete toolkit for data science with Python.
As the _conda_ package manager became a popular tool for installing packages, a smaller distribution of
_conda_ with only the necessary supporting software called _Miniconda_ was developed.

Conda installs software by comparing the list of software the user already has with the list of
dependencies needed for the new software to be installed. This process is called _solving dependencies_
and, with the original _conda_ tool, can be very time consuming. To address the problem of long
install times, [QuantStack](https://quantstack.net/) developed _mamba_. Mamba speeds up installations
of conda package.

Both _conda_ and _mamba_ install software from the same collections of packages. These collections
are called _channels_. The _Anaconda.org_ developers produced the original, _base_ channel and, since
then, two important channels have been produced by developer communities:

* _conda-forge_ is a collection of general purpose and scientific software

* _bioconda_ is a collection of software for bioinformatics

The _conda-forge_ community also created a distribution called [_Mambeforge_](https://github.com/conda-forge/miniforge#mambaforge)
that comes with _mamba_ pre-installed. This makes it a good starting point for software installation.

#### Operating Systems and Architectures

All computers run a _Operating System_ or OS, for example, Linux, Windows or MacOS. When software is packaged, it is
made available for a specific operating system. In addition, a computer has a Central Processing Unit (CPU) that
interprets the machine language instructions that make a computer work. Each CPU has its own architecture, and
only programs built for this architecture will work on the particular CPU. Most laptop and server computers in use
at present use the _x86_ architecture (also known as _amd64_). The latest Mac computers, however, use the _M1_ architecture.

Software package managers can only install software if it is built for the correct operating system and architecture.

#### Getting started with Mambaforge

To get started, you need to download the correct installer for the package management system. Since we will be using
_Mambaforge_, download the correct release for your operating system. 

For Linux on x86_64 / amd64, download using:

```bash
wget -c https://github.com/conda-forge/miniforge/releases/latest/download/Mambaforge-Linux-x86_64.sh
```

(the `-c` command line flag will ensure that your download will continue if it somewhat got interrupted)

for MacOS on x86_64 / amd64, download using:

```bash
curl -O -J -L -C - https://github.com/conda-forge/miniforge/releases/latest/download/Mambaforge-MacOSX-x86_64.sh
```


Then install the downloaded installer by running it using `bash`:

For Linux:

```bash
bash Mambaforge-Linux-x86_64.sh
```

For MacOS:

```bash
bash Mambaforge-MacOSX-x86_64.sh
```

The installer will ask you to agree to a License Agreement. Press the space bar to page through the license agreement
and then type `yes` and hit Enter to accept it. The installation then proceeds. Finally you are asked if you want to
run `conda init`. Again type `yes` and press Enter. The installer changes your login settings (in `.bashrc`), so after
installation you should log out and then log back in again.

A log of what you can expect to see during the install can be seen [here](https://gist.github.com/pvanheus/906d5fa5cc5d01a8656538f23b779582).

#### Setting up channels

Once you have conda installed, you should see a `(base)` added to your prompt e.g. `(base) user1@head:~$`. This means that conda has
been added to your shell login and is ready for use. As mentioned above, software developers make software available in _channels_
and these need to be added to your conda configuration. Run these commands (taken from [bioconda](https://bioconda.github.io) instructions):

```bash
conda config --add channels defaults
conda config --add channels bioconda
conda config --add channels conda-forge
conda config --set channel_priority strict
```

This configures your conda to search for packages from the _conda-forge_ and _bioconda_ channels in addition to the default channel provided by
Anaconda.org. You are now ready to install software.

#### Installing software with mamba and conda

One of the major advantages of conda as a package manager is that it makes it easy to have multiple _environments_. An environment is a collection
of software packages. Because not all software works well together (e.g. one package might use Python 2.7, another might use Python 3), best
practice is to make a new environment for each task that you want to perform and only install related software packages in it. When you start
in conda you are in the `base` environment (this is what the `(base)` in your prompt means). In general, do not install software into the
`base` environment!

For our practice, we want to install some quality control and trimming packages. While we could (and perhaps should) make a different
environment for each software package, for ease of use we will create a single environment called `qc` using the command:

```bash
mamba create -n qc -y fastqc multiqc flexbar trimmomatic
```

Note that we are using `mamba` because this allows for faster installations than using the `conda` command (the two commands might merge their
features in the future though). The `-y` flag tells `mamba` to install packages without checking with you for further confirmation and the
`-n qc` flag sets the name of the environment being created.

Once the installation of packages (which might take some minutes) is complete, you can use the environment with

```bash
conda activate qc
```

This will change your prompt to something like `(qc) user1@head:~$`. You can switch back to the base environment by using `conda deactivate`.

Note that if you happen to use `conda deactivate` too many times, you will completely deactivate conda. Then, to re-activate conda, you should
either log out and log back in or manually activate conda with `source $HOME/mambaforge/bin/activate`.

### Containers

Our discussion of software installation has focused on package management using conda and mamba. Another technology that is emerging
for software management is software containers. Containers technology is available on Linux and (with some extra effort) MacOS and
Windows with WSL2. Instead of installing software on your computer, they provide a custom set of software all gather together into
a single "container image".

Containers are beyond the scope of this course but if you want to know more about them, you could read up on 
[Docker for containerised bioinformatics](https://www.melbournebioinformatics.org.au/tutorials/tutorials/docker/docker/) and
[Singularity containers](https://telatin.github.io/microbiome-bioinformatics/Singularity/).
