FROM rocker/verse:4.2.2

## adding stuff from  https://github.com/Bioconductor/bioconductor_docker/blob/master/Dockerfile

RUN rm -f /var/lib/dpkg/available && apt clean && dpkg --clear-avail

# This is to avoid the error
# 'debconf: unable to initialize frontend: Dialog'
ENV DEBIAN_FRONTEND noninteractive

# This is to ensure that the image has access to rstudio commands
ENV PATH="${PATH}:/usr/lib/rstudio-server/bin"

# Update apt-get
RUN apt-get update \
	&& apt-get install -y --no-install-recommends apt-utils \
	&& apt-get install -y --no-install-recommends \
	## Basic deps
	gdb \
	libxml2-dev \
	python3-pip \
	libz-dev \
	liblzma-dev \
	libbz2-dev \
	libpng-dev \
	## sys deps from bioc_full
	pkg-config \
	fortran77-compiler \
	byacc \
	automake \
	curl \
	## This section installs libraries
	libpcre2-dev \
	libnetcdf-dev \
	libhdf5-serial-dev \
	libfftw3-dev \
	libopenbabel-dev \
	libopenmpi-dev \
	libxt-dev \
	libudunits2-dev \
	libgeos-dev \
	libproj-dev \
	libcairo2-dev \
	libtiff5-dev \
	libjpeg-dev \
	libjpeg-turbo8-dev \
	libjpeg8-dev \
	libreadline-dev \
	libgsl0-dev \
	libgslcblas0 \
	libgtk2.0-dev \
	libgl1-mesa-dev \
	libglu1-mesa-dev \
	libgmp3-dev \
	libhdf5-dev \
	libncurses-dev \
	libbz2-dev \
	libxpm-dev \
	liblapack-dev \
	libv8-dev \
	libgtkmm-2.4-dev \
	libmpfr-dev \
	libmodule-build-perl \
	libapparmor-dev \
	libprotoc-dev \
	librdf0-dev \
	libmagick++-dev \
	libsasl2-dev \
	libpoppler-cpp-dev \
	libprotobuf-dev \
	libpq-dev \
	libperl-dev \
	## software - perl extentions and modules
	libarchive-extract-perl \
	libfile-copy-recursive-perl \
	libcgi-pm-perl \
	libdbi-perl \
	libdbd-mysql-perl \
	libxml-simple-perl \
	libmysqlclient-dev \
	default-libmysqlclient-dev \
	libgdal-dev \
	## new libs
	libglpk-dev \
	## Databases and other software
	sqlite \
	openmpi-bin \
	mpi-default-bin \
	openmpi-common \
	openmpi-doc \
	tcl8.6-dev \
	tk-dev \
	default-jdk \
	imagemagick \
	tabix \
	ggobi \
	graphviz \
	protobuf-compiler \
	jags \
	## Additional resources
	xfonts-100dpi \
	xfonts-75dpi \
	biber \
	&& apt-get clean \
	&& rm -rf /var/lib/apt/lists/*

## Python installations
RUN python3 -m pip install scikit-learn pandas pyyaml cwltool

## add a couple of collections of latex style files
RUN tlmgr update --self \
	&& tlmgr install collection-latex \
	&& tlmgr install collection-latexrecommended \
	&& tlmgr install collection-latexextra

## Explicitly requested fonts for TeX Live
#
# NOTE: the "arial-urw" font package cannot be installed with tlmgr
# like other fonts. See also: https://tug.org/fonts/getnonfreefonts/
#
RUN tlmgr install fira lato roboto \
	&& curl -OL http://www.tug.org/fonts/getnonfreefonts/install-getnonfreefonts \
  	&& texlua ./install-getnonfreefonts \
  	&& rm -f install-getnonfreefonts \
	&& getnonfreefonts --sys arial-urw

## Explicitly requested packages
#
# For example:
#
#  RUN install2.r corrplot learnr
#  RUN Rscript -e 'devtools::install_github("OI-Biostat/oi_biostat_data")' \
#        && Rscript -e 'devtools::install_github("rstudio-education/gradethis")' \
#        && Rscript -e 'devtools::install_github("kosukeimai/qss-package", build_vignettes=TRUE)' \
#        && Rscript -e 'devtools::install_github("rundel/learnrhash")' \
#        && Rscript -e 'devtools::install_github("davidkane9/PPBDS.data")'

RUN install2.r --error --skipinstalled \
	BayesLCA \
	BiocManager \
	Hmisc \
	MASS \
	R.matlab \
	astrochron \
	boot \
	caTools \
	caret \
	coin \
	corrplot \
	curl \
	data.table \
	economiccomplexity \
	fields \
	fs \
	gapminder \
	glmnet \
	igraph \
	infer \
	learnr \
	lme4 \
	matrixcalc \
	mcmc \
	moderndive \
	multitaper \
	mvtnorm \
	neuralnet \
	pdxTrees \
	perm \
	pheatmap \
	pracma \
	quantmod \
	quantreg \
	randomForest \
	raster \
	remotes \
	reshape \
	rgdal \
	rpart \
	rpart.plot \
	rstan \
	sandwich \
	sf \
	tidymodels \
	tidyverse \
	# STAT 100 Spring 2023
	babynames \
	cowplot \
	GGally \
	gglm \
	ggrepel \
	knitr \
	Lock5Data \
	mosaic \
	mosaicData \
	NHANES \
	palmerpenguins \
	rmarkdown \
	Sleuth3 \
	tinytex \
	broom \
	devtools \
	janitor \
	lubridate \
	markdown \
	nycflights13 \
	patchwork \
	reprex \
	# END STAT 100 Spring 2023 requirements
	#STAT 108 Spring 2023 requirements
	kable \
	kableextra \
	gt \
	ggimage \
	RColorBrewer \
	ggthemes \
	gganimate \
	plotly \
	gitcreds \
	leaflet \
	tidytext \
	shiny \
	shinydashboard \
	flexdashboard \
	usethis \
	assertthat \
	testthat \
	distill \
	httr \
	rvest
	# END STAT 108 Spring 2023 requirements
