# Maps

Triangle meshes can be stored in various file formats. Rmagine utilizes the [Open Asset Import Library (assimp)](https://github.com/assimp/assimp) in order to support a wide range of well known file formats. After loading the raw scene graph buffers with Assimp, they are converted into Rmagines internal scene graph structure. Dependent on the computation backend `Embree` or `OptiX` the scene graph is prepared for fast ray traversals by building the required acceleration structures.

## Embree Map

```c++
#include <rmagine/map/EmbreeMap.hpp>

namespace rm = rmagine;

int main(int argc, char** argv)
{
    std::string filename = "path/to/mesh/file";
    rm::EmbreeMapPtr map = rm::import_embree_map(filename);
    return 0;
}
```

## OptiX Map

```c++
#include <rmagine/map/OptixMap.hpp>

namespace rm = rmagine;

int main(int argc, char** argv)
{
    std::string filename = "path/to/mesh/file";
    rm::OptixMapPtr map = rm::import_optix_map(filename);
    return 0;
}
```

## Properties

After loading, the map consists of a complete scene graph. It then usually is passed to a simulator (see next "Getting Started"-sections).
The advanced [Map](/library/maps.md)-section describes how to modify or create the internal maps from scratch.