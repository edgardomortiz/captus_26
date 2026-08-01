# Postprocessing:
this page contains some basic information about postprocessing options after runing CAPTUS. This tutorial is intended to work on a Linux terminal; we believe this will work on the Windows Subsystem for Linux (WSL) too.
To follow the tutorial you will need installed the following programs:

[MAFFT](https://mafft.cbrc.jp/alignment/software/)

[TrimAl](http://trimal.cgenomics.org/)

[AliView](https://ormbunkar.se/aliview/)

[IQTREE](http://www.iqtree.org/)

[Figtree](http://tree.bio.ed.ac.uk/software/figtree/)

[Astral3-pro](https://github.com/chaoszhang/ASTER/blob/master/tutorial/astral-pro3.md)

## Trimming alignments 

Let's look at an alignment, open `AT1G01180.fna` with aliview.

How does it look?

We can get rid of problematic sections by using a trimmer.

For a single file we can use:

```
cd ./04_alignments/03_trimmed/06_informed/01_coding_NUC/02_NT
trimal -in AT1G01180.fna -out AT1G01180.fna.trm -automated1
aliview your-alignment.al.trm -automated1 
```
How does it look now?

[Trimal has multiple options for triming that you might want to explore](https://trimal.readthedocs.io/en/latest/usage.html#trimming-methods)

You can run TRIMAL for all alignments, using the `-automated1` in a folder by doing:

```
for file in *.fna; do trimal -in $file -out $file.trm -automated1; done
```

## IQTREE
### Inferring gene trees

We can need to infer gene trees for each on the genes in the  data. Notice that we will use GTR+G to save some time from model testing. In the same folder
```
for file in *.trm; do iqtree3 -bb 1000 -s $file -m GTR+G; done
```

Let's take a look at one tree. You can look at the results using FIGTREE


## ASTRAL-PRO

The first step for using Astral-pro3 is to create a single file that contains all the trees. To create a single tree we can use `cat` and a wildcards in the following way:

```
cat *.treefile > nc_354.tre
```

Let's check that the concatenation of tree files worked:

```
cat nc_354.tre
```

We can now run Astral-pro3:

```
./astral-pro3 -i nc_354.tre -o nc_astral.tre
```

We can now see the tree in FIGTREE
