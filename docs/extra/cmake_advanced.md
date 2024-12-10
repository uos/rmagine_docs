# CMake - Advanced

## CMake - Standard

The standard way of including Rmagine is by first installing it and then importing it to another project via CMake's `find_package` command as follows:

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

## CMake - Optional Components

```cmake
add_compile_options(-std=c++17)
set(CMAKE_CXX_STANDARD 17)

# find components of a specific rmagine version
# '2.2.8' will get the newest rmagine which 
# is greater equal 2.2.8
find_package(rmagine 2.2.8
    COMPONENTS
        core
    OPTIONAL_COMPONENTS
        embree
        cuda
        optix
)

if(TARGET rmagine::embree)
    # Compile app with embree support,
    # if rmagine::embree was found
    add_executable(my_rmagine_embree_app 
        src/my_rmagine_embree_app.cpp)
    target_link_libraries(my_rmagine_embree_app
        rmagine::core
        rmagine::embree
    )
endif(TARGET rmagine::embree)

if(TARGET rmagine::optix)
    # Compile app with optix support,
    # if rmagine::optix was found
    add_executable(my_rmagine_optix_app 
        src/my_rmagine_optix_app.cpp)
    target_link_libraries(my_rmagine_optix_app
        rmagine::core
        rmagine::cuda
        rmagine::optix
    )
endif(TARGET rmagine::optix)
```


## CMake - FetchContent

Rmagine is compatible with CMake's FetchContent functionality. The following `CMakeListst.txt` shows how to use `FetchContent` and checks if the required targets have been built successfully.

```cmake
cmake_minimum_required(VERSION 3.16)
project(my_rmagine_app)

add_compile_options(-std=c++17)
set(CMAKE_CXX_STANDARD 17)

include(FetchContent)
set(FETCHCONTENT_QUIET FALSE)
FetchContent_Declare(
  rmagine
  GIT_REPOSITORY https://github.com/uos/rmagine.git
  GIT_TAG        v2.2.9 # put 'main' here for latest
  GIT_PROGRESS   TRUE
)
FetchContent_MakeAvailable(rmagine)

if(NOT TARGET rmagine::embree)
    message(FATAL "Could not build rmagine's embree backand which is required for this executable")
endif(NOT TARGET rmagine::embree)

add_executable(my_rmagine_embree_app
    src/my_rmagine_embree_app.cpp)
target_link_libraries(my_rmagine_embree_app
    rmagine::core
    rmagine::embree)
```