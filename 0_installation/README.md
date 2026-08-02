# Installation:

## MAC
### 1. Installing ANACONDA
Captus uses ANACONDA for installation. Please make sure to install it in your computer. [Full instructions for installation are found here](https://www.anaconda.com/docs/getting-started/miniconda/install/mac-cli-install
), but ANACONDA can be installed downloading the `miniconda` installer for M processors:
```
curl -O https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-arm64.sh
```
And then run the installer:
```
bash Miniconda3-latest-MacOSX-arm64.sh
```
If your mac is Intel-based, you can download `miniconda` using:
```
curl -O https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-x86.sh
```
And then run the installer:
```
bash Miniconda3-latest-MacOSX-x86.sh
```


### 2. Installing Captus
With ANACONDA installed now you can install CAPTUS. Mac computers with Apple silicon (M processors):
```
conda create --platform osx-64 -n captus -c bioconda -c conda-forge captus megahit=1.2.9=hfbae3c0_0 mmseqs2=16.747c6 salmon=1.10.3 bbmap=39.52 mafft=7.526
```

For Mac computers with Intel processors:
```
conda create -n captus -c bioconda -c conda-forge captus megahit=1.2.9=hfbae3c0_0 salmon=1.10.3 bbmap=39.52 mafft=7.526
```

Then check that Captus was correctly installed:
```
conda activate captus
captus -h
```
If the program was correctly installed, you will see the following help message:
```
usage: captus command [options]

Captus 1.6.6: Assembly of Phylogenomic Datasets from High-Throughput Sequencing data

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

### 1. Installing Ubuntu using WSL

Even though Python runs on Windows, most of Captus' dependencies only work on Mac and Linux. Fortunately, installing Linux has become very easy in Windows 10 and 11. If you are interested you can read about the procdure here: [https://ubuntu.com/wsl/docs/latest/howto/install-ubuntu-wsl2/](https://ubuntu.com/wsl/docs/latest/howto/install-ubuntu-wsl2/). Your machine needs at least 5GB of space in hard drive and must have virtualization enabled in the BIOS/UEFI.

To install Ubuntu on Windows using Windows Subsystem for Linux (WSL) let's open a PowerShell terminal and run:
```
wsl --install
```
Restart the computer if the installer asks for it, then open a PowerShell terminal and run the following to see which Linux distributions are available:
```
wsl --list --online
```
Finally, let's install Ubuntu:
```
wsl --install Ubuntu
```
In the future you can start Ubuntu using:
```
wsl -d Ubuntu
```

### 2. Installing ANACONDA on WSL Ubuntu

If your PC is Intel-based, you can download `miniconda` using:
```
curl -O https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
```
And then run the installer:
```
bash Miniconda3-latest-Linux-x86_64.sh
```
If your PC has an ARM64 processor use:
```
curl -O https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-aarch64.sh
```
And then run the installer:
```
bash Miniconda3-latest-Linux-aarch64.sh
```

### 3. Installing Captus on WSL Ubuntu
Run the following command:
```
conda create -n captus -c bioconda -c conda-forge captus megahit=1.2.9=hfbae3c0_0 salmon=1.10.3 bbmap=39.52 mafft=7.526
```

Then check that Captus was correctly installed:
```
conda activate captus
captus -h
```
If the program was correctly installed, you will see the following help message:
```
usage: captus command [options]

Captus 1.6.6: Assembly of Phylogenomic Datasets from High-Throughput Sequencing data

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



## Additional software
[MAFFT](https://mafft.cbrc.jp/alignment/software/)

[TrimAl](http://trimal.cgenomics.org/)

[AliView](https://ormbunkar.se/aliview/)

[IQTREE](http://www.iqtree.org/)

[Figtree](http://tree.bio.ed.ac.uk/software/figtree/)

[Astral3-pro](https://github.com/chaoszhang/ASTER/blob/master/tutorial/astral-pro3.md)