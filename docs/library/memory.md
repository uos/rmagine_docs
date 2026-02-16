## Memory

Rmagine internally uses so-called Memory objects to manage memory located on different hardware.
The location where the actual memory should be allocated can be passed as a template argument using one of the following keywords:

* `RAM` (RAM memory)
* `RAM_CUDA` (pinned CUDA host memory)
* `VRAM_CUDA` (CUDA device memory)
* (... and more)

After allocating the memory, accessing elements is similar to using `std::vector`'s:

* access element with `[]`
* resize the memory with `resize()`
* access raw data pointer with `raw()` function

The following code samples are describing how to work with Memory objects and how to transfer Memory to other hardware.


Example CPU-only:

```cpp
#include <rmagine/types/Memory.hpp>

namespace rm = rmagine;

int main(int argc, char** argv)
{
    // allocate memory with 1000 elements
    rm::Memory<float, rm::RAM> mem(1000);

    // shrink memory
    mem.resize(100);

    // set some values
    mem[0] = 10.0;
    mem[10] = 5.0;

    return 0;
}
```

Example GPU-only:


```cpp
#include <rmagine/types/MemoryCuda.hpp>

namespace rm = rmagine;

__global__
void set_value(float* data, unsigned int id, float val)
{
    data[id] = val;
}

int main(int argc, char** argv)
{
    // allocate memory with 1000 elements on GPU
    rm::Memory<float, rm::VRAM_CUDA> mem(1000);

    // shrink memory on GPU
    mem.resize(100);

    // this would cause a segfault. since the underlying memory is not available
    // on the device this code is executed:
    // mem[0] = 10.0;
    //
    // set some values in Cuda kernels instead
    set_value<<<1,1>>>(mem.raw(), 0, 10.0);
    set_value<<<1,1>>>(mem.raw(), 10, 5.0);

    return 0;
}
```

Example CPU <-> CPU:

```cpp
#include <rmagine/types/Memory.hpp>
#include <rmagine/types/MemoryCuda.hpp>

namespace rm = rmagine;

__global__ my_kernel(float* data, unsigned int N)
{
    // ...
}

int main(int argc, char** argv)
{
    // allocate memory with 1000 float elements
    rm::Memory<float, rm::RAM> mem(1000);
    
    // set some values
    mem[0] = 10.0;
    mem[10] = 5.0;

    // copy the whole memory to GPU
    rm::Memory<float, rm::VRAM_CUDA> mem_ = mem;

    // run some kernels
    my_kernel<<<mem_.size(), 1>>>(mem_.raw(), mem_.size());

    // copy back
    mem = mem_;

    return 0;
}
```


## Writing Memory dependent Functions

With the memory objects Rmagine offers at the same time the possibility to make function calls dependent on the location of the memory.
The next example adds two vectors and creates a new one.
The actual addition should be executed on the device where the memory is currently stored on.

```cpp
rm::Memory<float, rm::RAM> vec1(1000);
rm::Memory<float, rm::RAM> vec2(1000);

// fill vec1, vec2 ...

// copy to GPU
rm::Memory<float, rm::VRAM_CUDA> vec1_ = vec1;
rm::Memory<float, rm::VRAM_CUDA> vec2_ = vec2;

// we want to achieve this:
auto vec3 = add(vec1, vec2);
auto vec3_ = add(vec1_, vec2_);

// ...
```

So we try to create a function `add(a, b)` whose code will be executed on the CPU once `a` and `b` are in RAM. However, as soon as `a` and `b` are stored on the GPU the code should be executed on the GPU as well as the function returns a GPU memory object.
This can be done as follows:

### Signatures (Header):

```cpp
rm::Memory<float, rm::RAM> add(
    const rm::Memory<float, rm::RAM>& a, 
    const rm::Memory<float, rm::RAM>& b);

rm::Memory<float, rm::VRAM_CUDA> add(
    const rm::Memory<float, rm::VRAM_CUDA>& a, 
    const rm::Memory<float, rm::VRAM_CUDA>& b);    
```

### Code CPU

```cpp
rm::Memory<float, rm::RAM> add(
    const rm::Memory<float, rm::RAM>& a, 
    const rm::Memory<float, rm::RAM>& b)
{
    rm::Memory<float, rm::RAM> c(a.size());
    for(size_t i=0; i < a.size(); i++)
    {
        c[i] = a[i] + b[i];
    }
    return c;
}
```

### Code GPU

```cpp
__global__
void add_kernel(const float* a, const float* b, float* c, unsigned int N)
{
    const unsigned int id = blockIdx.x * blockDim.x + threadIdx.x;
    if(id < N)
    {
        c[id] = a[id] + b[id];
    }
}

rm::Memory<float, rm::VRAM_CUDA> add(
    const rm::Memory<float, rm::VRAM_CUDA>& a, 
    const rm::Memory<float, rm::VRAM_CUDA>& b)
{
    rm::Memory<float, rm::VRAM_CUDA> c(a.size());
    add_kernel<<<c.size(), 1>>>(a.raw(), b.raw(), c.raw(), c.size());
    return c;
}
```

Having these functions defined allows us to very flexible chain operations:

```cpp
// Simple (as above):
auto vec3 = add(vec1, vec2);
auto vec3_ = add(vec1_, vec2_);

// Advanced
// - add two vectors on CPU and upload to GPU on return
rm::Memory<float, rm::VRAM_CUDA> vec3_ = add(vec1, vec2);
// - add two vectors on GPU and download to CPU on return
rm::Memory<float, rm::RAM> vec3 = add(vec1_, vec2_);
```

