# Installation:

## MAC
### Installing ANACONDA
Captus uses ANACONDA for installation. Please make sure to install it in your computer. [Full instructions for installation are found here](https://www.anaconda.com/docs/getting-started/miniconda/install/mac-cli-install
), but ANACONDA can be installed downloading its installer:
```
curl -O https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-arm64.sh
```

And then runing the installer:
```
bash ~/Miniconda3-latest-MacOSX-arm64.sh
```
If your mac is intel-based, you will need `MacOSX-x86_64.sh`.

We also need a specific version for SALMON:
```
conda install -c bioconda -c conda-forge salmon=1.10.3
```

### Installing CAPTUS
With ANACONDA installed now you can install CAPTUS. Mac computers with Apple silicon (M processors):
```
conda create --platform osx-64 -n captus -c bioconda -c conda-forge captus megahit=1.2.9=hfbae3c0_0 mmseqs2=16.747c6
```

For Mac computers with Intel processors:
```
conda create -n captus -c bioconda -c conda-forge captus megahit=1.2.9=hfbae3c0_0
```

Then check that Captus was correctly installed:
```
conda activate captus
captus -h
```
If the program was correctly installed, you will see the following help message:
```
usage: captus command [options]

Captus 1.3.3: Assembly of Phylogenomic Datasets from High-Throughput Sequencing data

Captus-assembly commands:
  command     Program commands (in typical order of execution)
                clean = Trim adaptors and quality filter reads with BBTools,
                        run FastQC on the raw and cleaned reads
                assemble = Perform de novo assembly with MEGAHIT and estimate
                           contig depth of coverage with Salmon: Assembling
                           reads that were cleaned with the 'clean' command is
                           recommended, but reads cleaned elsewhere are also
                           allowed
                extract = Recover targeted markers with BLAT and Scipio:
                          Extracting markers from the assembly obtained with
                          the 'assemble' command is recommended, but any other
                          assemblies in FASTA format are also allowed
                align = Align extracted markers across samples with MAFFT or
                        MUSCLE: Marker alignment depends on the directory
                        structure created by the 'extract' command. This step
                        also performs paralog filtering and alignment trimming
                        using TAPER and ClipKIT

Help:
  -h, --help  Show this help message and exit
  --version   Show Captus' version number

For help on a particular command: captus_assembly command -h
```

## PC

## Additional software
[MAFFT](https://mafft.cbrc.jp/alignment/software/)

[TrimAl](http://trimal.cgenomics.org/)

[AliView](https://ormbunkar.se/aliview/)

[IQTREE](http://www.iqtree.org/)

[Figtree](http://tree.bio.ed.ac.uk/software/figtree/)

[Astral3-pro](https://github.com/chaoszhang/ASTER/blob/master/tutorial/astral-pro3.md)