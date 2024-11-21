
Follow the following steps if you want Rmagine to work with Embree 3:

```bash
user@pc:~$ git clone https://github.com/embree/embree.git --branch v3.13.5
user@pc:~$ mkdir embree/build && cd embree/build
user@pc:~/embree/build$ cmake -DCMAKE_BUILD_TYPE=Release ..
user@pc:~/embree/build$ make -j`nproc`
user@pc:~/embree/build$ sudo make install
```

During the `cmake`-process errors could occur. The following flags could fix it. You can edit them by calling `ccmake .` in build directory.

```bash
# The Intel Implicit SPMD Program Compiler is not necessarily needed 
EMBREE_ISPC_SUPPORT=OFF
# Tasking system: TBB/Internal. You can choose INTERNAL if TBB is not installed
EMBREE_TASKING_SYSTEM=INTERNAL 
# Tutials are not needed for library compilation
EMBREE_TUTORIALS=OFF
```

Or you can pass the arguments directly to cmake for the minimal build
```bash
cmake -DCMAKE_BUILD_TYPE=Release -DEMBREE_ISPC_SUPPORT=OFF -DEMBREE_TASKING_SYSTEM=INTERNAL -DEMBREE_TUTORIALS=OFF ..
```
