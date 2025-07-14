# Installation (From Source)

The following instructions are tested on an Ubuntu 20.04 operating system.

## Dependencies

### Assimp (Open Assets Importer Library)

For loading commonly used mesh/scene formats.

```bash
user@pc:~$ sudo apt install libassimp-dev
```

## Backbones

![rmagine_backends](/resources/img/rmagine_backends.png)

Rmagine provides an interface to integrate ray tracing libraries, we call backbones. All of these backbones are optional. So far we integrated Intel Embree and NVIDIA OptiX.

### Embree Backbone (optional)

We support Embree in its latest version (tested: v4.0.1, v4.2.0):

```bash
user@pc:~$ git clone https://github.com/embree/embree.git
user@pc:~$ mkdir embree/build && cd embree/build
user@pc:~/embree/build$ cmake -DCMAKE_BUILD_TYPE=Release ..
user@pc:~/embree/build$ make -j`nproc`
user@pc:~/embree/build$ sudo make install
```

For older Embree versions we refer to [this](/extra/embree3.md).

### OptiX Backbone (optional)

Rmagine supports NVIDIA OptiX versions of 7.2 or newer (experimental support for OptiX 8+).
The OptiX-Library is installed via the GPU driver.
The OptiX-Headers can be downloaded from [GitHub](https://github.com/NVIDIA/optix-dev).
The Headers require a specific GPU driver and CUDA version to be installed on your system:

| OptiX Version | Minimum Driver Version |
|---------------|------------------------|
|     7.2       |  456.71                |
|     7.3       |  465.84                |
|     7.4       |  495.89                |
|     7.5       |  495.89 (untested)     |
|     7.6       |  520.00 (untested)     |
|     7.7       |  530.41                |

!!! note 

    Check NVIDIA docs for newer versions.

## Compilation

Download the Rmagine repository. 

```bash
user@pc:~/rmagine$ mkdir build
user@pc:~/rmagine$ cd build
user@pc:~/rmagine/build$ cmake ..
user@pc:~/rmagine/build$ make
```

The path to OptiX-Headers should be specified with the CMake-Variable `OptiX_INCLUDE_DIR`. This can be done using `ccmake`, for example.

### Bash Variable (Alternative)

Alternatively, cmake checks for the environment variable `OPTIX_INCLUDE_DIR` to exist. After downloading the OptiX-SDK you can add the following command to your `.bashrc`:

```bash
export OPTIX_INCLUDE_DIR=~/.../NVIDIA-OptiX-SDK-7.4.0-linux64-x86_64/include
```

After adding this path, the project should compile without changing the cmake flags.

### Optional: Check Compilation 

You can check if everything went wrong by running the benchmark that was build besides the library:

```bash
user@pc:~/rmagine/build$ ./bin/rmagine_benchmark_cpu ../dat/sphere.ply
...
[ 100% - velos/s: 6261.9, mean: 6244.84] 
Result: 6244.84 velos/s
```

or if the OptiX support was successfully build:

```bash
user@pc:~/rmagine/build$ ./bin/rmagine_benchmark_gpu ../dat/sphere.ply
...
[ 100% - velos/s: 383094, mean: 383457, rays/s: 5.52178e+09] 
Result: 383457 velos/s
```

## Installation

After compilation do

```bash
user@pc:~/rmagine/build$ sudo make install
```

### Optional: Check Installation

You can check if everything went wrong by running the benchmark that was build besides the library:

```bash
user@pc:~$ rmagine_benchmark_cpu rmagine/dat/sphere.ply
...
[ 100% - velos/s: 6261.9, mean: 6244.84] 
Result: 6244.84 velos/s
```

or if the OptiX support was successfully build:

```bash
user@pc:~$ rmagine_benchmark_gpu rmagine/dat/sphere.ply
...
[ 100% - velos/s: 383094, mean: 383457, rays/s: 5.52178e+09] 
Result: 383457 velos/s
```

## Uninstall Rmagine

```bash
user@pc:~/rmagine/build$ sudo make uninstall
```

# Installation (Debian Package) - Experimental

We are working on creating debian packages for easier installations.

## Dependencies

```console
sudo apt install libassimp-dev libeigen3-dev
```

## Install
Download latest Rmagine debian packages from Github releases page (v2.3.0). Install the core by calling

```console
sudo apt install ./rmagine-core_2.3.0_amd64.deb
```

### Embree Backbone  

We support Embree in its latest version (tested: v4.0.1 - v4.3.0). Make sure you have Embree installed on your system.

```console
sudo apt install ./rmagine-embree_2.3.0_amd64.deb
```

### OptiX Backbone

Make sure you have a current NVIDIA driver installed, then install rmagine-cuda and rmagine-optix by:

```console
sudo apt install ./rmagine-cuda_2.3.0_amd64.deb
sudo apt install ./rmagine-optix_2.3.0_amd64.deb
```

## Uninstall

To uninstall everything related to rmagine, call:

```console
sudo apt-get remove rmagine-core
```

