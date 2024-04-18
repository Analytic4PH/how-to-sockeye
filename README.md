# how-to-sockeye

## Quick functions

```print_members```


```
#################################### Allocation members for st-username-1 ####################################
sli (Sears Li) 
moak1 (Maya Oak)  
```

## Slurm job


Example:

```
#!/bin/bash
#SBATCH --account=username                # Allocation code
#SBATCH --nodes=1                         # Number of nodes for each sub-job.
#SBATCH --ntasks-per-node=1               # Number of tasks per node for each sub-job.
#SBATCH --time=X:00:00                    # Estimating X hours of runtime, e.g. X=3   (job will not be complete if actual runtime needed exceeds X)
#SBATCH --mem=YG                          # Estimating Y GB of memory needed, e.g. Y=8  (will not run successfully if actual memory needed exceeds Y)
#SBATCH --output=logs/array_%A_%a.out     # [optional] Redirects output to a unique file for each sub-job.
#SBATCH --error=logs/array_%A_%a.err      # [optional] Redirects error logs to a unique file for each sub-job.
#SBATCH --mail-user=your_email_addr@ca    # [optional] Email address for job notifications
#SBATCH --job-name=nps_job_array          # [optional] Job name
#SBATCH --mail-type=ALL                   # [optional] Email notifications received for ALL job events [other options: E for errors]

export LC_ALL=C; unset LANGUAGE

# get list of input files, input directory is the first passed-in parameter
input_dir=$1
input_files=($input_dir/*mzML)
input_file=${input_files[$SLURM_ARRAY_TASK_ID]}

# Load necessary modules
module load gcc
module load apptainer

# Change to working directory
apptainer exec --nv /arc/project/st-username-1/apptainer/ubuntu.sandbox Rscript /arc/project/st-username-1/git/mass_spec/go.R $input_file
```

