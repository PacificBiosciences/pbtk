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

 *  ccs-kinetics-bystrandify
 *  extracthifi
 *  pbindex
 *  pbindexdump
 *  pbmerge
 *  zmwfilter

## Usage

TBD

## Changelog

 * **3.0.0**
   * Add `zmwfilter —show-all`
   * Add `pbindexdump —zmws-only`
   * Add `REVIO` platform

 * 1.0.0
   * Gather all tools into `pbtk`
