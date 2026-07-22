# Running CAPTUS:

**Intro ppt**

Let's make sure CAPTUS is installed:
```
conda activate captus
``` 

Help can be accessed by:
```
captus -h
```

## Imput prep

In general, a good tip for renaming your samples is to think on how you want the names in your final phylogenetic tree. If names are too long or subject to change, you can use codes that later can be replaced by names, but this requires some programming skills.

- The only special characters that are safe to use in the sample name are `-`, and `_` (`_` is commonly used to replace spaces in many phylogenetic programs).
- Do not use spaces or other special characters (e.g. ! " # $ % & ( ) * + , . / : ; < = > ? @ ] [ \ ^ { | } ~).
- Do not use double undescore `__` as CAPTUS uses them internally. 

The format used by CAPTUS looks like this:

<img src="https://github.com/edgardomortiz/captus_26/blob/main/1_running_captus/img/fastq.png" width="600" >

- Any text found before the `_R#` pattern and the extension will become your sample name (`Pouteria_lucuma_EO9854` in this case).
If you are using paired-end reads, your R1 and R2 filenames should contain the patterns `_R1` and `_R2` respectively to be correctly matched and used as pairs.
- **important** For single-end your filenames should still contain `_R1`.
- These are the valid extensions: `.fq`, `.fastq`, `.fq.gz`, and `.fastq.gz`.

## Downloading the data

This data has been specially prepared for this workshop, it has been subsampled from published data available in GenBank

DROPBOX LINK

within your project folder, create another folder `00_raw_reads`, then move all the read files into this folder

## First Step: Cleaning the Reads

Let’s start the analysis with cleaning the raw reads using the clean command. The clean command trims adapter sequences and low-quality bases, and filters out reads with low average quality score.
```
captus clean -r 00_raw_reads
```

Let's look at the output

## Second Step: *De Novo* Assembly 

Run the following command to perform de novo assembly for all the samples with default settings optimized for targeted-capture and genome skimming data.
```
captus assemble -r 01_clean_reads
```

Let's look at the output in the newly created folder `02_assemblies`

<img src="https://github.com/edgardomortiz/captus_26/blob/main/1_running_captus/img/tutorial_basic_assemble.png" width="600" >

## Third Step: Extract Target Sequences

A reference target file in fasta format is needed before we can do the extraction of our target. The file can contain multiple references for a single region, for example samples from multiples species from the same locus. Formatting is important so CAPTUS can interpret the file correctly. CAPTUS can interpret DNA and aminoacid sequences.

When multiple reference sequences per locus are found in the reference dataset, CAPTUS will evaluate during the extraction which of those references matches your assembly best based on similarity and total recovered length percentage.

Here is an example of a reference protein dataset that has two loci (called accD and cemA) with five reference sequences each (probably coming from different taxa to expand phylogenetic coverage).

```
>AA-S46062.1-accD [cluster_size=80]
MALQSLRGSMRSVVGKRICPLIEYAIFPPLPRIIVYASRRARMQRGNYSLIKKPKKVSTLRQYQSTKSPMYQSLQRICGVREWLNKYCMWKEVDEKDFG*
>AAZ94660.1-accD [cluster_size=17]
MEKRWLNSMLSKGELEYRCRLSKSINSLGPIESEGSIINNMNKNIPSHSDSYNSSYSTVDDLVGIRNFVSYDTFLVRDSNSSSYSIYLDIENQIFEIDN*
>ABH88096.1-accD [cluster_size=3]
MQKWRFNSMLLNRELEYGCEFKESLGPIENTSLNEEPKILSDIHKKINRWDDSDNSSYNSLDYLVGADNIQDFLSDKTFLVRDNKRNSYSIYLDIEKKT*
>ABW20568.1-accD [cluster_size=7]
MQNWINNSFQAEFEQESYFGSLGENSMNPRSGGDRYPEALIIRDITGETSAIYFDITDQILENDTHQTILASPIENDLWAEKDISIDIYRYINELIFYD*
>ACU46588.1-accD [cluster_size=1]
MAKYWFNLMLSYKMLSYNKLEHRCGLSKSMDNLNDLGHIGGNEELILNENDAKKNILGLENYNTHSINYLFDSRNIYNLIYNETFLVRNSNGYHYFVYF*
>QNK04966.1-cemA [cluster_size=1]
MKNKKAFIPLLHITFIVFLPWWIAFLFNKGLESWVINWWNTSKSEIFLNDIQEKNILEKFIELEELLLLDEMIKEYPET*
>QNP0849-5.1-cemA [cluster_size=4]
MTKKKAFTPIFYLSFLLFLPWWIDLLFNKCLRSWPTHWWNTRQSEMFLTTLQEKSLLEKFLELEEFLFLDKIIKKEFET*
>QNP08626.1-cemA [cluster_size=2]
MIKNKVFTPLFYLAFIVFLPWGIYFLLNKCMGSWTTNWWTTRESEILSTNINENSLLEKFIQFEEFLLLDEIIKKDTET*
>QNQ64689.1-cemA [cluster_size=9]
MAKNKICIPFISIVFLPWWISFLFKKDFESWVTNWWNTSKSEILLNDIQEKSILKTFIELEELFLLDEMLKEYPETRLQ*
>QPZ48083.1-cemA [cluster_size=7]
MAKKKAFISLIYLASIVFLPWWLSFTFNKSMESWVKNCWNTGPSENFLNDIEEKIIIKKFIELEELSLFDEILKDYTQD*
```

Here is an explanation of the formatting:

<img src="https://github.com/edgardomortiz/captus_26/blob/main/1_running_captus/img/multi_seq_per_locus.png" width="600" >

- The sequence name (any text found before the first space) can contain multiple `-` characters, but only the last one will become the separator.
- Any text found before the separator will be considered as sequence ID.
- Any text found after the the separator will become the locus name.
- Any text found after the first space is considered the description and this text is optional.

Place the reference file inside your working directory. Then run the extrating command:

```
captus extract -a 02_assemblies -n XXXXXXX
```
Let's look at the results 

<img src="https://github.com/edgardomortiz/captus_26/blob/main/1_running_captus/img/tutorial_basic_extract.png" width="600" >

the FASTA files (`*.faa` and `*.fna`; highlighted in the image above) in each directory store the extracted sequences 

