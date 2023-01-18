FROM huitacademictechnology/fas-rstudio-general:sha-aa5ccb8

# Packages requested by BST 282
RUN install2.r -e circlize factoextra ggfortify pheatmap purrr Seurat Signac ROCR msigdbr
RUN Rscript -e 'BiocManager::install(c("apeglm", "biobroom", "biomaRt", "bladderbatch", "ComplexHeatmap", "DESeq2", "EnsDb.Hsapiens.v75", "fgsea", "GenomeInfoDb", "leukemiasEset", "limma", "sva", "tximport", "tximportData", "hgu133plus2.db"))'