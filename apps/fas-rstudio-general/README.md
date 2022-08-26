# FAS Rstudio General Image

This is the default image for Rstudio in FAS OnDemand. It's intended to meet the needs of most data science and research workflows without introducing compatibility issues. If a library is requested during a term and can be added to this repo without conflict, then it should be.

## Base Image

This image is based on the [Rocker Project](https://rocker-project.org/) and in particular the [Version-stable Rocker Images](https://github.com/rocker-org/rocker-versioned2) which install a fixed version of R from source and R packages from a fixed snapshot of CRAN for reproducibility.

See [rocker/verse](https://hub.docker.com/r/rocker/verse/tags) on docker hub.

## R dependencies

Dependencies are specified inline in the `Dockerfile`. The [littler](http://dirk.eddelbuettel.com/code/littler.html) `install2.r` script is useful for installing packages directly from [CRAN](https://cran.r-project.org/) and the `Rscript` utility is useful for executing arbitrary R expressions, such as running `devtools::install_github()` or `BiocManager::install()`.

## Installed packages

The `packages.txt` file contains the list of installed packages in the image, which is useful for checking if a particular package has already been installed or what version is available. It is created by running the following:

```
docker run --rm image:tag Rscript -e "as.data.frame(installed.packages()[,c(1,3)])" >packages.txt
```

## OnDemand Configuration

The `form.yml` file needs to be configured so that the `rstudio_version` references the correct image, keeping in mind that the docker image must be pulled by academic cluster staff, converted to the [Singularity](https://docs.sylabs.io/guides/3.0/user-guide/quick_start.html) image format, and made available to Ondemand in order to use it.

Here's the change that needs to be made in `form.yml`:

```yaml
---
title: Rstudio Server
cluster: "academic"
attributes:
  rstudio_version: "imagename.sif"  # <-- Change Me
```

The naming convention we've adopted for converting docker images to singularity files (`.sif`) is as follows:

```
user/repo:tag -> user_repo_tag.sif
```

For example:

```
harvardat/fas-rstudio-general:sha-7b5663a -> harvardat_fas-rstudio-general_sha-7b5663a.sif
```

Note that the `form.yml` can be updated before the image is pulled to the cluster, but just be aware that the server won't launch successfully until the image is actually available in the cluster.