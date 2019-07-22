
<!-- README.md is generated from README.Rmd. Please edit that file -->

# enrichr.db

The goal of enrichr.db is to provide the data from
[Enrichr](http://amp.pharm.mssm.edu/Enrichr/) in a format that makes it
easy to access the gene sets programmatically from R.

The current Enrichr release included is from 2019-01-23.

## Installation

You can install the released version of enrichr.db from
[Github](https://github.com/clemenshug/enrichr.db) with:

``` r
if (!requireNamespace("remotes", quietly = TRUE))
  install.packages("remotes")
remotes::install_url("https://github.com/clemenshug/enrichr.db/releases/download/v0.1/enrichr.db_0.1.tar.gz")
```

## Example

This is a basic example which shows you how to query the gene set
database:

``` r
library(tidyverse)
#> ── Attaching packages ───────────────────────────────────────────────────────────────────────── tidyverse 1.2.1 ──
#> ✔ ggplot2 3.2.0     ✔ purrr   0.3.2
#> ✔ tibble  2.1.3     ✔ dplyr   0.8.2
#> ✔ tidyr   0.8.3     ✔ stringr 1.4.0
#> ✔ readr   1.3.1     ✔ forcats 0.4.0
#> ── Conflicts ──────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
#> ✖ dplyr::filter() masks stats::filter()
#> ✖ dplyr::lag()    masks stats::lag()
library(enrichr.db)

# Finding all libraries that have something to do with drug treatments
drug_gene_sets <- enrichr_terms %>%
  filter(grepl("drug", library, ignore.case = TRUE))
```

This gives a data frame of gene set libraries. The gene sets are in the
`data` column of the data frame and are implemented as lists. Each list
element is a gene set and contains a vector of HGCN gene
symbols.

| library                                                  | year | n\_terms | gene\_coverage | n\_genes\_per\_term | category       | description                                                          | data               |
| :------------------------------------------------------- | :--- | -------: | -------------: | ------------------: | :------------- | :------------------------------------------------------------------- | :----------------- |
| Drug\_Perturbations\_from\_GEO\_2014                     | 2014 |      701 |          47107 |                 509 | Legacy         | {1} is differentially expressed when the cell is perturbed with {0}. | \<701 gene sets\>  |
| Drug\_Perturbations\_from\_GEO\_down                     | NA   |      906 |          23877 |                 302 | Crowd          | {0} is downregulated by {1}.                                         | \<906 gene sets\>  |
| Drug\_Perturbations\_from\_GEO\_up                       | NA   |      906 |          24350 |                 299 | Crowd          | {0} is upregulated by {1}.                                           | \<906 gene sets\>  |
| DrugMatrix                                               | NA   |     7876 |           5209 |                 300 | Diseases/Drugs | {0} is differentially expressed in sample {1}                        | \<7876 gene sets\> |
| RNA-Seq\_Disease\_Gene\_and\_Drug\_Signatures\_from\_GEO | NA   |     1302 |          22440 |                 505 | Crowd          | {0} is differentially expressed in {1}.                              | \<1302 gene sets\> |

``` r
# Printing 10 genes of the first 5 gene sets in the first library we found (Drug_Perturbations_from_GEO_2014)
print(
  head(drug_gene_sets$data[[1]], n = 5) %>%
    map(head, n = 10)
)
#> $`estradiol_mus musculus_gpl339_gse16854_chdir_down`
#>  [1] "FZD2"   "ANXA2"  "ELOVL5" "TPI1"   "RPS6"   "MIF"    "HSPA4" 
#>  [8] "CYP7B1" "RPSA"   "TGFBR2"
#> 
#> $`letrozole_homo sapiens_gpl3921_gse33366_chdir_down`
#>  [1] "CKMT1A"  "MX2"     "PSMB9"   "SCAMP4"  "SCD"     "PBEF1"   "OPTN"   
#>  [8] "CYFIP2"  "GPATCH2" "ASAH1"  
#> 
#> $`mifepristone_homo sapiens_gpl6947_gse39654_chdir_down`
#>  [1] "ACSL3"     "RPL21"     "ACSL1"     "RPL23"     "CKMT1A"   
#>  [6] "CNBP"      "DEK"       "HSPE1"     "RPL13AP20" "SCD"      
#> 
#> $`tamoxifen_homo sapiens_gpl96_gds2367_chdir_down`
#>  [1] "UBE2C"    "HUWE1"    "HN1"      "HNRNPAB"  "RPSA"     "C20ORF24"
#>  [7] "CDC20"    "RAB31"    "PPIA"     "SCD"     
#> 
#> $`fluorouracil_homo sapiens_gpl550_zr-75-1_gds1627_chdir_up`
#>  [1] "DUSP4"   "NME3"    "AKR1A1"  "TUBB2A"  "AKR7A2"  "PPP1CB"  "MRPL23" 
#>  [8] "TMEM258" "TMCO6"   "KRT19"
```

## Funding

We gratefully acknowledge support by NIH Grant 1U54CA225088-01: Systems
Pharmacology of Therapeutic and Adverse Responses to Immune Checkpoint
and Small Molecule Drugs.
