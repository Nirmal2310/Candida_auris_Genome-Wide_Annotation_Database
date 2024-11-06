# Candida auris Genome-Wide Annotation Database
This repository contains the Annotation Database for Candida auris B11220 and the steps to build the database locally.

### The files required to build this database are in the DATA directory. You can build the database locally using the following R command
```R
library(AnnotationForge)

chr_data <- read.table(file="DATA/chr_names_B11220.txt")

colnames(chr_data) <- c("GID", "CHROMOSOME")

gene_symbol_data <- read.table(file="DATA/gene_symbols_B11220.txt")

colnames(gene_symbol_data) <- c("GID", "SYMBOL", "GENENAME")

gene_ontology_data <- read.table(file="DATA/GO_terms_B11220.txt")

colnames(gene_ontology_data) <- c("GID", "GO", "EVIDENCE")

makeOrgPackage(gene_info=gene_symbol_data, chromosome=chr_data, go=gene_ontology_data, version="0.1",
maintainer="Your Name <yourname@email.com>",
author="Your Name <yourname@email.com>",
outputDir = ".",
tax_id="498019",
genus="Candida",
species="auris.B11220",
goTable="go")

install.packages("./org.Cauris.B11220.eg.db", repos=NULL, type="source")

library(org.Cauris.B11220.eg.db)
```
### To perform enrichment analysis on your data using Candida auris B11220 Genome-Wide Database use the following R code
```R
library(clusterProfiler)
library(org.Cauris.B11220.eg.db)

#### GO Enrichment
ego_bp <- enrichGO(data$GID, ont = "BP", pAdjustMethod = "BH", OrgDb = org.Cauris.B11220.eg.db,
                    pvalueCutoff = 0.05, minGSSize = 3, maxGSSize = 800, readable = T, keyType="GID")

dotplot(ego_bp,showCategory=10, font.size = 12) + theme(axis.text.y=element_text(size=12))

#### KEGG Pathway Analysis

ego_pathway <- enrichKEGG(
    data$GID,
    organism = "caur",
    keyType = "ncbi-geneid",
    pvalueCutoff = 0.05,
    pAdjustMethod = "BH",
    minGSSize = 3,
    maxGSSize = 800,
    qvalueCutoff = 0.2,
    use_internal_data = FALSE
)

dotplot(ego_pathway,showCategory=10, font.size = 12) + theme(axis.text.y=element_text(size=12))

#### Example of GID (Gene ID): "79514224", "40026922", "40026919", "40026918", "40026917", "40026916"

#### Example of Gene Symbol: "CJI96_0000001", "CJI96_0000002", "CJI96_0000005", "CJI96_0000006", "CJI96_0000007", "CJI96_0000008"

```

### You can also use the prebuilt database for Candida auris B11220.

#### Step 1: Unzip the Database directory
```bash
unzip org.Cauris.B11220.eg.db.zip
```
#### Step 2: Install the database in R from local directory.
```R
install.packages("/path/to/the/directory/org.Cauris.B11220.eg.db", repos=NULL, type="source")
library(org.Cauris.B11220.eg.db)
```
