# FAS OnDemand Rstudio Apps

This repository contains course-specific rstudio applications for Open OnDemand defined in a declarative configuration and programmatically generated. 

The [fas-ondemand-rstudio](https://github.com/fasrc/fas-ondemand-rstudio.git) repo is used as a base for all apps.

## Adding new apps

1. Add a new entry to `apps.json` (see examples).
2. Run the `update.py` script to create the app folder(s). 
3. _Optional_: Build and test the docker image locally.
4. Commit and push the changes.

Notes:
- Existing apps will not be modified by the process above. This merely bootstraps new apps from a template repo. They can be modified manually from that point. Or you can start over by deleting the app folder and running the update process again.
- The `form.yml` will need to be updated manually with the correct image for deployment to the RC academic cluster. This will only be known after the image has been built and pushed to docker hub.

## Github Actions

Github Actions will automatically build an image if the app is pushed and it contains a `Dockerfile`. Once published to docker hub at [harvardat](https://hub.docker.com/u/harvardat), it will be available to be pulled and converted to a singularity image on the RC academic cluster. 

## Updating apps.json

**Example 1**: allow user to select cpu/memory, but no custom packages

```
{
    "apps": [
        {
            "app_name": "fas-rstudio-demo",
            "cpu": {
                "max": 16,
                "min": 1,
                "select": true,
                "value": 4
            },
            "docker_image": "harvardat/rstudio-master:latest",
            "memory": {
                "max": 32,
                "min": 4,
                "select": true,
                "value": 8
            },
            "title": "Rstudio Server - Demo"
        }
    ]
}
```

**Example 2** hard-code the cpu/memory and add custom packages

```
{
    "apps": [
        {
            "app_name": "fas-rstudio-demo2",
            "cpu": {
                "value": 8
            },
            "docker_image": "harvardat/rstudio-demo2:1e9e2ca8",
            "memory": {
                "value": 16
            },
            "packages": {
                "bioc": [
                    "clusterProfiler",
                    "DESeq2",
                    "edgeR",
                    "enrichplot"
                ],
                "cran": [
                    "data.table",
                    "dplyr",
                    "ggnewscale",
                    "msigdbr",
                    "pheatmap",
                    "Seurat",
                    "tibble",
                    "VennDiagram"
                ],
                "github": [
                    "YuLab-SMU/clusterProfiler.dplyr"
                ]
            },
            "title": "Rstudio Server - Demo2"
        }
    ]
}
```


To format `apps.json` after editing, run the folowing: 

```
$ python -mjson.tool < apps.json | sponge apps.json
```

## Building and testing docker images

To build and test a docker image locally:

```
$ export APP_NAME=fas-rstudio-demo
$ cd apps/$APP_NAME
$ docker build -t harvardat/$APP_NAME:latest .
$ docker run --rm -p 8787:8787 -e PASSWORD=yourpasswordhere harvardat/$APP_NAME:latest
```

To manually tag and push a docker image:

```
$ export GIT_COMMIT_HASH=$(git log -1 --format=%h)
$ docker tag harvardat/$APP_NAME:latest harvardat/$APP_NAME:$GIT_COMMIT_HASH
$ docker push harvardat/$APP_NAME:$GIT_COMMIT_HASH
$ docker push harvardat/$APP_NAME:latest
```

To print a list of installed R packages in the docker image, create a file named `list_installed.R`:

```
$ cat << '__EOF' >list_installed.R
ip <- as.data.frame(installed.packages()[,c(1,3:4)])
rownames(ip) <- NULL
ip <- ip[is.na(ip$Priority),1:2,drop=FALSE]
print(ip, row.names=FALSE)
__EOF
```

And then run it against the image:

```
$ docker run --rm -i harvardat/$APP_NAME:latest R <list_installed.R
```
