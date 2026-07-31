# Running CAPTUS:

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

[DOWNLOAD DATA HERE](https://urldefense.com/v3/__https://www.dropbox.com/scl/fo/fch6wr6j9mjxqy1ezat63/ADqE9XkiXWLY1xMShCeMkZQ?rlkey=os9xqpyexlkjijjj9h1y30wqn&st=bd1fsxvg&dl=0__;!!OzdlGbv3!-EhQ-6Rr2tTgJy5Lyb5JdSies7NA3HN84ITtZGqhIFeeEqkmNMVHEXaYVlgcDr6vvuyQrRzpvVuV-TIywGAgtCjMtiAPtQ$)

You don't need to log-in into DROPBOX, simply click on `Or continue with download only` at the bottom of the pop-up window.

Please create a folder called `captus_workshop`, this folder is going to be working directory.

Within `captus_workshop`, create another folder `00_raw_reads`, then move all the read files into this folder.

## First Step: Cleaning the Reads
The data in usage today comes from a [published phylogeny in the Lecythidaceae](https://doi.org/10.3100/hpib.v29iss1.2024.n18
) that employed both target sequencing of custom markers and genome skimming, the latter aiming to obtain chloroplast data. The data has been subsampled so it can be run in personal computers.

Let’s start the analysis with cleaning the raw reads using the clean command. The clean command trims adapter sequences and low-quality bases, and filters out reads with low average quality score.

Navigate inside your terminal to within the `captus_workshop` folder, then:

```
captus clean -r 00_raw
```

Let's look at the output in the folder `01_clean_reads`. Pay special attention to the `HTML` reports `01_qc_stats_before` and `02_qc_stats_after`

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

If for any reason the assembly step did not work [download the assemblies from this link](https://www.dropbox.com/scl/fi/brwtk22kg91vtii9ir0p4/02_assemblies.zip?rlkey=lwfdb5jvjpfun9mmqckuheesw&dl=0)

[Download the ribosomal and nuclear target files here](https://www.dropbox.com/scl/fi/of9zzh7t95i7t0flwy0ps/targets.zip?rlkey=v5wgd1mlagavnnj8joj7obtfr&dl=0)

Place the reference file inside your working directory. Then run the extrating command:
```
captus extract -a 02_assemblies -n targets/Lecy_nuclear_targets.fa -p seedplantsptd -m seedplantsmit -d targets/Lecy_nrDNA_targets.fa
```
Let's look at the results 

<img src="https://github.com/edgardomortiz/captus_26/blob/main/1_running_captus/img/tutorial_basic_extract.png" width="600" >

the FASTA files (`*.faa` and `*.fna`; highlighted in the image above) in each directory store the extracted sequences 

## Fourth Step: Sequence Alignment and Paralog Filtering
Now that we have extracted the sequences out of our assemblies CAPTUS will put them together in matrices and perform clipping and filtering. A great quality of CAPTUS is that it saves intermediate steps allowing the user to acces intermediate matrices. For example, the user can access a locus aligment beefore paralogs are filtered out, so the user can use an alternative method for paralog detection.

Let's run the aligment:
```
captus align -e 03_extractions
```

Let's look at the results 

<img src="https://github.com/edgardomortiz/captus_26/blob/main/1_running_captus/img/tutorial_basic_align.png" width="600" >

[A full explanation of the output can be found here](https://edgardomortiz.github.io/captus.docs/assembly/align/output/)


Let's look at the matrices that still contain paralogs. Since we provided our own targets we need to navigate to:
```
04_alignments/03_trimmed/04_unfiltered/04_misc_DNA/02_matches_flanked
```
These aligments along with `captus-align_astral-pro.tsv` can be used as inputs with ASTRAL-pro.


Now, let's look at the aligments that paralogs filtered out, ready for phylogenetic analysis:
```
04_alignments/03_trimmed/06_informed/04_misc_DNA/02_matches_flanked
```
Captus also produces a nice report:
`captus-align_report.html`

## Fifth: Phylogenetic inference:
While most likely you might need to do some more processing to your alignents, we can inferred a concatenated phylogeny using IQ-TREE:
```
mkdir 05_phylogeny && cd 05_phylogeny
iqtree3 -p ../04_alignments/03_trimmed/06_informed/01_coding_NUC/02_NT -pre concat -T AUTO
```
-p : NEXUS/RAxML partition file or path to a directory with alignments
-pre : Prefix for output files
-T : Number of cores/threads to use or AUTO-detect (default: 1)

Results can be opened in FIGTREE

***Congratulations you have completed the tutorial***


