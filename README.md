<img src="img/pbtk.png" alt="pbtk logo" width="120px" align="right"/>
<h1 align="center">pbtk</h1>
<p align="center">PacBio BAM toolkit</p>

***

## Availability

Latest version can be installed via bioconda package `pbtk`.

Please refer to our [official pbbioconda page](https://github.com/PacificBiosciences/pbbioconda)
for information on Installation, Support, License, Copyright, and Disclaimer.

## Tools

This repository is replacing individual tool repositories and binaries from `pbbam`.
In bioconda, `pbtk` is a dependency of `pbbam`, so you won't see immediately
that those binaries are longer from `pbbam` directly.

 *  `bam2fasta`
 *  `bam2fastq`
 *  `ccs-kinetics-bystrandify`
 *  `extracthifi`
 *  `pbindex`
 *  `pbindexdump`
 *  `pbmerge`
 *  `zmwfilter`

## Usage

### `bam2fastx`

Tools `bam2fasta` and `bam2fastq` have identical interfaces and transform multiple PacBio BAM and/or DataSet XML files into a compressed FASTA or FASTQ file, respectively:
```
# generates out.fasta.gz
bam2fasta -o out in.bam
bam2fasta -o out in.xml

# generates out.fastq.gz
bam2fastq -o out in_1.bam in_2.bam in_3.xml in_4.bam
```
Option `-u` disables compression (drops .gz extension), while option `-c <int>` determines the Gzip compression level.

Option `-p/--seqid-prefix <str>` adds the provided prefix to each sequence header.

Additionally, input files can be split depending on barcode pairs into multiple files:
```
# generates multiple out.{barcode}_{barcodePair}.fasta.gz
bam2fasta --split-barcodes -o out in1.bam in2.bam
```

### `ccs-kinetics-bystrandify`

Converts a PacBio BAM or DataSet XML file containing CCS kinetics tags to a pseudo-bystrand file with `pw` and `ip` tags that can be used as a substitute for subreads in applications expecting such kinetics information:
```
ccs-kinetics-bystrandify in.bam out.bam
ccs-kinetics-bystrandify in.xml out.xml
```

Option `--min-coverage <int>` specifies the minimum number of passes per strand (tags `fn` and `rn`) for creating a strand-specific read.

### `extracthifi`

Simple tool for extracting reads with accuracy above QV 20 (0.99) from a given BAM file:
```
extracthifi in.bam out.bam
```

### `pbindex`

Minimalistic tool which creates an index file that enables random access into PacBio BAM files:
```
# generates in.bam.pbi
pbindex in.bam
```

### `pbindexdump`

Tool which transforms PBI files to JSON or c++ format:
```
pbindexdump in.bam.pbi > out.json
pbindexdump --format cpp in.bam.pbi > out.cpp
```

Option `--json-indent-level <int>` defines the indentation of the JSON file, while option `--json-raw` modifies the output JSON file to more closely reflect the PBI file format.

Alternatively, hole numbers in plain text can be reported with:
```
pbindexdump --zmws-only in.bam.pbi > out.txt
```
**Note:** in case of subreads, the output text file can contain multiple equal hole numbers (as opposed to `zmwfilter --show-all` which reports only unique ones).

### `pbmerge`

Simple tool which merges several PacBio BAM files together, either by providing them on the command line, a DataSet XML or a file containing one file name per line:
```
pbmerge in1.bam in2.bam in3.bam > out.bam
pbmerge -o out.bam in.xml
pbmerge in.fofn > out.bam
```

Option `--no-pbi` disables creation of the index file.

### `zmwfilter`

Utility tool for filtering PacBio BAM, DataSet XML or FASTX files. Plain filtering based on ZMW hole numbers is supported for any input format, given that the output format is the same, by providing an include list or an exclude list. That can be either in form of a comma separated list on the command line or a single file containing one hole number per line:
```
zmwfilter --include 1,2,4,8,16 in.bam out.bam
zmwfilter --include hole_numbers.txt in.fasta out.fasta

zmwfilter --exclude 42 in.xml out.bam
zmwfilter --exclude hole_numbers.txt in.xml out.fastq
```

ZMW hole numbers present in a PacBio file can be obtained with option `--show-all` and without providing an output file:
```
zmwfilter --show-all in.bam > out.txt
```

**Note:** Functionality described below is for BAM and DataSet XML files only.

Filtering reads by their names can be achieved by providing a file which contains one read name per line (following PacBio query template name convention):
```
zmwfilter --names read_names.txt in.bam out.bam
```

BAM files can also be randomly downsampled to a provided number of ZMWs or to a fraction of the total count (for reproducibility use a fixed seed):
```
zmwfilter --downsample 0.333 in.xml out.bam
zmwfilter --downsample-count 1024 --downsample-seed 42 in.bam out.bam
```

Additionally, filtering can be constrained by providing a minimal number of passes (incompatible with `--names <str>`):
```
zmwfilter --num-passes 2 --include hole_numbers.txt in.bam out.bam
zmwfilter --num-passes 4 --downsample 0.333 in.bam out.bam
```

**Note**: options `--include <str>`, `--exclude <str>`, `--show-all`, `--names <str>`, `--downsample <float>` and `--downsample-count <int>` are all mutually exclusive!

## Changelog

 * **3.1.1**
   * SMRT Link v13 release
   * Fix `ccs-bystrandify-kinetics` output

 * 3.0.0
   * Add `zmwfilter —show-all`
   * Add `pbindexdump —zmws-only`
   * Add `REVIO` platform

 * 1.0.0
   * Gather all tools into `pbtk`
