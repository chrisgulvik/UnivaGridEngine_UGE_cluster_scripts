# Univa Grid Engine (UGE) cluster scripts
tested on UGE 8.4.3 but might also work on other UGE, SGE, and OGE scheduler versions


### Requirements
- **$LAB_HOME** environment variable set containing subdirectories
  - **.anaconda2/** containing a full anaconda installation with a virtual environment called 'bpy2' with python2, biopython, the scipy stack, and other bioinformatics-related modules installed
  - **.bin/** containing all genomics_scripts here [[link]](https://github.com/chrisgulvik/genomics_scripts)
  - **.job/** containing all of these UGE scripts
  - **.lib/** containing library files
- ability to submit to **all.q** queue

### Installation Example

    git clone https://github.com/chrisgulvik/UnivaGridEngine_UGE_cluster_scripts.git $LAB_HOME/.job
    echo 'export PATH="$LAB_HOME/.job:$LAB_HOME/.bin:$PATH"' >> ~/.bashrc

### Batch Job Workflows
- ##### Illumina PE reads v1.8+ to annotated assembly
 - Given a directory of paired read files, each sample will have its genome assembled and annotated. If 3 or more samples are provided, Parsnp and ANI will be computed as well.
 - Usage: `trim.asm.annot.parsnp.ani.uge-bash <input-dir> <output-dir>`
