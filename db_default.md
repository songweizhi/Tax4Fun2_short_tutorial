
### Making functional predictions using the default database

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
