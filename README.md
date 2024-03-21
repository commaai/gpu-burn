# gpu-burn
Multi-GPU CUDA stress test
(fork of [wilicc/gpu-burn](https://github.com/wilicc/gpu-burn))

# Modifications
* adds support for FP16 (with and without tensor cores)
* adds support for FP16 multiply FP32 accumulate (requires tensor cores)
* change batch size for optimal FLOP/s
* more accurately calculate FLOP/s
* report TFLOP/s instead of GFLOP/s

# TODO
* implement FP16 data comparison (check for errors)

# Building
To build GPU Burn:

`make`

To remove artifacts built by GPU Burn:

`make clean`

GPU Burn builds with a default Compute Capability of 5.0.
To override this with a different value:

`make COMPUTE=<compute capability value>`

CFLAGS can be added when invoking make to add to the default
list of compiler flags:

`make CFLAGS=-Wall`

LDFLAGS can be added when invoking make to add to the default
list of linker flags:

`make LDFLAGS=-lmylib`

NVCCFLAGS can be added when invoking make to add to the default
list of nvcc flags:

`make NVCCFLAGS=-ccbin <path to host compiler>`

CUDAPATH can be added to point to a non standard install or
specific version of the cuda toolkit (default is 
/usr/local/cuda):

`make CUDAPATH=/usr/local/cuda-<version>`

CCPATH can be specified to point to a specific gcc (default is
/usr/bin):

`make CCPATH=/usr/local/bin`

CUDA_VERSION and IMAGE_DISTRO can be used to override the base
images used when building the Docker `image` target, while IMAGE_NAME
can be set to change the resulting image tag:

`make IMAGE_NAME=myregistry.private.com/gpu-burn CUDA_VERSION=12.0.1 IMAGE_DISTRO=ubuntu22.04 image`

# Usage

    GPU Burn
    Usage: gpu_burn [OPTIONS] [TIME]
    
    -m X      Use X MB of memory
    -m N%     Use N% of the available GPU memory
    -fp64     Use double precision for multiply and accumulate
    -fp32     Use single precision for multiply and accumulate
    -fp16     Use half precision for multiply and accumulate
    -fp16fast Use half precision for multiply and single precision for accumulate
    -tc       Use Tensor cores (if available)
    -l        List all GPUs in the system
    -i N      Execute only on GPU N
    -h        Show this help message
    
    Example:
    gpu_burn -fp16fast -d 3600
