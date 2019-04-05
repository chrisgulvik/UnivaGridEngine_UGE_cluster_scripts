# Univa Grid Engine (UGE) cluster scripts
tested on UGE 8.4.3 but might also work on other UGE, SGE, and OGE scheduler versions


## Requirements
- **$LAB_HOME** environment variable set containing subdirectories
  - **.anaconda2/** containing a full anaconda installation with a virtual environment called ['bpy2'](https://github.com/chrisgulvik/genomics_scripts#example-installation-of-bpy2-environment) with python2, biopython, the scipy stack, and other bioinformatics-related modules installed
  - **.bin/** containing all genomics_scripts here [[link]](https://github.com/chrisgulvik/genomics_scripts)
  - **.job/** containing all of these UGE scripts
  - **.lib/** containing library files
- ability to submit to both **all.q** and **short.q** queues

## Installation Example

    git clone https://github.com/chrisgulvik/UnivaGridEngine_UGE_cluster_scripts.git $LAB_HOME/.job
    echo 'export PATH="$LAB_HOME/.job:$LAB_HOME/.bin:$PATH"' >> ~/.bashrc

## Batch Job Workflows
### _Create Assemblies_
- ##### Illumina PE reads v1.8+ to annotated assembly
  - Given a directory of paired Illumina FastQ read files, each sample will have its genome assembled and annotated. If 3 or more samples are provided with the script containing 'parsnp.ani', Parsnp and ANI will be computed as well.
  - Usage: `trim.asm.annot.parsnp.ani.uge-bash <input-dir> <output-dir>`
  - Usage: `trim.asm.annot.uge-bash <input-dir> <output-dir>`

- ##### PacBio (with or without corresponding Illumina) reads to annotated assembly
  - Given a directory of PacBio read files, each sample will have its genome assembled and annotated. If Illumina PE reads are also provided, assemblies will be polished.
  - Usage: `pacbio-illumina.cln.asm.ann.uge-bash <input-dir> <output-dir>`

### _Analyze Assemblies_
- ##### AAI from GenBank or Amino Acid FastA files
  - Given a directory of files, optionally gunzip compressed, AAI with Diamond will be computed for all pairs. If the same output-dir is given to a follow-up analysis, only new pairs will be computed, and all pairs re-summarized. Especially useful for direct assessment of \*\_genomic.gbff.gz files from NCBI.
  - Usage: `batch-aai-diamond.uge-bash <input-dir> <output-dir>`

- ##### ANI from GenBank or Nucleotide FastA files
  - Given a directory of files, optionally gunzip compressed, ANI with BLASTn (identical as assemblies2ani.uge-bash) will be computed for all pairs. If the same output-dir is given to a follow-up analysis, only new pairs will be computed, and all pairs re-summarized. Especially useful for direct assessment of \*\_genomic.gbff.gz files from NCBI.
  - Usage: `batch-ani.uge-bash <input-dir> <output-dir>`

- ##### AAI and ANI from nucleotide assemblies
  - Given a directory of FastA assembly files, all pairwise AAI with BLASTp or ANI with BLASTn comparisons will be computed and the bidirectional values summarized. If the same output-dir is given to a follow-up analysis, only new pairs will be computed, and all pairs re-summarized. Prokka is used on each input file initially to unify sample naming and keep the same minimum contig length. Output sample names are modified if not plain alphanumeric.
  - Usage: `assemblies2aai.uge-bash <input-dir> <output-dir>`
  - Usage: `assemblies2ani.uge-bash <input-dir> <output-dir>`

- ##### Pseudogene estimation of assemblies
  - Given a directory of FastA nucleotide assembly files, the fraction of proteins with unusually large and small lengths are reported in graphical and tabular formats.
  - Usage: `assemblies2pseudogenes.uge-bash <input-dir> <output-dir>`

- ##### Single gene trees from assemblies
  - Given a directory of FastA assembly files, a series of genes will be extracted from each and NJ trees constructed. Target genes include:  the 16S rRNA gene, *atpD*, *dnaJ*, *glnA*, *groL*, *recA*, *rpoA*, *rpoB*, *rpoC*, *secA*, *sodA*, *tuf*, *pheS*, and *thrC*.
  - Usage: `assemblies2phylotrees.uge-bash <input-dir> <output-dir>`

- ##### SNPs from assemblies
  - Given a directory of FastA assembly files, Parsnp will be ran on the full set. SNP distances and a phylogenetic tree are produced.
  - Usage: `run_parsnp.uge-bash <input-dir> <output-dir>`
  - To only compute SNP distances and report them in tabular format (pairwise listing and matrix sheet styles) of a previously ran Parsnp, a standalone workflow also exists. It only requires requires the parsnp.ggr binary file in the specified directory and places output in the same place.
  - Usage: `parsnp-distances.uge-bash <parsnp-dir>`
