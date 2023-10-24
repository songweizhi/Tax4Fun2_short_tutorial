Tax4Fun2: prediction of habitat-specific functional profiles and functional redundancy based on 16S rRNA gene sequences
---

+ Tax4Fun2 was developed by Wemheuer et al. (2020) (https://doi.org/10.1186/s40793-020-00358-7). This is only a short tutorial on how Tax4Fun2 can be installed and executed for functional prediction.
+ I am a former colleague of the Tax4Fun2 developers, this tutorial was prepared with the hope that it would be helpful. Suggestions can be posted on the [issue page](https://github.com/songweizhi/Tax4Fun2_short_tutorial/issues).


Installation
---

#### Download
+ Source code: [Tax4Fun2_1.1.5.tar.gz](https://zenodo.org/records/10035668)
+ Database file: [Tax4Fun2_ReferenceData_v2.tar.gz](https://zenodo.org/records/10035668)
+ Example input files: [example_input_files](https://github.com/songweizhi/Tax4Fun2_short_tutorial/tree/master/example_input_files)

#### In Rstudio

+ open RStudio > Tools > install packages > from "Install from" select "Package Archive File" > choose "Tax4Fun2_1.1.5.tar.gz" > click "Install"

#### In Terminal

    R
    install.packages(pkgs="Tax4Fun2_1.1.5.tar.gz", repos=NULL, source=TRUE)

#### Database file 

+ You'll need to have Tax4Fun2's default database (Tax4Fun2_ReferenceData_v2.tar.gz) to perform functional prediction. 
+ Before it can be used, you'll need to decompress it: `tar xzvf Tax4Fun2_ReferenceData_v2.tar.gz`

Example commands
---

1. [Making functional predictions using the default database](db_default.md)
1. [Making functional predictions using the default database + a user-generated database](db_default_user.md)

