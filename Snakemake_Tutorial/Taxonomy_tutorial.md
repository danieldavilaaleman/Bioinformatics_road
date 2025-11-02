# Welcome to 101 Snakemake tutorial!
On this tutorial, we are going to implement a Snakemake pipeline for a real world case in the microbiome field: Taxonomy classification of Next-Generation Sequencing (NGS) data.

This tutorial is based on the official [Snakemake tutorial documentation](https://snakemake.readthedocs.io/en/stable/) and [mmseqs2 tutorial](https://github.com/soedinglab/MMseqs2/wiki/Tutorials) 

## The Goal of our tutorial
1. QC of the NGS data using `fastp`
2. Get the stats of our "clean" NGS files usign `seqkit`
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

## Step 3 - Create a Snakefile and the first pipeline rule
1. `vim Snakefile`
2. Add the first rule of the pipeline to run `fastp`
```
rule clean_reads:
	input:
		"data/raw_reads/ERR318619.fastq.gz"
	output:
		"data/clean_reads/ERR318619_clean.fastq.gz"
	conda:
		"/path/to/miniconda3/envs/snakemake-tutorial"
	shell:
		"fastp -i {input} -o {output}"
```
3. Check that the Snakefile file works properly, try a "dry" run.
`snakemake -np`

4. Run the Snakemake pipeline
`snakemake --cores 8 --sdm conda`

## Step 3 - Generalizing Snakemake rules
The las rule will only work for the sample `ERR318619.fastq.gz` but we can use **wildcards** to generalize the clean_reads rule.
1. `vim Snakefile`
2. ```
   rule clean_reads:
	input:
		"data/raw_reads/{sample}.fastq.gz"
	output:
		"data/clean_reads/{sample}_clean.fastq.gz"
	conda:
		"/Users/danieldavila/miniconda3/envs/snakemake-tutorial"
	shell:
		"fastp -i {input} -o {output}"
   ```
3. Check Snakefile file with a "dry" run. NOTE:dry run ask for the output file.
`snakemake -np data/clean_reads/ERR318620_clean.fastq.gz`
4. Can you specify multiple output targets?
`snakemake -np data/clean_reads/{ERR318620_clean,ERR429199_clean}.fastq.gz`

## Step 4 - Adding the second rule, stats of the clean reads.
1. `vim Snakefile`
2. ```
   rule clean_reads:
	input:
		"data/raw_reads/{sample}.fastq.gz"
	output:
		"data/clean_reads/{sample}_clean.fastq.gz"
	conda:
		"/Users/danieldavila/miniconda3/envs/snakemake-tutorial"
	shell:
		"fastp -i {input} -o {output}"

	rule reads_stats:
	input:
		"data/clean_reads/{sample}_clean.fastq.gz"
	output:
		"data/stats/{sample}_stats.txt"
	conda:
		"/Users/danieldavila/miniconda3/envs/snakemake-tutorial"
	shell:
		"seqkit stats {input} > {output}"
   ```
3. Check Snakefile file with a "dry" run. NOTE:dry run ask for the output file.
`snakemake -np data/stats/{ERR318619,ERR318620,ERR429199}_stats.txt`

## Step 5 - Adding rule for taxonomy classification and results visualization
1. `vim Snakefile`
2. ```
   rule clean_reads:
	input:
		"data/raw_reads/{sample}.fastq.gz"
	output:
		"data/clean_reads/{sample}_clean.fastq.gz"
	conda:
		"/Users/danieldavila/miniconda3/envs/snakemake-tutorial"
	shell:
		"fastp -i {input} -o {output}"

   rule reads_stats:
	input:
		"data/clean_reads/{sample}_clean.fastq.gz"
	output:
		"data/stats/{sample}_stats.txt"
	conda:
		"/Users/danieldavila/miniconda3/envs/snakemake-tutorial"
	shell:
		"seqkit stats {input} > {output}"

   rule reads_taxonomy:
	input:
		"data/clean_reads/{sample}_clean.fastq.gz"
	output:
		"data/taxonomy/{sample}.lca.tsv"
	conda:
		"/Users/danieldavila/miniconda3/envs/snakemake-tutorial"
	shell:
		"""
		mmseqs createdb {input} {wildcards.sample}.DB
		mmseqs taxonomy {wildcards.sample}.DB data/mmseqs_DB/data/mmseqs_DB/uniprot_sprot {wildcards.sample}.lca_results tmp -s 2
		mmseqs createtsv {wildcards.sample}.DB {wildcards.sample}.lca_results {wildcards.sample}.lca.tsv
		"""
   ```
3. Check Snakefile with "dry" run
  `snakemake -np data/stats/{ERR318619,ERR318620,ERR429199}_stats.txt data/taxonomy/{ERR318619,ERR318620,ERR429199}.lca.tsv`


