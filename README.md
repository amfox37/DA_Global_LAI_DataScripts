[![DOI](https://zenodo.org/badge/503078933.svg)](https://zenodo.org/badge/latestdoi/503078933)


The repository includes two folders.

1. DAmodelrun contains:

   a. the key files used to set up and run the freerun (default) and the data assimilation experiments of clm5.
      The files includes set-up scripts: 1) DART_params.csh; 2) CLM5.0.06_f09_assim_LAI_global_setup_bandcheck; 3) CESM2_0_DART_config,
      a script implementing the data assimilation (filter) process: assimilate.csh, 
      and the parameter configuration file for the assimilation programs: input.nml.
      Note that the input.nml.free is the configuration for the freerun, and input.nml.assim is the configuration for the assimilation run.

      The freerun and assimilation run shares the same workflow, meaning both of the experiments were set up using the scripts and file 
      mentioned above. The only difference between them is in the freerun model simulation is evaluated against observation and in the assimilation 
      run the observation is assimilated into the model simulation, which is reflected by the opposite configuration between assimilate_these_obs_types 
      and evaluate_these_obs_types in the two input.nml files.

      The two executable programs filter and fill_inflation_restart in assimilate.csh used during the data assimilation process are created 
      by compiling the source codes in DART (Data Assimilation Research Testbed), a software developed and maintained by Data Assimilation 
      Research Section in National Center for Atmospheric Research. Please refer to https://github.com/NCAR/DART/tree/SWE_repartition_fix for 
      the standard DART code for clm and DART documentation https://docs.dart.ucar.edu/en/clm-swe_pre-release/README.html for more details about 
      the software and those two executable programs.It's noteworthy that preprocess must be compiled and run before the compiling and creation of 
      filter and fill_inflation_restart.

      The version of clm5 used in the study is release-clm5.0.06 which can be cloned from https://github.com/ESCOMP/CTSM/tree/release-clm5.0.06.

      To execute data assimilation of CLM (DART_CLM): 

      Firstly, you need to git clone or download the standard DART code for clm and the official clm code from the links mentioned above;

      Secondly, you need to download the key files under DAmodelrun directory in our repository and put each of them under directories within 
      a structure like this:
      ${dartroot}/models/clm/shell_scripts/cesm2_0/DART_params.csh
      ${dartroot}/models/clm/shell_scripts/cesm2_0/CLM5.0.06_f09_assim_LAI_global_setup_bandcheck
      ${dartroot}/models/clm/shell_scripts/cesm2_0/CESM2_0_DART_config
      ${dartroot}/models/clm/shell_scripts/cesm2_0/assimilate.csh
      ${dartroot}//models/clm/work/input.nml
      copy input.nml.assim to input.nml for assimilation run and copy input.nml.free to input.nml for freerun.
      where ${dartroot} is the path of the directory of standard DART code cloned or downloaded on your local machine or HPC.

      Thirdly, customize the set up scripts, mainly the DART_params.csh. For example, in the file to correct the path of clm code or DART 
      to be the location where you want to download or clone, the path of observation on your machine, and etc. 

      Fourthly, follow the instructions printed on the screen after executing the set up script CLM5.0.06_f09_assim_LAI_global_setup_bandcheck, and run the model.


   b. SourceMods directory which includes the modified source files CNBalanceCheckMod.F90 and PhotosynthesisMod.F90.
      They are modified based on the source files with the same names under the src directory in release-clm5.0.06.
      The modification in CNBalanceCheckMod.F90 is to relax the limit of carbon balance error, and the modification in
      PhotosynthesisMod.F90 is to fix the quadratic solution error bug caused by negative shaded photosynthesis (Danica Lombardozzi & Keith Oleson,
      See https://github.com/ESCOMP/CTSM/issues/756 for more details).


   c. The four datm.streams.txt* files with a proprietary format of the text inside record the information of the CAM reanalysis forcing data we used. 
      Check here https://rda.ucar.edu/datasets/ds199.1/ for more details of the forcing data.


2. The folder Analysis consists of matlab scripts used to plot the figures and table in the manuscript.

   The total size of all mat data and the netcdf data used in the matlab scripts is 13GB. You can download all the mat data from 
   https://data.cyverse.org/dav-anon/iplant/home/huoxl90/DA_Global_LAI_DataScripts_AnalysisData/
   The data stored in mat format are extracted from the model outputs which are in netcdf format.
   Since the model outputs are in a very large size (7TB in total), we decided to preserve them on NCAR's HPC.
   If you have a NCAR HPC account, you can access the model outputs under the campaign storage.
   Go to /glade/campaign/cisl/dares/huoxl/ctsm/clm5.0.06_f09_assim_inf_fitpobsth6updatehist_LAI_e60/run for the data assimilation model output, 
   and /glade/campaign/cisl/dares/huoxl/ctsm/clm5.0.06_f09_freerundecade_LAI_e60/run for the default or freerun model output.
   The netcdf file LAI3g_Bimonthly_2000_2016_CLMgrid.nc contains the GIMMS LAI3g data which is the observation of LAI assimilated into CLM5.

   You can git clone or download the repository to your machine and read the matlab scripts in the Analysis folder.
   In order to execute the matlab scripts you need to download the mat data from the link mentioned above.
   Before you execute the matlab scripts on your machine to generate the figures in the manuscript,
   please modify the basedir in the matlab scrpits to be the path where the mat data downloaded on your machine.



If you have any question, please don't hesitate to reach out to huoxl90@email.arizona.edu or afox@ucar.edu