## Slicing and MemoryViews

Rmagine also provides mechanisms to slice these Memory objects and handling shallow copies through so-called Memory Views.

```cpp
rm::Memory<float, RAM> mem(1000);
// MemoryView to the elements [100: 200]
rm::MemoryView<float, RAM> slice = mem(100, 200);
// this sets slice[0] and mem[100] to 10
slice[0] = 10.0;
```

With that it is possible to access existing memory very flexible:

```cpp
rm::Memory<int, rm::RAM> mem(1000);
rm::Memory<int, rm::VRAM_CUDA> mem_(1000);

// copy [100:200] to [0:100]
mem(0, 100) = mem(100, 200)
mem_(0, 100) = mem_(100, 200)

// or even transfer memory slice-wise
mem_(0, 100) = mem(500, 600); // upload a slice
mem(400, 500) = mem_(100, 200); // download a slice
```

### Application Example 1

for debuging purposes sometimes it is required to print a fetch a single element out of a GPU buffer. Here we just want to print the first element of a GPU memory object as follows:

1. copy one element to CPU through
2. print

```cpp
rm::Memory<int, rm::VRAM_CUDA> mem_(1000);

// download [0:1] to CPU
rm::Memory<int, rm::RAM> one_elem_mem = mem_(0,1);
std::cout << one_elem_mem[0] << std::endl;
```

### Application Example 2

Oftentimes the GPU has a very limited amount of Memory.
In Code this can be overcome using Rmagine's slices as follows:

```cpp
// max available CPU mem: 1000
rm::Memory<int, RAM> mem(1000);
// max available GPU mem: 10
rm::Memory<int, VRAM_CUDA> mem_(10);

// i = [0, 10, 20, 30, ..., 990]
for(size_t i=0; i<mem.size(); i += mem_.size())
{
    mem_ = mem(i, i + mem_.size());
    // process algorithm on GPU
}
```

## Cuda Isolated Library Creation

In order to ship a library and its headers, each CUDA piece of code should be invisable after compilation. That means the shipped header should be free of code that can be only proccessed by the NVCC compiler.
Exeptions for that are, if the library is clearly marked as to use with CUDA.
The following example shows how to achieve that using `Memory` objects and the function `add` from above.
In this example the `add`-function is further improved to handle slices.

### Signatures (Header):

File: `add.h`
```cpp
rm::Memory<float, rm::RAM> add(
    const rm::MemoryView<float, rm::RAM>& a, 
    const rm::MemoryView<float, rm::RAM>& b);

rm::Memory<float, rm::VRAM_CUDA> add(
    const rm::MemoryView<float, rm::VRAM_CUDA>& a, 
    const rm::MemoryView<float, rm::VRAM_CUDA>& b);
```

File `add.cuh`
```cpp
rm::Memory<float, rm::VRAM_CUDA> add(
    const rm::MemoryView<float, rm::VRAM_CUDA>& a, 
    const rm::MemoryView<float, rm::VRAM_CUDA>& b);
```

### Code

File `add.cpp`

```cpp
#include "add.h"

rm::Memory<float, rm::RAM> add(
    const rm::MemoryView<float, rm::RAM>& a, 
    const rm::MemoryView<float, rm::RAM>& b)
{
    rm::Memory<float, rm::RAM> c(a.size());
    for(size_t i=0; i < a.size(); i++)
    {
        c[i] = a[i] + b[i];
    }
    return c;
}
```

File: `add.cu`

```cpp
#include "add.cuh"

__global__
void add_kernel(const float* a, const float* b, float* c, unsigned int N)
{
    const unsigned int id = blockIdx.x * blockDim.x + threadIdx.x;
    if(id < N)
    {
        c[id] = a[id] + b[id];
    }
}

rm::Memory<float, rm::VRAM_CUDA> add(
    const rm::MemoryView<float, rm::VRAM_CUDA>& a, 
    const rm::MemoryView<float, rm::VRAM_CUDA>& b)
{
    rm::Memory<float, rm::VRAM_CUDA> c(a.size());
    add_kernel<<<c.size(), 1>>>(a.raw(), b.raw(), c.raw(), c.size());
    return c;
}
```



### Main and CMake

File: `Main.cpp`


```cpp
#include <rmagine/types/Memory.hpp>
#include "add.h"
#include <rmagine/types/MemoryCuda.hpp>
#include "add.cuh"

int main(int argc, char** argv)
{
    // CPU
    rm::Memory<float, rm::RAM> vec1(100);
    rm::Memory<float, rm::RAM> vec2(100);
    auto vec3 = add(vec1, vec2);
    auto vec3_slice = add(vec1(0, 10), vec2(10, 20));

    // GPU
    rm::Memory<float, rm::RAM> vec1_(100);
    rm::Memory<float, rm::RAM> vec2_(100);
    auto vec3_ = add(vec1_, vec2_);
    auto vec3_slice_ = add(vec1_(0, 10), vec2_(10, 20));

    return 0;
}
```

File: `CMakeLists.txt`

```cmake
# ...

add_library(my_add add.cpp)
cuda_add_library(my_add_cuda add.cu)

add_executable(Main Main.cpp)
target_link_libraries(Main
    my_add
    my_add_cuda
)

# ...
```

The Main.cpp and potential other code thus can be compiled with another compiler than the NVCC host compiler even though the CUDA code is executed internally.