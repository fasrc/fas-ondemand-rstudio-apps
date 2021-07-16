FROM harvardat/rstudio-master:latest
RUN Rscript -e 'install.packages(c("data.table","dplyr","ggnewscale","msigdbr","pheatmap","Seurat","tibble","VennDiagram"))' \
	&& Rscript -e 'remotes::install_github(c("YuLab-SMU/clusterProfiler.dplyr"), build_vignettes=TRUE)' \
	&& Rscript -e 'BiocManager::install(c("clusterProfiler","DESeq2","edgeR","enrichplot"), ask=False)'