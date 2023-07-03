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
_Mambaforge_, download the release for Linux on amd64. This can be done from your Bash command line using the command:

```bash
wget -c https://github.com/conda-forge/miniforge/releases/latest/download/Mambaforge-Linux-x86_64.sh
```

(the `-c` command line flag will ensure that your download will continue if it somewhat got interrupted)

Then install the downloaded installer by running it using `bash`:

```bash
bash Mambaforge-Linux-x86_64.sh
```

The installer will ask you to agree to a License Agreement. Press the space bar to page through the license agreement
and then type `yes` and hit Enter to accept it. The installation then proceeds. Finally you are asked if you want to
run `conda init`. Again type `yes` and press Enter. The installer changes your login settings (in `.bashrc`), so after
installation you should log out and then log back in again.

A log of what you can expect to see during the install can be seen [here](https://gist.github.com/pvanheus/906d5fa5cc5d01a8656538f23b779582).

#### Setting up channels

Coming Soon
