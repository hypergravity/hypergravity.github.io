---
layout: post
title:  "GPU Computing"
date:   Tuesday, 30. August 2016 11:53AM
categories: HPC, Python, MATLAB, GPU
---

To obtain the same amount of FLOPS, GPU is a cheaper way.
But the issue is, for scientific computing, programmers have to deal with something purely about hardware, which should not happen.

## Available tools

- CUDA (only for NVIDIA devices)
- OpenCL
- OpenACC

Recently CUDA 8.0 is released, but CUDA 7.5 seems to be more supported by other softwares. But CUDA can only be used on NVIDIA devices.

Taking CUDA as an example, installation is described in [here](http://developer.download.nvidia.com/compute/cuda/7.5/Prod/docs/sidebar/CUDA_Installation_Guide_Linux.pdf).

## Python

### wrappers

- PyCUDA (actually not that easy to install, conf. [this](https://wiki.tiker.net/PyCuda/Installation/Linux/Ubuntu))
- PyOpenCL

In Python, PyCUDA and PyOpenCL provide a wrapper of CUDA and OpenCL, respectively.
You have to split the data stream so that the countless cores in GPU can process them separately.

### numba acceleration
Also, the previously mentioned `numba.jit` can still be used to accelerate the GPU computing programs.

## MATLAB

In MATLAB, 2014a/2016a all support GPU programming using `gpuArray`.
The rest seems to be easy and automatic.
Here is an example:

![](https://raw.githubusercontent.com/hypergravity/hypergravity.github.io/master/_posts/2016-08-30/matlab2016a_gpuarray_matdot.png)
Due to the latency, GPU behaves worse than CPU.
But re-do the commands, GPU has a ~40X speed gain.
