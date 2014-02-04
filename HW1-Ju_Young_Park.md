Homework 1 - Ju Young Park
========================================================

### 1. Use the GEOmetabd package to f ind all HCV gene expression data using the Illumina platform submitted by an investigator at Yale. This should be done with a single query, showing the title, the GSE accession number, the GPL accession number and the manufacturer and the description of the platform used.

# Connecting to GEOdatabase

```r
library(GEOmetadb)
```

```
## Loading required package: GEOquery
## Loading required package: Biobase
## Loading required package: BiocGenerics
## Loading required package: parallel
## 
## Attaching package: 'BiocGenerics'
## 
## The following objects are masked from 'package:parallel':
## 
##     clusterApply, clusterApplyLB, clusterCall, clusterEvalQ,
##     clusterExport, clusterMap, parApply, parCapply, parLapply,
##     parLapplyLB, parRapply, parSapply, parSapplyLB
## 
## The following object is masked from 'package:stats':
## 
##     xtabs
## 
## The following objects are masked from 'package:base':
## 
##     anyDuplicated, append, as.data.frame, as.vector, cbind,
##     colnames, duplicated, eval, evalq, Filter, Find, get,
##     intersect, is.unsorted, lapply, Map, mapply, match, mget,
##     order, paste, pmax, pmax.int, pmin, pmin.int, Position, rank,
##     rbind, Reduce, rep.int, rownames, sapply, setdiff, sort,
##     table, tapply, union, unique, unlist
## 
## Welcome to Bioconductor
## 
##     Vignettes contain introductory material; view with
##     'browseVignettes()'. To cite Bioconductor, see
##     'citation("Biobase")', and for packages 'citation("pkgname")'.
## 
## Setting options('download.file.method.GEOquery'='auto')
## Loading required package: RSQLite
## Loading required package: DBI
```

```r
library(RSQLite)
geo_con <- dbConnect(SQLite(), "GEOmetadb.sqlite")
dbListTables(geo_con)
```

```
## character(0)
```

```r
dbListFields(geo_con, "gse_gpl")
```

```
## Error: RS-DBI driver: (error in statement: no such table: gse_gpl)
```


# Finding all HCV gene expression data 

```r
dbGetQuery(geo_con, "SELECT gse.title, gse.gse, gpl.gpl, gpl.manufacturer, gpl.description FROM (gse JOIN gse_gpl ON gse.gse=gse_gpl.gse) j JOIN gpl ON j.gpl=gpl.gpl WHERE gpl.Title LIKE '%Illumina%' AND gse.contact LIKE '%Institute: Yale %'  AND gse.Title LIKE '%HCV%' LIMIT 5;")
```

```
## Error: RS-DBI driver: (error in statement: no such table: gpl)
```


# Results
      gse.title                                                          gse.gse  gpl.gpl
    1 The blood transcriptional signature of chronic HCV [Illumina data] GSE40223 GPL10558
    2 The blood transcriptional signature of chronic HCV                 GSE40224 GPL10558
         
       gpl.manufacturer
    1    Illumina Inc.
    2    Illumina Inc.
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  gpl.description
    1 The HumanHT-12 v4 Expression BeadChip provides high throughput processing of 12 samples per BeadChip without the need for expensive, specialized automation. The BeadChip is designed to support flexible usage across a wide-spectrum of experiments.;\t;\tThe updated content on the HumanHT-12 v4 Expression BeadChips provides more biologically meaningful results through genome-wide transcriptional coverage of well-characterized genes, gene candidates, and splice variants.;\t;\tEach array on the HumanHT-12 v4 Expression BeadChip targets more than 31,000 annotated genes with more than 47,000 probes derived from the National Center for Biotechnology Information Reference Sequence (NCBI) RefSeq Release 38 (November 7, 2009) and other sources.;\t;\tPlease use the GEO Data Submission Report Plug-in v1.0 for Gene Expression which may be downloaded from https://icom.illumina.com/icom/software.ilmn?id=234 to format the normalized and raw data.  These should be submitted as part of a GEOarchive.  Instructions for assembling a GEOarchive may be found at http://www.ncbi.nlm.nih.gov/projects/geo/info/spreadsheet.html;\t;\tOctober 11, 2012: annotation table updated with HumanHT-12_V4_0_R2_15002873_B.txt
    2 The HumanHT-12 v4 Expression BeadChip provides high throughput processing of 12 samples per BeadChip without the need for expensive, specialized automation. The BeadChip is designed to support flexible usage across a wide-spectrum of experiments.;\t;\tThe updated content on the HumanHT-12 v4 Expression BeadChips provides more biologically meaningful results through genome-wide transcriptional coverage of well-characterized genes, gene candidates, and splice variants.;\t;\tEach array on the HumanHT-12 v4 Expression BeadChip targets more than 31,000 annotated genes with more than 47,000 probes derived from the National Center for Biotechnology Information Reference Sequence (NCBI) RefSeq Release 38 (November 7, 2009) and other sources.;\t;\tPlease use the GEO Data Submission Report Plug-in v1.0 for Gene Expression which may be downloaded from https://icom.illumina.com/icom/software.ilmn?id=234 to format the normalized and raw data.  These should be submitted as part of a GEOarchive.  Instructions for assembling a GEOarchive may be found at http://www.ncbi.nlm.nih.gov/projects/geo/info/spreadsheet.html;\t;\tOctober 11, 2012: annotation table updated with HumanHT-12_V4_0_R2_15002873_B.txt


### 2. Reproduce your above query using the data.table package. Again, try to use a single line of code. [Hint: You first need to convert all db tables to data.table tables].
 
# Converting all db tables to data.table tables

```r
library(data.table)
gse.dt <- data.table()
gse_gpl.dt <- gse <- 
setkey(gse.dt, gse)
```

```
## Error: some columns are not in the data.table: gse
```

```r
setkey(gse_gpl.dt, gse)
```

```
## Error: object 'gse_gpl.dt' not found
```

```r
merge1 <- merge(gse.dt, gse_gpl.dt)
```

```
## Error: object 'gse_gpl.dt' not found
```

```r

setkey(merge1, gpl)
```

```
## Error: object 'merge1' not found
```

```r
setkey(gpl.dt, gpl)
```

```
## Error: object 'gpl.dt' not found
```

```r
merge2 <- merge(merge1, gpl.dt)
```

```
## Error: object 'merge1' not found
```


# Reproducing above(#1) query using the data.table package

```r
merge2[title.x %like% "HCV" & contact.x %like% "Yale" & manufacturer %like% 
    "Illumina", list(gse, gpl, manufacturer, description)]
```

```
## Error: object 'merge2' not found
```


# Results
