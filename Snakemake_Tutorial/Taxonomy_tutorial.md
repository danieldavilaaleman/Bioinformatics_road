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


