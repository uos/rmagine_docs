# CMake - Advanced

## CMake - Standard

The standard way of including Rmagine is by first installing it and then importing it to another project via CMake's `find_package` command as follows:

```cmake
add_compile_options(-std=c++17)
set(CMAKE_CXX_STANDARD 17)

# find components of a specific rmagine version
# '2.2.1...' will get the newest rmagine which 
# is greater than 2.2.1
find_package(rmagine 2.2.1... 
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

add_executable(my_rmagine_app src/my_rmagine_app.cpp)
target_link_libraries(my_rmagine_app
    rmagine::core
    rmagine::embree)
```