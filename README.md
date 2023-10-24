Tax4Fun2
---

+ [Tax4Fun2](https://doi.org/10.1186/s40793-020-00358-7) was developed by Dr Bernd Wemheuer (the corresponding author). This is only a short tutorial on how Tax4Fun2 can be installed and executed.
+ I am a former colleague of the Tax4Fun2 developers, this tutorial was prepared with the hope that it would be helpful. Suggestions on this tutorial can be posted on the [issue page](https://github.com/songweizhi/Tax4Fun2_short_tutorial/issues)


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


Making functional predictions using the default database
---

    library(Tax4Fun2)

    # modify the following 8 lines as needed
    query_otu_seq   = 'demo_OTU.fasta'              # input OTU sequence file
    query_otu_table = 'demo_OTU_table.csv'          # input OTU table
    pwd_op_folder   = 'Tax4Fun2_output_folder'      # output directory
    pwd_ref_data    = 'Tax4Fun2_ReferenceData_v2'   # path to the default database (i.e., Tax4Fun2_ReferenceData_v2), need to be decompressed before use
    norm_by_cn      = TRUE                          # normalize_by_copy_number (TRUE or FALSE)
    norm_path       = TRUE                          # normalize_pathways (TRUE or FALSE)
    iden            = 0.97                          # min_identity_to_reference, please modify as needed
    num_of_threads  = 6                             # number of CPU cores to use, please modify as needed

    # predict functions
    runRefBlast(path_to_otus = query_otu_seq, path_to_reference_data = pwd_ref_data, path_to_temp_folder = pwd_op_folder, database_mode = "Ref99NR", use_force = T, num_threads = num_of_threads)
    makeFunctionalPrediction(path_to_otu_table = query_otu_table, path_to_reference_data = pwd_ref_data, path_to_temp_folder = pwd_op_folder, database_mode = "Ref99NR", normalize_by_copy_number = norm_by_cn, min_identity_to_reference = iden, normalize_pathways = norm_path)


Making functional predictions using the default database + a user-generated database
---

1. Please refer to Step 1 to generate your own database.
1. You'll need to store your reference genomes/MAGs in a folder and provide full path to that folder to Tax4Fun2 with "pwd_user_data".
1. You also need to specify a name to the to-be generated database with "name_user_data".
1. Reference genomes/MAGs without 16S rRNA genes will be ignored in database building.
1. (provide more details here, why this matters) You need to know the copy number of 16S rRNA genes in your reference genomes/MAGs, [MarkerMAG](https://github.com/songweizhi/MarkerMAG) might be able to help with this.

#### Step 1: generate your own reference database

    library(Tax4Fun2)

    # modify the following 4 lines as needed
    pwd_ref_data   = 'Tax4Fun2_ReferenceData_v2'   # path to Tax4Fun2's default database, need to be decompressed before use
    pwd_user_data  = 'genome_folder'               # path to the folder that holds the reference genome/MAG files
    name_user_data = 'name_of_user_database'       # specify the name of the generated database, specify only the name, do not include path here!
    gnm_ext        = 'fna'                         # extension of the genome files
    
    # Generate your own database
    extractSSU(genome_folder = pwd_user_data, file_extension = gnm_ext, path_to_reference_data = pwd_ref_data)
    assignFunction(genome_folder = pwd_user_data, file_extension = gnm_ext, path_to_reference_data = pwd_ref_data, num_of_threads = num_threads, fast = TRUE)
    generateUserData(path_to_reference_data = pwd_ref_data, path_to_user_data = pwd_user_data, name_of_user_data = name_user_data, SSU_file_extension = "_16SrRNA.ffn", KEGG_file_extension = "_funPro.txt")

#### Step 2: perform functional predictions
    
    library(Tax4Fun2)

    # modify the following 10 lines as needed
    query_otu_seq   = 'demo_OTU.fasta'              # input OTU sequence file
    query_otu_table = 'demo_OTU_table.csv'          # input OTU table
    pwd_op_folder   = 'Tax4Fun2_output_folder'      # output directory
    pwd_ref_data    = 'Tax4Fun2_ReferenceData_v2'   # need to be the same as in step 1
    pwd_user_data   = 'genome_folder'               # need to be the same as in step 1
    name_user_data  = 'name_of_user_database'       # need to be the same as in step 1, specify only the name, do not include path here
    norm_by_cn      = TRUE                          # normalize_by_copy_number (TRUE or FALSE)
    norm_path       = TRUE                          # normalize_pathways (TRUE or FALSE)
    iden            = 0.97                          # min_identity_to_reference, please modify as needed
    num_of_threads  = 6                             # number of CPU cores to use , please modify as needed
    
    # predict functions
    runRefBlast(path_to_otus = query_otu_seq, path_to_reference_data = pwd_ref_data, path_to_temp_folder = pwd_op_folder, database_mode = "Ref99NR", use_force = T, num_threads = num_of_threads, include_user_data = T, path_to_user_data = pwd_user_data, name_of_user_data = name_user_data)
    makeFunctionalPrediction(path_to_otu_table = query_otu_table, path_to_reference_data = pwd_ref_data, path_to_temp_folder = pwd_op_folder, database_mode = "Ref99NR", normalize_by_copy_number = norm_by_cn, min_identity_to_reference = iden, normalize_pathways = norm_path, include_user_data = T, path_to_user_data = pwd_user_data, name_of_user_data = name_user_data)
