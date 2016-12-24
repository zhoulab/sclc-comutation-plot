# gene-pairs

## Quickstart

### HPC

`cd /ufrc/zhou/share/projects/bioinformatics/gene-pairs`

    $ ./run_all_hpc.sh -l [GENE LIST] -a [ALPHA LVL] [-f filter out disjoint pairs]

## Scripts

### `gene_pairs.py`

#### Source files

* `data/samples.txt`
* `data/PercDict.dic`
* `data/GeneNumDict.dic`
* `data/genenonsilent200.txt`
* `GenePairsSig/{list_name}.txt`

### `gene_pairs_fisher.R`

#### Source files

* **GenePairsNum** file (generated by `gene_pairs.py`)

## Input Arguments

* `-l [GENE LIST]`
	* `Sig43List`/`Sig200List`/`MoreThan2`
* `-a [ALPHA LVL]` to filter P_value
* `-f` filter out pairs with 0 common samples

## Output Files

#### Directory: 

* `results/{list_name}/`
* `results/{list_name}_removed_common/` (`-f`)

### GenePairs

* `GenePairs_{list_name}.txt`

Columns:

* `Gene1`
* `Gene1Freq`
* `Gene2`
* `Gene2Freq`
* `PercofSamples`
* `Co_Occurrence` (sort descending)

### GenePairsNum

* `GenePairsNum_{list_name}.txt`
* flat file from `gene_pairs.py` to `gene_pairs_fisher.R`

Columns:

* `Gene1`
* `Gene1Samples`
* `Gene2`
* `Gene2Samples`
* `Common`

### FisherGenePairs

* `FisherGenePairs_{list_name}.txt`
* `FisherGenePairs_{list_name}_{alpha}.txt`

Columns:

* `Gene1`
* `Gene1Samples`
* `Gene2`
* `Gene2Samples`
* `Common`
* `P_value` (sort ascending)
* `Adjusted_p`

## Process

For each gene pair, 4 values are computed:

* `N1 = 228 - (N2 + N3 + N4)`
* `N2 = Gene2Samples - CommonSamples`
* `N3 = Gene1Samples - CommonSamples`
* `N4 = CommonSamples`

A two-sided Fisher's exact test is performed on the matrix:

    N1 N2
    N3 N4

If the `-f` flag is applied, gene pairs are ignored from calculation and output if they share no common samples.

With the user-defined alpha level, significant rows are written to the Fisher file (based on `P_value`). If no alpha level is specified, all gene pairs with common samples will be written.
