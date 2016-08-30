---
layout: post
title:  "Python High Performance Computing"
date:   Sunday, 21. August 2016 04:51PM
categories: python, HPC
---

Although Python performance is not that efficient as C/C++ or Fortran, it is very easy-to-use and easy-to-understand and very easy to develop.
It may take more time to evaluate the Python code but it takes much less time to implement the same algorithm using Python.
Lots of Python packages/modules are available for various purposes, making it easy for you to do almost anything you want.

In scientific computing field, as one of the two dominant high-level interpreted languages (MATLAB and Python), Python takes advantages of its open source nature and develops very fast. It is possible to build a program using Python with high performance through various ways, i.e., integration of C/C++ code, multi-processing, distributed computing, etc.

There are basically two ways to enhance the performance of Python code:

1. Enhance the code efficiency, which is without using more computing resources
1. Parallel computing, which speeds up through making use of more computing resources

***

## Enhance the code efficiency

### Principles of optimization for high performance

This requires the programmers to have some knowledge of data structure, hardwares and high performance third-party packages.
For example, vectorization makes full use of the CPU cache, thus leading to a significant enhancement of performance. But this requires the programmers to know the array storage format, e.g., in C/Fortran format.
Once the programmers knows the advantages of each data structures in Python, the code could be optimized to have a significant improvement on performance.

### Integrating compiled languages

- **Cython**
It is inevitable to mention Cython, which is excellent extension of Python to enhance the performance directly. At least without the dynamical type of python objects the code will have 30-70% speed up.
- **SWIG & F2PY**
Also, SWIG and F2PY are useful to integrate C/C++ and Fortran into Python, respectively.

### Using LLVM

- **numba/numbapro**

Also, numba/numbapro is a very convenient way to speed up your code without much modifications (this is very important in development).
Using the @numba.jit decorator, even numpy code, whose most CPU-intensive code is written using C/Fortran, can accelerate.
Taking the matrix dot as an example,

```python
import numpy as np
import numba

@numba.jit
def mydot(a, b):
    return np.dot(a, b)

a = np.random.randn(10000, 1000)
b = np.random.randn(1000, 10000)

%timeit np.dot(a, b)
%timeit mydot(a, b)
```

The output is:

```
1 loop, best of 3: 1.03 s per loop
1 loop, best of 3: 870 ms per loop
```

This is at least 10% speed up, and could be significant to the problem.

***

## Parallel computing

### Asynchronous programming

This is the third programming model besides serial and parallel programming model.
It is to launch multiple threads asynchronously / concurrently.
They may or may not be parallel.

Available modules: (Python 3.x only)

1. concurrent (Python 3.2+)
 - concurrent.futures
1. asyncio
 - asyncio.coroutines
 - asyncio.futures



### Multi-threading

Unfortunately, due to the GIL in CPython, it is impossible to speed up Python program using multi-threading (this is a big issue, although Jython and IronPython are implemented without GIL, and PyPy team is also struggling to solve it).
Only one thread is allowed to access the resources under the interpreter at a time.
If the program is to solve a CPU-bound problem involving no or only little I/O operations, multi-threading gives no speed up.
But for I/O problem, this may be useful to support concurrence.

Available modules:

1. **threading** / **_thread**

To communicate with other threads, use

- threading.Lock
- threading.RLock
- threading.Semaphore
- threading.Condition
- Queue.Queue (much safer, thus recommended)

### Multi-processing (SMP)

Nowadays an SMP (system with multi-processors) are ubiquitous (even the Raspberry PI equips a multi-core ARM CPU).
It is quite easy to write a multi-processing program in Python.

Although a process is heavier than a thread, for CPU-bound problem the overhead is negligible.
So, feel free to use multi-processing in your program.

Available modules/packages:

1. **multiprocessing**
 - multiprocessing.Process
 - multiprocessing.Pool

Communications through:

- multiprocessing.Pipe
- multiprocessing.Queue

To synchronize processes, use

- multiprocessing.Lock
- multiprocessing.Event
- multiprocessing.Condition
- multiprocessing.Semaphore
- multiprocessing.RLock
- multiprocessing.Barrier (Python 3.x)

To manage a state between processes, use

- multiprocessing.Manager


### Distributed computing

#### Pure python ways
Computer clusters are available for many scientists now. It is possible to get your code evaluated on different nodes. Most packages/modules accept instructions in a task form, then distribute tasks on different cores/nodes. Communications are implemented as PIPEs (in Celery).

Available packages/modules:

1. [parallelpython](http://www.parallelpython.com/) (pp)
1. [SCOOP](https://github.com/soravux/scoop) (usage much like mpiexec, supports hostfile)
1. Celery (relies on script files ...)
1. Pyro4 (resambling java Remote Method Invocation)
1. PyCSP (Python Communicating Sequential Processes)
1. RPyC (Remote Python Call)

Most of them are implemented in a client-server style (except PyCSP).
Among them, the first two are suitable for scientific computing.


#### MPI

In MPI standard (which is the leading standard for message-passing parallel), there are implementations of Point-to-Point / Collective / One-sided communications. Most of them are safe and easy-to-use. Among the several available framework, MPI is proved to be one of the most efficient ones. So feel free to use it.

Available packages/modules:

1. mpi4py
 - Point-to-Point communications (send, recv, sendrecv, ...)
 - Collective communications (bcast, scatter, gather, reduce, ...)
 - Topologies

References:

- [MPI Forum](https://www.mpi-forum.org/)
- [openMPI](https://www.open-mpi.org/)
- [mpi4py](https://pythonhosted.org/mpi4py/)
- An Introduction to Parallel Programming, Peter S. Pacheco
- Using MPI, second ed., W. Gropp et al.
- Using MPI-2, W. Gropp et al.


#### MapReduce
![](http://www.glennklockwood.com/data-intensive/hadoop/mapreduce-workflow.png)

MapReduce is a concept introduced by Google.
It is originally designed to process Big Data, which is so large that could not be stored in a single machine.

Available packages/modules:

1. Disco

For Hadoop, Cloudera provides an easy-to-instal distribution, the Cloudera Distribution of Hadoop (CDH).


#### [ipyparallel](http://ipyparallel.readthedocs.io/en/latest/)

Although it is possible to have ~100-1000 core in the available computer cluster, it could be harder to own/manage an even larger cluster/cloud (I think to most normal scientists, particularly astronomers, only ~100 cores are available, through workstation or mini-cluster of computers). So MapReduce, which is designed to process Big Data, is not relevant to small-scale scientific problems, in addition to the difficulties to implement communications between the mappers in MapReduce system. But ipyparallel, the IPython parallel module provides scientists a good tool for research, especially it is an interactive tool. It is good with some experience with ipyparallel (formerly named IPython.parallel, but isolated now).

The ipyparallel implements a cluster in server-client mode. The way to start is to type `ipcluster start -n 24` in terminal. The point here is that the ipyparallel supports SMP/MPI/ssh/PBS, and also the EC2 cluster.

```python
from ipyparallel import Client
rc = Client(profile='default')
dv = rc[:]
```

### Other ways to do parallel

One of the other ways to do parallel is to use **GPU**.
This is migrated to another post.
