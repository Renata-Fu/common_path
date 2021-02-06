# README
This pipeline does gene set enrichment analysis for an input dataset using three pathway databases –– KEGG[1], Reactome[2], and WikiPathways[3], and it identifies pathways that are presented by multiple databases.

## Introduction
Gene set enrichment analysis (GSEA) is a common method in computational biology. GSEA identifies enriched pathways in certain cell populations or under certain conditions, and it is important to our understanding of many illnesses, as well as how to treat them. While there are many pathway databases that can be used in GSEA, most studies fail to analyze results from multiple pathway databases because 1) the process is tedious 2) it is hard to categorize pathways based on the databases since different databases often name one biological pathway differently.

Super3Path streamlines the process of using three databases for GSEA, and shows users pathways presented by multiple databases, so that users do not have to sort through everything themselves.

## Installation
* You can download Super3Path by clicking the green Code" button on the webpage, and then selecting "Download ZIP".
* To download using bash, simply type `git clone` into terminal and the url which can be found by clicking the "Code" button.

## Requirements
* R 4.0.3 or newer
    * other packages will be automatically installed by the first module, download_packages.R
* Python 3.8 or newer
    * Pandas ([installation guide here](https://pandas.pydata.org/pandas-docs/stable/getting_started/install.html))

## Usage
On any computer with installed software (e.g. Anaconda) supporting terminal application, change directory to "Super3Path".
### module 1: download_packages.R
This module downloads necessary R packages for the next module. If a package is already installed, it will be updated.
#### command line
`Rscript download_packages.R`

-------------------------------

### module 2: GSEA_3databases.R
This module does gene set enrichment analysis for a gene expression profile.
It uses pathway databases KEGG, Reactome, and WikiPathways (.gmt files provided in the input_files folder of this package).

This module uses the fgsea package. [4]
#### user input
* a gene expression profile comparing two conditions with microarray
    * must be a .txt file
    * must contain a column with standard gene names
    * must contain a column with log fold change (logFC)
#### command line
`Rscript GSEA_3databases.R [arg1] [arg2] [arg3]`

`arg1`, `arg2`, and `arg3` are mandatory arguments.
* `arg1`: full path to your input file
* `arg2`: index of the log fold change (logFC) column
* `arg3`: index of the standard gene name column
#### optional command line arguments
Type `plot-all` after `arg3` to plot all pathways. Separate the two arguments with a space. If this option is chosen, three .pdf files containing pathways presented by each database will be outputted. This process may take a long time depending on the speed of your device, so please do not try to open the .pdf files before this message appears in your terminal window: "done."

This R script will output three .csv files regardless of whether you choose to plot the pathways or not. These files are inputs for the next script, so please do not delete them. It is also suggested that you not change the names or location of these files, as it may complicate the next command.

-------------------------------

### module 3: common_pathways.py
This module finds pathways common to 2 or more databases. Pathways have different names in different databases, but may represent the same biological process.

This module utilizes Compath's mapping catalog [5]. A slightly modified version of this catalog can be found in the input_files folder.
#### command line
`python3 common_pathways.py`
#### optional command line arguments
* If you changed the name of any input file (.csv files from the last script), please add the full path of the file as an argument. Please enter the paths in the file order: KEGG, Reactome, WikiPathways.
    * If you did not change the name to the first and/or second file, please enter `''` as a placeholder.
Remember to separate all arguments with spaces.

This Python script will output two .csv files. They are, respectively, collections of upregulated and downregulated pathways that are presented by two or three databases.

## Acknowledgements
R. Fu, Y. Bai and Q. -E. Wang, "Computational identification of key pathways and differentially-expressed gene signatures in ovarian cancer stem cells," 2020 IEEE International Conference on Bioinformatics and Biomedicine (BIBM), Seoul, Korea (South), 2020, pp. 1843-1848, doi:10.1109/BIBM49941.2020.9313416.

## Citations
Kanehisa, M., Furumichi, M., Sato, Y., Ishiguro-Watanabe, M., & Tanabe, M. (2020). KEGG: Integrating viruses and cellular organisms. Nucleic Acids Research, 49(D1). doi:10.1093/nar/gkaa970

Jassal, B., Matthews, L., Viteri, G., Gong, C., Lorente, P., Fabregat, A., . . . D’Eustachio, P. (2019). The reactome pathway knowledgebase. Nucleic Acids Research. doi:10.1093/nar/gkz1031

Martens, M., Ammar, A., Riutta, A., Waagmeester, A., Slenter, D., Hanspers, K., . . . Kutmon, M. (2020). WikiPathways: Connecting communities. Nucleic Acids Research, 49(D1). doi:10.1093/nar/gkaa1024

Korotkevich, G., Sukhov, V., Budin, N., Shpak, B., Artyomov, M. N., & Sergushichev, A. (2016). Fast gene set enrichment analysis. doi:10.1101/060012

Domingo-Fernández, D., Hoyt, C. T., Bobis-Álvarez, C., Marín-Llaó, J., & Hofmann-Apitius, M. (2018). ComPath: An ecosystem for exploring, analyzing, and curating mappings across pathway databases. Nature. doi:10.1101/353235
