# SamBootstrap
Implements SAM analysis for full data sets and related subsets

Sam Bootstrap v0.1 - July 30, 2019

Introduction

The SamBootstrap tool is aimed at implementing SAM (Statistical Analysis of Microarrays) on a data set and on its subsets for bootstrap validation. It is based on the SAMR tool and on some additional shell and PHP scripts.


BOOTSTRAP PROCEDURE

CAVEAT:

The bootstrap procedure is meant to carry out both full data and subgroup SAM analyses.

FOLDERS
CASES/          controls vs all cases (full samples and subsets)
STAGE/          controls vs low stage; controls vs high stage; high stage vs low stage (full samples and subsets)
GRADE/          controls vs low grade; controls vs high grade; high grade vs low grade (full samples and subsets)
MET/            controls vs non metastatic; controls vs metastatic; non metastatic vs metastatic (full samples and subsets)

data/           lists of peaks (input)
subsets/        subset files, listing files to be included in each subset
results/        SAM output
stats/          statistics on outputs


0. CREATE PEAK LISTS ONCE
Copy peak list files for each sample into the data/ folder.
Example:
  Copy files 1*.txt and C*.txt in the data/ folder.
  Files created by running:
    Rscript buildSampleFiles.R


1. CREATE FILE LISTS FOR EACH GROUP
Copy file lists into the main folder.
Example:
  See files all_cases.txt and all_controls.txt
  Done by copyng & pasting from Excel file.


2. CREATE SUBGROUPS AND ALL DATA FILE
Usage:
  bootstrap.php <group1_file_list> <group2_file_list> <subgroup_prefix> [ <list1_size> [<list2_size>] ]
  Default for group sizes: 80% of full size (rounded)
  Create files named <prefix>_NNN.txt in the subsets/ folder for subgroups.
  Create file named all_data.txt in the subsets/ folder for all data.
Example:
  bootstrap.php all_controls.txt all_cases.txt subset
Output:
  Samples in all_controls.txt: 19;
  Samples in all_cases.txt: 173
  No bootstrap size specified. 80% assumed.
  Bootstrap size: 153;
  Samples from all_controls.txt: 15;
  Samples from all_cases.txt: 138


3. RUN SAM
sh goRunSAM.sh <FDR> <subset_group1_size> <subset_group2_size> <full_group1_size> <full_group2_size>
This script launches the runSAM.R program by using Rscript.
The usage of the R script is as follows:
Rscript runSAM.R <input_file> <output_file_high> <output_file_low> <fdr> <group1_size> <group2_size>

4. EXTRACT SUMMARY STATISTICAL DATA
sh getAllStats.sh
This script retrieve statistics for single signals and for the whole analysis.


CONTACTS

Paolo Romano, IRCCS Ospedale Policlinico San Martino, Genova, Italy
Email: paolo.romano@hsanmartino.it

                                        
