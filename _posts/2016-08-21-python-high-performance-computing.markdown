---
layout: post
title:  "Python High Performance Computing"
date:   Sunday, 21. August 2016 04:51PM 
categories: python
---

Although Python is not that efficient as C/C++ or Fortran, it is very easy-to-use and easy-to-understand and very easy to develop. Lots of Python packages/modules are available for various purposes, making it easy for you to do almost anything you want.

In scientific computing field, as one of the two dominant high-level interpreted languages (MATLAB and Python), Python takes advantages of its open source nature and develops very fast. It is possible to build a program using Python with high performance through various ways, i.e., integration of C/C++ code, multi-processing, distributed computing, etc.

# Parallel computing

## Parallel computing

### Parallel computing

#### Parallel computing

## Single core CPU

This is embarrassing and really out of date (even Raspberry PI equips a multi-core ARM CPU!). Concurrent (time sliced, thus pseodo-parallel) programming is possible, but only if you focus on the start time of all tasks. No real speed up in terms of FLOPS.


## SMP (e.g., multi-core computer)

On a **SMP** (systems with multi-processors), the easiest way is to do multi-threading / multi-processing. Unfortunately, due to the GIL in CPython, it is not possible to get any gain on speed using multi-threading (this is a big issue). Therefore, the multi-processing is preferred. But note that the memory is not shared between processes. 

Available packages/modules:

1. concurrent
1. threading / _thread
1. multiprocessing
1. joblib
1. parallelpython (pp)


## Distributed computing (e.g., computer cluster)
Computer clusters are ubiquitous now. It is possible to get your code evaluated on different nodes. Most packages/modules accept instructions in a task form, then distribute tasks on different cores/nodes. Communications are implemented as PIPEs (in Celery).

In MPI standard (which is the leading standard for message-passing parallel), there are implementations of Point-to-Point / Collective / One-sided communications. Most of them are save and easy-to-use. Among the several available framework, MPI is proved to be one of the most efficient ones. So feel free to use it.

Available packages/modules:

1. parallelpython (pp)
1. Celery
1. mpi4py

## IPython/MapReduce/Hadoop
Although it is possible to have ~100-1000 core in the available computer cluster, it could be harder to own/manage an even larger cluster/cloud (I think to most astronomers only <100 cores are available, through workstation or mini-cluster of computers). So MapReduce and Hadoop is not relevant to small-scale scientific problems. But IPython learns somewhat the ideas of MapReduce/Hadoop. It is good with some experience with ipyparallel (formerly named IPython.parallel, but isolated now). 

The ipyparallel implements a cluster in server-client mode. The way to start is to type `ipcluster start -n 24` in terminal. The point here is that the ipyparallel supports SMP/MPI/ssh/PBS, and also the EC2 cluster.


## Other ways to do parallel
One of the other ways to do parallel is to use **GPU**.


## Performance enhancement by integration of compiled languages
It is inevitable to mention Cython, which is excellent extension of Python to enhance the performance directly. At least without the dynamical type of python objects the code will have 30-70% speed up.

Also, SWIG and F2PY are useful to integrate C/C++ and Fortran into pure Python, respectively.

***

# Other things you want to know

## Torque/PBS

This is not related to Python but the torque/PBS system is ubiquitous on computer clusters in all scales.
To batch your jobs, you have to submit your shell scripts with the command `qsub`.

## MPI

MPI is the leading standard of parallel computing now. MPI can be used in torque/PBS system and jobs can be submitted using `qsub`

References:

* [MPI Forum](https://www.mpi-forum.org/)
* [openMPI](https://www.open-mpi.org/)
* [mpi4py](https://pythonhosted.org/mpi4py/)
* An Introduction to Parallel Programming, Peter S. Pacheco
* Using MPI, second ed., W. Gropp et al.
* Using MPI-2, W. Gropp et al.




