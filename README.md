# **Study CD45+ tumor infiltrate of mouse tumors with single-cell RNA-seq**

The scripts developed for the single-cell RNA-seq study in Aitziber Buqué _et al._ **_"Immunosurveillance of HR+ 1 breast cancer - Prophylactic and therapeutic effects of nicotinamide"_**. It focuses on comparing control vs. nicotinamide (NAM) treated CD45+ tumor infiltrate of mouse tumors with single-cell RNA-seq.

## **Requirements**

* R (tested in R version 3.5.2 (2018-12-20) -- "Eggshell Igloo")
* R librarys: DropletUtils, IRanges, R.methodsS3, Biobase, BiocGenerics, BiocParallel, cowplot, DelayedArray, dplyr, fgsea, GenomeInfoDb, GenomicRanges, ggplot2, ggpubr, gplots, harmony, kableExtra, magrittr, Matrix, matrixStats, pheatmap, R.oo, R.utils, Rcpp, readr, reshape2, S4Vectors, scater, Seurat, SingleCellExperiment, SingleR, SummarizedExperiment, tidyr

## **Data**

Raw counts data and processed Seurat object will be released after final publication.

## **Reproduce results**

#### **1-3. Data preprocess**

[1 QC.R](https://github.com/nyuhuyang/scRNAseq-Immunosurveillance/blob/master/R/Rscript/QC.R)

Download cell ranger results in `~/Downloads` folder. Open terminal, enter the _Code_ root directory and run `Rscript R/Rscript/QC.R "doc/190125_scRNAseq_info.xlsx"`. This script moves all necessary files to `data` folder, run initial quality control. Output ggplot is stored in `output/{date}/g1_2_{date}.Rda`

[2 scater.R](https://github.com/nyuhuyang/scRNAseq-Immunosurveillance/blob/master/R/Rscript/scater.R)

Run `Rscript R/Rscript/scater.R "doc/190125_scRNAseq_info.xlsx"` in terminal. This script uses scater pipeline to to process the single-cell RNA-seq data. The output SingleCellExperiment object is stored in `data/sce_2_{date}.Rda`

[3 Seurat_setup.R](https://github.com/nyuhuyang/scRNAseq-Immunosurveillance/blob/master/R/Rscript/Seurat_setup.R)

Run `Rscript R/Rscript/Seurat_setup.R "doc/190125_scRNAseq_info.xlsx"` in terminal. This script uses Seurat 2 to read the result from the previous scater pipeline, perform normalization, scaling, dimension reduction, and unsupervised-clustering. The output Seurat object is saved in file `data/MouseTumor_2_{date}.Rda`

Above three scripts assume they are run on the same date. Otherwise the `{date}` in the file names must be updated accordingly.


#### **4-5. Identify cell types**

[4 Identify_Cell_Types_Manually.R](https://github.com/nyuhuyang/scRNAseq-Immunosurveillance/blob/master/R/Rscript/Identify_Cell_Types_Manually.R)

Run `Rscript R/Rscript/Identify_Cell_Types_Manually.R "data/MouseTumor_2_{date}.Rda"`. This script use predefinde cell type markers to manually identify cell types.

In addition, an R shiny app is built to identify cell types. [scRNAseq-Immunosurveillance](https://weillcornellmed.shinyapps.io/scRNAseq-Immunosurveillance/)


[5 SingleR.R](https://github.com/nyuhuyang/scRNAseq-Immunosurveillance/blob/master/R/Rscript/SingleR.R)

Run `Rscript R/Rscript/SingleR.R "data/MouseTumor_2_{date}.Rda"`. This script use SingleR package to identify cell types based reference datasets.

#### **6-9. Characterize CD45+ tumor infiltrate of mouse tumors**

[6 Figures.R](https://github.com/nyuhuyang/scRNAseq-Immunosurveillance/blob/master/R/Figures.R), removing unwanted cells, relabel the clustering, generate tsne plot and bar plots. 

6.1. Generate TSNE plots and compare gene expression in individual cell types (figure 8a, b)

After completing step 4,5 and identified all cell types, here we will compare specific gene expression in different cell types.

Use the results from step 4 and 5. Below figure shows all cell types identified.
![](https://github.com/nyuhuyang/scRNAseq-Immunosurveillance/blob/master/Figs/T%20cells_Monocytes.jpeg)

Keep Macrophages, Monocytes, T, NK cells and remove all other cell types.
![](https://github.com/nyuhuyang/scRNAseq-Immunosurveillance/blob/master/Figs/T%20cells_NK%20cells.jpeg)

Use the scripts in bar chart section and generate multiple figures representing gene expression in each individual cell type, like below
![](https://github.com/nyuhuyang/scRNAseq-Immunosurveillance/blob/master/Figs/Macrophages_H2-D1.jpeg)

[7 Differential_analysis.R](https://github.com/nyuhuyang/scRNAseq-Immunosurveillance/blob/master/R/Differential_analysis.R), conducting differential analysis between control and NAM treated tumor sample, and generate heatmaps.

[8 FGESA.R](https://github.com/nyuhuyang/scRNAseq-Immunosurveillance/blob/master/R/FGESA.R), performing gene set enrichment analysis based on the results from previous step 7. 
2. Differential analysis and gene set enrichment analysis (figure S7, S8)

After removing all other unwanted cell types from step 6, we will analyze the gene signature of each cell type and performing GSEA.

conducting differential analysis between control and NAM treated tumor sample, and generate heatmaps like below:
![](https://github.com/nyuhuyang/scRNAseq-Immunosurveillance/blob/master/Figs/Monocytes_NAM%20vs.Monocytes_Control.jpeg)

Performing gene set enrichment analysis based on the results from previous step 7.
![](https://github.com/nyuhuyang/scRNAseq-Immunosurveillance/blob/master/Figs/Hallmark_GSEA_Macrophages.jpeg)
