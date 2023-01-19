FROM huitacademictechnology/fas-rstudio-general:sha-aa5ccb8

# Packages requested by BST 282
#
# Note: using BiocManager::install() to install packages from both
# CRAN and BioConductor. If it's not a BioConductor package, it will
# fall back to CRAN automatically

RUN Rscript -e 'BiocManager::install(c("apeglm","biobroom","biomaRt","bladderbatch","circlize","ComplexHeatmap","DESeq2","EnsDb.Hsapiens.v75","factoextra","fgsea","GenomeInfoDb","ggfortify","hgu133plus2.db","leukemiasEset","limma","msigdbr","pheatmap","purrr","ROCR","Seurat","Signac","sva","tximport","tximportData"))'
