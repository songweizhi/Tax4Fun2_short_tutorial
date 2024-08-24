
A short tutorial on Tax4Fun2
---

+ Tax4Fun2 was developed by Wemheuer et al. (2020) (https://doi.org/10.1186/s40793-020-00358-7). This is only a short tutorial on how it could be used for functional prediction.
+ I am a former colleague of the Tax4Fun2 developers, this tutorial was prepared with the hope that it would be helpful. 
+ Error reports and suggestions can be posted on the [issue page](https://github.com/songweizhi/Tax4Fun2_short_tutorial/issues).

Installation
---

#### Files needed
+ Source code: [Tax4Fun2_1.1.5.tar.gz](https://zenodo.org/records/10035668)
+ Database file: [Tax4Fun2_ReferenceData_v2.tar.gz](https://zenodo.org/records/10035668)

      # You'll need to decompress it before performing functional prediction
      $ tar xzvf Tax4Fun2_ReferenceData_v2.tar.gz

+ Example input files: [example_input_files](https://github.com/songweizhi/Tax4Fun2_short_tutorial/tree/master/example_input_files)

#### In Rstudio

+ open RStudio > Tools > install packages > from "Install from" select "Package Archive File" > choose "Tax4Fun2_1.1.5.tar.gz" > click "Install"

#### In Terminal (Linux and Mac)

    $ R
    $ install.packages(pkgs="Tax4Fun2_1.1.5.tar.gz", repos=NULL, source=TRUE)

+ If you're running Tax4Fun2 on **Windows**, you might found [this](https://github.com/songweizhi/Tax4Fun2_short_tutorial/issues/2) helpful.

Example commands
---

1. [Making functional predictions using the default database](db_default.md)

2. [Making functional predictions using the default database + a user-generated database](db_default_user.md)

