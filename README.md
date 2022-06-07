This GitHub repository houses code and input data used in the statistical analyses reported in the manuscript ***Metagenomics of Parkinson’s disease implicates the gut microbiome in multiple disease mechanisms***.

The most current release of this repository is archived at Zenodo: [![DOI](https://zenodo.org/badge/497096789.svg)](https://zenodo.org/badge/latestdoi/497096789)

The following README gives an overview of the overall structure of the repository and important notes about its contents.

## Directory tree for repository
```
Wallen_et_al_PDShotgunAnalysis
|
|-- PDShotgunAnalysis.Rmd -- R markdown file that contains code for performing the analyses
|
|-- PDShotgunAnalysis.nb.html -- html notebook that is generated from rendering the R markdown file;
|                                shows code and outputs of code side by side; must be viewed using a
|                                web browser like safari, chrome, firefox, etc.
|
|-- render.R -- R script that can be used to render the R markdown file
|
|-- render.job -- job script that can be used to submit the render.R Rscript to a SLURM scheduler
|                 on a computing cluster
|
|-- environment -- contains .yml file for setting up a conda environment that contains fastspar
|                  and pandoc, which are programs needed to render the R markdown file in full.
|
|-- input -- contains the following inputs that were used in the analyses:
|             --> relative abundance profiles of MetaPhlAn detected clades for PD and NHC samples
|             --> count profiles of MetaPhlAn detected clades for PD and NHC samples
|             --> count profiles of HUMAnN detected KO groups for PD and NHC samples
|             --> count profiles of HUMAnN detected metabolic pathways for PD and NHC samples
|             --> enterotype profiles for PD and NHC samples
|             --> subject metadata
|             --> read counts remaining after each step of sequence QC
|
|-- output -- contains outputs from code contained in the PDShotgunAnalysis.Rmd file
    |
    |-- 0.Pre_analysis
    |       contains the following data:
    |        --> formatted MetaPhlAn clade profiles broken up by taxonomic level
    |        --> histograms of read count and proportion of reads remaining after each QC step
    |
    |-- 1.Metadata
    |       contains the following data:
    |        --> subject metadata summary stats and differences b/t PD vs NHC for each variable
    |        --> variance inflation factors for each PD-associated variable
    |
    |-- 2.Gut_microbiome_composition
    |       contains the following data:
    |        --> principal component analysis of PD and NHC samples using Aitchison distances
    |        --> PERMANOVA results for PD vs NHC using Aitchison distances
    |        --> chi-squared results for differences in enterotype distribution b/t PD and NHC
    |        --> mosaic plot showing distribution of enterotypes in PD and NHC
    |
    |-- 3.Taxonomic_associations
    |       contains the following data:
    |        --> species differential abundance analysis results for MaAsLin2 and ANCOM-BC +
    |            folder of raw MaAsLin2 output from analysis
    |        --> genus differential abundance analysis results for MaAsLin2 and ANCOM-BC +
    |            folder of raw MaAsLin2 output from analysis
    |        --> counts of elevated/reduced species within each PD-associated genus (heterogeneity)
    |        --> boxplot and fold change plot of 84 PD-associated species
    |        --> MaAsLin2 results for association of 84 PD-associated species adjusting for confounders +
    |            folder of raw MaAsLin2 output from analysis
    |        --> additional analyses results (SILVA Prevotella, cluster 17)
    |
    |-- 4.a.Network_analysis
    |       contains the following data:
    |        --> SparCC input tables for PD and NHC groups
    |        --> SparCC correlation and p-value matrices for PD and NHC groups
    |        --> node file uploaded to Gephi for network visualization
    |        --> edge files for PD and NHC groups uploaded to Gephi for network visualization
    |
    |-- 4.b.Gephi_network_plots
    |       contains the following data:
    |        --> Gephi project files for PD and NHC groups
    |        --> network images exported from Gephi
    |
    |-- 5.Gene_pathway_associations
            contains the following data:
             --> KO group differential abundance analysis results for MaAsLin2 and ANCOM-BC +
                 folder of raw MaAsLin2 output from analysis
             --> pathway differential abundance analysis results for MaAsLin2 and ANCOM-BC +
                 folder of raw MaAsLin2 output from analysis
             --> boxplot and fold change plot of functionally relevant KO groups and pathways
             --> additional analysis results (sporulation KO groups)
```

## Important notes about this repository

### Input data to the analyses are included in this repository
As stated in the directory tree, the input data used in the analyses of this manuscript are located in the `input` directory. Running the code in the `PDShotgunAnalysis.Rmd` file using this input data should give the exact same results as reported in the manuscript. Microbial-based input data included here were derived from raw shotgun metagenomic sequences using a pipeline found at the following GitHub repository: [zwallen/SLURM_Shotgun_Metagenomic_Pipeline](https://github.com/zwallen/SLURM_Shotgun_Metagenomic_Pipeline). Any additional processing steps not covered by the pipeline should be detailed in the methods of the manuscript and in the `PDShotgunAnalysis.Rmd` file. Subject metadata input was collected via questionnaires and manually entered and quality controlled. The metadata file contains one tab for the data itself, followed by a tab that provides a data dictionary for the metadata variables. This is the same metadata that has been uploaded with the raw shotgun sequences to NCBI's SRA repository under BioProject PRJNA834801.

### Dependencies for running code in the R markdown file
See the html notebook file `PDShotgunAnalysis.nb.html` for required R packages and the versions used for the analyses. Additionally, non-R related software that needs to be downloaded and in the users $PATH to render the R markdown file in its entirety is the C++ implementation of SparCC called [FastSpar](https://github.com/scwatts/fastspar), and the program [Pandoc](https://pandoc.org/), which is used by the `render` function in the `rmarkdown` R package to render R markdown files. Additional dependencies may be required depending on how each program is installed. The easiest way I find is to create a conda environment to download these programs into, which should download all their dependencies. See the directory `environment` for a `.yml` file that can be used to build a conda environment that contains these two programs and their dependencies.

### Running time for rendering the whole R markdown file is lengthy
Running time when rendering the entire R markdown file `PDShotgunAnalysis.Rmd` takes a while (hours) mainly due to the analysis of KO groups, and then making sure results of that analysis are outputted in excel as numbers instead of strings. If wanting to render the entire R markdown document on your own, I would recommend running over night.
