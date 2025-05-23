# MHsketch
Sketch density reduction using MinHashing: We present an efficient parallel algorithm MHsketch that uses MinHashing to reduce sketch density for long read mapping. We are able to implement and test this new multithreaded extension to the code (MPI+OpenMP multithreading). 

![figParallelProcessing](https://github.com/user-attachments/assets/c2ffe14b-891e-407f-a804-0c71e14351b1)


# Citation information:
Tazin Rahman, and Ananth Kalyanaraman. "Density-reducing Jaccard Estimators for
Sketch-based Long Read Applications", Under Review

# Dependencies:
JEM-Alltoallc has the following dependencies:

* MPI library (preferably MPI-3 compatible)
* C++14 (or greater) compliant compiler

# Build:
make ksize=$KMER_SIZE

For example:
make ksize = 15

# Execute:
For the multi-threaded version, set the number of threads:
```
export OMP_NUM_THREADS= $number_of_threads
```
Run JEM-mapper:
```
mpiexec -np $number_of_procs $BINARY -s {Contig_Fasta_File} -q {Long_Read_Fasta_File} -a {A_int_Values_File} -b {B_int_Values_File} -p {Prime_int_Values_File} -r $read_segment length -T $NO_OF_TRIALS
```
```
Input arguments 
* -s: input contigs fasta file
* -q: input long reads fasta file
* -a: For diffent trials we have used a linear congruential hash funtion of the form: (Ax+B)%P, whwere A and B are integers and P is a prime and x is a kmer we want to hash. So, using this parameter, we provide the input values for different A values
* -b: input B values
* -p: input prime numbers
* -r: read segment length
* -T: number of trials
```

For example: if we want to run it on 8 threads and 4 processes:  
export OMP_NUM_THREADS=8  
mpiexec -np 4 ./jem -s ~/Ecoli_reads_100x_contigs.fasta -q ~/Ecoli_reads_10x_long_reads.fasta -a ~/A.txt -b ~/B.txt -p ~/Prime.txt -r 1000 -T 30

Notes:
* This code has been tested on high-performance computing cluster (HPC) with MPI compatibility. For the system we used we had to set the number of processes in the given way. Please change the parameters accordingly.

