
### Making functional predictions using the default database + a user-generated database

#### Notes

1. Please refer to Step 1 to generate your own database.
1. You'll need to store your reference genomes/MAGs in a folder and provide full path to that folder to Tax4Fun2 with "pwd_user_data".
1. You also need to specify a name to the to-be generated database with "name_user_data".
1. Reference genomes/MAGs without 16S rRNA genes will be ignored in database building.
1. For accurate functional profiling, you'll need to know the copy number of 16S rRNA genes in your own reference genomes/MAGs, especially if you specify `norm_by_cn=TRUE` in `makeFunctionalPrediction()`.
1. Considering the above two points, [MarkerMAG](https://github.com/songweizhi/MarkerMAG) might be able to help in 
   + Assigning 16S rRNA genes to MAGs
   + Estimating the copy number of 16S rRNA genes in MAGs


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
