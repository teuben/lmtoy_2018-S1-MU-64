# 2018-S1-MU-64

This project observed Sextans A in CO 

## OBSNUM

A total of 49 science obsnum's were taken in the CO line (115.3 GHz).
The good data were taken in a 4 night end sprint before Covid shut down the observatory.

More detailed descriptions are in the file **mk_runs**.


## LMTOY Data Reduction

There are two ways to run the SLpipeline, using a different $WORK_LMT directory where the root
of the data processing occurs

1. Use the WORK_LMT that came with where **lmtoy** was installed. This will likely require
   write permission from the owner

   This is the way it runs on Unity.

2. Set WORK_LMT to a directory here in this directory,  something like

              WORK_LMT=`pwd`

   and no permissions in the $LMTOY tree are. Of course you still need to have LMTOY
   installed. The pipeline will then create all  data products in this local directory.

### Creating the run files

A master script **mk_runs** contains all the information on which obsnums are good,
which beams are good, etc.  You always will need to re-run this script to create the
SLpipeline *run* files. The script also uses the (optional) **OBSNUM.args** files, where
arguments specific to this obsnum can be stored. These files should be edited by
a user to create a new "final" dataset. Any optional post-processing after the
pipeline will not be described here (but is of course recommended?).

This command creates the run files (it uses the **mk_runs** scripts):

      make runs
	  
in this case just **m51.run1** and **m51.run2**

### Running the pipeline


With [SLURM](https://slurm.schedmd.com/documentation.html) this is the way:

      sbatch_lmtoy m51.run1
      # wait for it to finish
      sbatch_lmtoy m51.run2

whereas with [Gnu Parallel](https://www.gnu.org/software/parallel/)

      parallel --jobs 16 m51.run1
      parallel --jobs 16 m51.run2

can be submitted in a shell as the seond one will wait until the first one has finished
all pipeline calls. On "lma" this takes about 30 minutes to process all single obsnums
(run1) and a few combination maps (run2)

If you have no good parallel/batch processing available, the slow and trusted way is
via your [unix shell](https://www.gnu.org/software/bash/):

      bash m51.run1
      bash m51.run2

but this will take a while of course (5:04 hours on "lma"):

### Alternate ProjectId's

For the M51 project by accident the ProjectId during one night (2020-02-13, see **lmtinfo.log**)
was given the wrong name. In addition one pointing observation was stored under
2018S1SEQUOIACommissioning.

It may in fact be diserable to add certain pointing observations to the dataset, to confirm
that pointing was optimal. most (all?) osbervations have (or should have) a regular pointing done
before, maybe during, and after an observation. In theory one could use a cross-correlatoin
technique to convince oneselves that the data are safe to stack.  *we will be looking into this*


## Files:


Description of the file that should be in this directory


      lmtinfo.log     logfile from lmtinfo.py on all relevant science observations
      mk_runs         script to make the run files
      m51.run1        created by mk_runs
      m51.run2        created by mk_runs
      2018-S1-MU-64/  (optional) directory with pipeline results, otherwise in $WORK_LMT