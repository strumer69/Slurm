# Slurm
## overview:
* https://slurm.schedmd.com/overview.html
* Slurm is an open source, fault-tolerant, and highly scalable cluster management and job scheduling system for large and small Linux clusters.
* Slurm requires **no kernel modifications** for its operation and is relatively self-contained. As a **cluster workload manager**, Slurm has three key functions.
  * First, it allocates exclusive and/or non-exclusive access to resources (compute nodes) to users for some duration of time so they can perform work.
  * Second, it provides a framework for starting, executing, and monitoring work (normally a parallel job) on the set of allocated nodes.
  * Finally, it arbitrates contention for resources by managing a queue of pending work.
# --------------------------------------------------
## LRZ documentation: 
* https://doku.lrz.de/slurm-workload-manager-10745909.html
* sbatch --> submit a job script
* salloc --> create an interactive SLURM shell.
## example:
* make a file named simple_job.sh with the following content:
``` c++
#!/bin/bash
#SBATCH --job-name=simple_test
#SBATCH --output=simple_test.out
#SBATCH --error=simple_test.err
#SBATCH --time=00:10:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --partition=general

echo "Starting job at: $(date)"
echo "This job is running on:"
hostname
echo "Working directory: $(pwd)"

# Sleep for 30 seconds to simulate work
sleep 30

echo "Job completed at: $(date)"
```
### submit it with the following command:
* sbatch simple_job.sh
* Error --> invalid partition specified: general
* solution --> at first  --> List available partitions (queues)
* sinfo
* then change the partition name from **general** to a proper name
### check the jon status :
* squeue -u $USER
### view the output:
* cat simple_test.out
* cat simple_test.err
### cancel a job
* scancel <jobid>
### view detailed job information: 
* scontrol show job <jobid>
## MPI example:
``` c++
#!/bin/bash
#SBATCH --job-name=mpi_test
#SBATCH --output=mpi_test.out
#SBATCH --error=mpi_test.err
#SBATCH --time=00:10:00
#SBATCH --nodes=2
#SBATCH --ntasks-per-node=4
#SBATCH --partition=general
#SBATCH --constraint=avx

module load intel-mpi

echo "Running MPI test on $(hostname)"
mpirun -np 8 ./your_mpi_program
```
* to be continued from the error of running MPI job on LRZ
