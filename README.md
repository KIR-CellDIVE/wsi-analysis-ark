# Whole slide image analysis of Cell DIVE multiplex microscopy images using `ark-analysis` toolbox

This repository provides an easy to install and deploy `Apptainer` container for the `ark-analysis` toolbox. This is part of the [DIVE-MAP: Cell DIVE Multiplex Analysis Pipeline](https://github.com/KIR-CellDIVE/DIVE-MAP) and aims to follow the same installation and deployment process like other tools in this pipeline ([whole slide segmentation](https://github.com/KIR-CellDIVE/wsi-segmentation)).

## Installation

### Setup system for building the whole-slide image analysis container
Please follow the steps outlined here [here](https://github.com/KIR-CellDIVE/wsi-analysis) to prepare your computer for building and running the whole-slide image analysis container. You only have to setup you system once to build the various containers that are part of [DIVEMAP](https://github.com/KIR-CellDIVE/DIVE-MAP)

### Build container

We start by creating a `builds` folder in the HOME `~` directory and cloning/downloading this repository from GitHub: 

```bash
mkdir -p ~/builds \
&& cd ~/builds \
&& git clone https://github.com/KIR-CellDIVE/wsi-analysis-ark.git
```
Next, we build a Apptainer container called `wsi_analysis_ark.sif` based on definition file `Apptainer`:

```bash
cd wsi-analysis-ark/singularity \
&& sudo singularity build wsi_analysis_ark.sif Apptainer
```

In order to make it easier to run the container in the future we create to bash scripts `wsi-analysis-ark` in `~/.local/bin` that can simply be called from anywhere inside the console. Adapt these commands if you decided to download and build the container in a different directory. (Skip this step if you'd rather start the containers directly yourself). 

We make sure that `~/.local/bin` exists.
```bash
mkdir -p ~/.local/bin
```
Then, we create two bash scripts in `~/.local/bin` to make starting the container to run the segmentation more straightforward.

```bash
echo "#! /bin/bash
## run wsi-analysis-ark container providing the ark-analysis toolbox
[ -d "/mnt" ] && apptainer \"\$@\" run --bind /mnt:/opt/ark-analysis/templates/drives --bind /:/opt/ark-analysis/templates/host $HOME/builds/wsi-analysis-ark/apptainer/wsi_analysis_ark.sif || apptainer run \"\$@\" --bind /:/opt/ark-analysis/templates/host $HOME/builds/wsi-analysis-ark/apptainer/wsi_analysis_ark.sif" > ~/.local/bin/wsi-analysis-ark
```

Lastly, we make these two bash scripts executable

```bash
chmod +x ~/.local/bin/wsi-analysis-ark
```
and reload the `~/.profile` file to add `~/.local/bin` to `$PATH`.
```bash
source ~/.profile
```



## Run ark-analysis toolbox

If you have followed the installation step you should be able to start the `ark-analysis` toolbox now. If you are on `Windows` and you use `WSL`, first open `PowerShell` and enter the previously created WSL environment `Ubuntu_DIVEMAP` as the user `ubuntu` if you haven't already done so:

```bash
wsl -d Ubuntu_DIVEMAP -u ubuntu
```

Once you are in the `WSL` environment you can run start up the `ark-analysis` toolbox by typing
```bash
wsi-analysis-ark
```


> You can pass additional singularity arguments if you want. For example to bind a results folder to a directory `/data` to make it more easily accessible inside the notebook. In `WSL` the `C:` drive, `D:` drive, etc are mounted and located at `/mnt/c`, `/mnt/d`, etc, respectively. To mount your data folder to `/data` start the notebooks as follows:
>```bash 
> wsi-analysis-ark --bind /path/to/result:/data
>```
>

You should now see a link similar to `http://127.0.0.1:9998/lab/workspaces/lab?reset?token=...`, copy it and open it in your preferred browser. Then, in the left sidebar navigate you can open one of analysis notebooks. Since the notebooks are read-only, save a copy via `File->Download` in the top taskbar and open from the left sidebar before preceding with your analysis. If you are following one from the [whole slide segmentation](https://github.com/KIR-CellDIVE/wsi-segmentation) pipeline you should start with the [2 - "Pixel clustering with pixie" notebook](https://github.com/angelolab/ark-analysis#2-pixel-clustering-with-pixie) of the `ark-analysis` toolbox.

## How to cite

If you use this work was part of your analysis please cite this the original `ark-analysis` repo directly (https://github.com/angelolab/ark-analysis) as well as their relevant accompanying publications:

1. [Greenwald, Miller et al. Whole-cell segmentation of tissue images with human-level performance using large-scale data annotation and deep learning [2021]](https://www.nature.com/articles/s41587-021-01094-0)
2. [Liu et al. Robust phenotyping of highly multiplexed tissue imaging data using pixel-level clustering [2023]](https://doi.org/10.1038/s41467-023-40068-5)
