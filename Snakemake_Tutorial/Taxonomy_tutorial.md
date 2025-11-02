# Welcome to 101 Snakemake tutorial!
On this tutorial, we are going to implement a Snakemake pipeline for a real world case in the microbiome field: Taxonomy classification of Next-Generation Sequencing (NGS) data.

This tutorial is based on the official [Snakemake tutorial documentation](https://snakemake.readthedocs.io/en/stable/) and [mmseqs2 tutorial](https://github.com/soedinglab/MMseqs2/wiki/Tutorials) 

## The Goal of our tutorial
1. QC of the NGS data using `fastp`
2. Get the stats of our "clean" NGS files usign `seqtk`
3. Taxonomy classification usign `mmseqs2`
4. Visualization of taxonomy results using `mmseqs taxonomyreport`

## Step 1 - Installation
We need to install Snakemake and all the required tools for our pipeline. In this case, we need to install `fastp, seqtk`, and `mmseqs2`.

For this step, we will use `mamba`, a fast and efficient version of `conda`, to create an environment with all the tools and dependecies need it in this project.

#### Install conda (if you do not have it)
Go to [Miniconda installation site](https://www.anaconda.com/docs/getting-started/miniconda/install) to check installation instructions.

#### Create a conda environment that contains Snakemake, fastp, seqtk, and mmseqs2
Create the conda environment usinf the `environment.yaml`:
```
name: snakemake-tutorial
channels:
  - conda-forge
  - bioconda
  - defaults
dependencies:
  - snakemake
  - fastp
  - seqkit
  - mmseqs2
  - graphviz

```

## Step 2 - Download Sequencing data
Download sample data for our pipeline. This data is part of the [Tara Oceans Foundation.](https://www.ebi.ac.uk/ena/browser/view/PRJEB402)
Each fastq file is a downsample file of 10,000 reads for demostration purpose.

## Step 3 - Create a Snakemake file and the first pipeline rule
1. `vim Snakemake`
2. 




