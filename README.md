Tax4Fun2


Installation
---

### Install Tax4Fun2
1. Go to RStudio > Tools > install packages
1. from "Install from" select "Package Archive File"
1. choose Tax4Fun2_1.1.5.tar.gz, then click "Install"
1. You'll need to have Tax4Fun2's default database (Tax4Fun2_ReferenceData_v2.tar.gz) to perform functional prediction. Before it can be used, you'll need to decompress it.

### There are two ways of running Tax4Fun2
1. Run Tax4Fun2 with its default database
1. Run Tax4Fun2 with its default database + a self-generated database


Making functional predictions using the default database
---

    # modify the following 8 lines
    pwd_op_folder   = 'Tax4Fun2_output_folder'      # specify output folder
    query_otu_seq   = 'demo_OTU.fasta'              # OTU sequence file
    query_otu_table = 'demo_OTU_table.csv'          # OTU table
    pwd_ref_data    = 'Tax4Fun2_ReferenceData_v2'   # path to Tax4Fun2's default database (i.e., Tax4Fun2_ReferenceData_v2), need to be decompressed before use
    norm_by_cn      = TRUE                          # normalize_by_copy_number (TRUE or FALSE)
    norm_path       = TRUE                          # normalize_pathways (TRUE or FALSE)
    iden            = 0.97                          # min_identity_to_reference, please modify as needed
    num_of_threads  = 6                             # number of CPU cores to use, please modify as needed

    # Run Tax4Fun2
    library(Tax4Fun2)
    runRefBlast(path_to_otus = query_otu_seq, path_to_reference_data = pwd_ref_data, path_to_temp_folder = pwd_op_folder, database_mode = "Ref99NR", use_force = T, num_threads = num_of_threads)
    makeFunctionalPrediction(path_to_otu_table = query_otu_table, path_to_reference_data = pwd_ref_data, path_to_temp_folder = pwd_op_folder, database_mode = "Ref99NR", normalize_by_copy_number = norm_by_cn, min_identity_to_reference = iden, normalize_pathways = norm_path)


Making functional predictions using the default database + a user-generated database
---

1. Please refers to steps in section 3.1 to generate your own database.
1. You'll need to store your reference genomes/MAGs in a folder and provide full path to that folder to Tax4Fun2 with "pwd_user_data".
1. You also need to specify a name to the to-be generated database with "name_user_data".
1. !!! Reference genomes/MAGs without 16S rRNA genes will be ignored in database building.
1. !!! You need to know the copy number of 16S rRNA genes in your reference genomes/MAGs, MarkerMAG might be able to help with this.

### Step 1: generate your own reference database

    # modify the following 4 lines
    pwd_ref_data    = 'Tax4Fun2_ReferenceData_v2'   # path to Tax4Fun2's default database, need to be decompressed before use
    pwd_user_data   = 'genome_folder'               # path to the folder that holds the reference genome/MAG files
    name_user_data  = 'name_of_user_database'       # specify the name of the generated database, specify only the name, do not include path here!
    gnm_ext         = 'fna'                         # extension of the genome files
    
    # Generate your own database
    library(Tax4Fun2)
    extractSSU(genome_folder = pwd_user_data, file_extension = gnm_ext, path_to_reference_data = pwd_ref_data)
    assignFunction(genome_folder = pwd_user_data, file_extension = gnm_ext, path_to_reference_data = pwd_ref_data, num_of_threads = num_threads, fast = TRUE)
    generateUserData(path_to_reference_data = pwd_ref_data, path_to_user_data = pwd_user_data, name_of_user_data = name_user_data, SSU_file_extension = "_16SrRNA.ffn", KEGG_file_extension = "_funPro.txt")

### Step 2: perform functional predictions
    
    # modify the following 10 lines
    pwd_ref_data    = 'Tax4Fun2_ReferenceData_v2'   # need to be the same as in section 3.1
    pwd_user_data   = 'genome_folder'               # need to be the same as in section 3.1
    name_user_data  = 'name_of_user_database'       # need to be the same as in section 3.1, specify only the name, do not include path herepwd_op_folder   = 'Tax4Fun2_output_folder'      # specify output folder
    query_otu_seq   = 'demo_OTU.fasta'              # OTU sequence file
    query_otu_table = 'demo_OTU_table.csv'          # OTU table
    norm_by_cn      = TRUE                          # normalize_by_copy_number (TRUE or FALSE)
    norm_path       = TRUE                          # normalize_pathways (TRUE or FALSE)
    iden            = 0.97                          # min_identity_to_reference, please modify as needed
    num_of_threads  = 6                             # number of CPU cores to use , please modify as needed
    
    # Run Tax4Fun2 with your data included
    library(Tax4Fun2)
    runRefBlast(path_to_otus = query_otu_seq, path_to_reference_data = pwd_ref_data, path_to_temp_folder = pwd_op_folder, database_mode = "Ref99NR", use_force = T, num_threads = num_of_threads, include_user_data = T, path_to_user_data = pwd_user_data, name_of_user_data = name_user_data)
    makeFunctionalPrediction(path_to_otu_table = query_otu_table, path_to_reference_data = pwd_ref_data, path_to_temp_folder = pwd_op_folder, database_mode = "Ref99NR", normalize_by_copy_number = norm_by_cn, min_identity_to_reference = iden, normalize_pathways = norm_path, include_user_data = T, path_to_user_data = pwd_user_data, name_of_user_data = name_user_data)
