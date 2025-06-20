# MHsketch
MHsketch is a high-performance C++ framework for sketch density reduction in long-read mapping workflows. The algorithm of MHsketch is designed to use MinHashing to reduce sketch density for long read mapping. MHsketch supports multiple sketching methods—including minimizers, syncmers, and strobemers—making it flexible for a range of bioinformatics applications. We implemented and tested the multithread execution of the framework (MPI+OpenMP multithreading). 

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
Run MHsketch:
```
mpiexec -np $number_of_procs $BINARY -s {Contig_Fasta_File} -q {Long_Read_Fasta_File} -a {A_int_Values_File} -b {B_int_Values_File} -p {Prime_int_Values_File} -r $read_segment length -T $NO_OF_TRIALS
```
```
Input arguments 
* -c: input contigs fasta file
* -r: input long reads fasta file
* -a: input file for A values for linear congruential hash function of the form [(Ax+B)%P]
* -b: input file for B values for linear congruential hash function of the form [(Ax+B)%P]
* -p: input file for  prime numbers (P) for hash function  [(Ax+B)%P]
* -l: read segment length
* -t: number of trials
* -m: sketching method you want ot use; it can be minimizer, or syncmer, or strobemer
Optional arguments
* -s: if you are using syncmer as the sketching method, then provide the s-size
* -w: if you are using minimizer as the sketching method, then provide the window size; if you are using strobemer as sketching method, provide the w_min size
* -v: if you are using strobemer as sketching method, provide the w_max size
```

For example, if we want to run it on 8 threads and 4 processes:  
export OMP_NUM_THREADS=8  
mpiexec -np 4 ./jem -s ~/Ecoli_reads_100x_contigs.fasta -q ~/Ecoli_reads_10x_long_reads.fasta -a ~/A.txt -b ~/B.txt -p ~/Prime.txt -r 1000 -T 30

Notes:
* This code has been tested on high-performance computing cluster (HPC) with MPI compatibility. For the system we used we had to set the number of processes in the given way. Please change the parameters accordingly.

