# Integration

How to use and integrate Rmagine into your own project.

## CPU (Embree)

### In Code

```cpp
#include <rmagine/map/EmbreeMap.hpp>
#include <rmagine/simulation/SphereSimulatorEmbree.hpp>

namespace rm = rmagine;

int main(int argc, char** argv)
{
    std::string filename = "path/to/mesh/file";
    rm::EmbreeMapPtr map = rm::import_embree_map(filename);

    rm::SphereSimulatorEmbree sim;
    sim.setMap(map);

    // go on (see workflow section)
    return 0;
}
```

### CMake

Add to your CMakeLists.txt:

```cmake
add_compile_options(-std=c++17)
set(CMAKE_CXX_STANDARD 17)

# find components of a specific rmagine version
# '2.2.8' will get the newest rmagine which 
# is greater equal 2.2.8
find_package(rmagine 2.2.8
  COMPONENTS
    core 
    embree
)

add_executable(my_rmagine_app 
    src/my_rmagine_app.cpp)

# link against rmagine targets
target_link_libraries(my_rmagine_app
    rmagine::core
    rmagine::embree
)
```

## GPU (OptiX)

### In Code

```cpp
#include <rmagine/map/OptixMap.hpp>
#include <rmagine/simulation/SphereSimulatorOptix.hpp>

namespace rm = rmagine;

int main(int argc, char** argv)
{
    std::string filename = "path/to/mesh/file";
    rm::OptixMapPtr map = rm::import_optix_map(filename);

    rm::SphereSimulatorOptix sim;
    sim.setMap(map);

    // go on (see workflow section)
    return 0;
}

```

### CMake

Add to your CMakeFile:

```cmake
add_compile_options(-std=c++17)
set(CMAKE_CXX_STANDARD 17)

# find components of a specific rmagine version
# '2.2.8' will get the newest rmagine which 
# is greater equal 2.2.8
find_package(rmagine 2.2.8 
  COMPONENTS
    core 
    cuda
    optix
)

add_executable(my_rmagine_app 
    src/my_rmagine_app.cpp)

# link against rmagine targets
target_link_libraries(my_rmagine_app
    rmagine::core
    rmagine::cuda
    rmagine::optix
)
```

For more details and alternate ways of integrating Rmagine into your CMake project we refer to: [CMake - Advanced](/extra/cmake_advanced.md).