
<!-- README.md is generated from README.Rmd. Please edit that file -->

# DataForIrina

<!-- badges: start -->
<!-- badges: end -->

The goal of DataForIrina is to document the steps taken for the Dataset
provided to Irina

## load needed packages

``` r
library(dplyr)
library(data.table)
library(readxl)
```

we will start by reading the dataset delivered to us from from the
biodiversiy council, this dataset was generated on the 21st of September
of 2022.

``` r
ToClean <- readxl::read_xlsx("2022-09-21.xlsx") 
```

# Filter data

We then filter the dataset to include only plants

``` r
OnlyPlants <- ToClean |> 
  # make all names machine readable
  janitor::clean_names() |>
  # Filter to Kingdom Plantae
  dplyr::filter(rige == "Plantae")
```

Bot not all this have been resolved to species level, in the table
bellow we see the number of taxa that have been resolved to which level

| taxonrang |    n |
|:----------|-----:|
| Art       | 5333 |
| Slægt     | 1562 |
| Hybrid    |  612 |
| Underart  |  460 |
| Familie   |  358 |
| Varietet  |  303 |
| Orden     |  126 |
| Form      |   70 |
| Klasse    |   32 |
| Sektion   |   25 |
| Række     |   10 |
| Superart  |    8 |

We want to keep only some of this, so we only include the ones that are
to the level of Species, Form, Subspecies, and Variety (Art, Form,
Underart, Varietet).

``` r
OnlyPlants <- OnlyPlants |> 
  # Filter only some resolutions
  dplyr::filter(taxonrang %in% c("Art", "Form", "Underart", "Varietet")) |> 
# Ensure no duplecates
  dplyr::distinct()
```

This leaves us with 6,166 valid taxa. this dataset can be downloaded
[here](https://github.com/Sustainscapes/DataForIrina/raw/master/OnlyPlants.xlsx)

## Remove introduced species

Another dataset was made where we removed introduced species

``` r
OnlyNatives <- OnlyPlants |> 
  dplyr::filter(herkomst != "Introduceret" | is.na(herkomst)) 
```

This leaves us with 4,891 valid taxa. this dataset can be downloaded
[here](https://github.com/Sustainscapes/DataForIrina/raw/master/OnlyNatives.xlsx)

# Download Datasets

- [OnlyPlants
  Dataset](https://github.com/Sustainscapes/DataForIrina/raw/master/OnlyPlants.xlsx):
  Filtered dataset including only plant taxa.

- [OnlyNatives
  Dataset](https://github.com/Sustainscapes/DataForIrina/raw/master/OnlyNatives.xlsx):
  Dataset with introduced species removed.
