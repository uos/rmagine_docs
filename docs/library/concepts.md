# Key Concepts

Rmagine aims to perform computations flexible on selectable computing devices (CPU, GPU, ...).
It also provides mechanisms to minimize graphical overheads and unnecessary copies between devices.
# Structure

Rmagine has the following top-level structure of directories:

- Math - `rmagine/math`
- Types - `rmagine/types`
- Utilility - `rmagine/util`
- Map - `rmagine/map`
- Simulation - `rmagine/simulation`
- Noise - `rmagine/noise`

## Math

Contains all math related types and functions. 
All math datatypes are completely CUDA compatible.
See `rmagine/math/types.h` for all types.
See [Math](/library/math) section for more details.

## Types

Fundamental types required for simulations, e.g. [sensor models](/getting_started/sensors). Depends on math types.

## Util

Utility functions. For exaple, including `rmagine/util/prints.h` lets you print every math types via std::cout.

```cpp
#include <iostream>
#include <rmagine/math/types.h>
#include <rmagine/util/prints.h>

namespace rm = rmagine;

int main(int argc, char* argv)
{
    rm::Transform T = rm::Transform::Identity();
    std::cout << "my transformation: " << T << std::endl;
    return 0;
}
```

The Stopwatch in `rmagine/util/StopWatch.hpp` lets you easily stop the runtime of a Code section.

```cpp
#include <iostream>
#include <rmagine/util/StopWatch.hpp>

namespace rm = rmagine;

void demanding_function()
{
    // ...
}

int main(int argc, char* argv)
{
    rm::StopWatch sw;
    double el;

    sw();
    demanding_function();
    el = sw();
    std::cout << "Demanding Function took " << el << " s" << std::endl;

    return 0;
}

```

# Map

All classes and functions that relate to map construction and map modifications. See [Getting Started - Maps](/getting_started/maps) for a introduction to map loading. Go to [Library - Maps](/library/maps) for a more detailed descriptions of how to build Rmagine maps.

# Simulation

All classes and functions that relate to the actual simulations.


# Noise
All classes and functions that relate to postprocessed noising. 
