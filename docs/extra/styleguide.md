

## Naming Conventions

### Files
- Files containing a single C++-Class are written camelcase with `.hpp` extension. Examples: `MyClass.hpp` (Header), `MyClass.cpp` (Code), and `MyClass.tcc` (Template Code).
- Files holding a set of Structs, Classes, or Functions are written lowercase with `.h` extension. Examples: `math/types.h`, `conversions.h`, or `math.h` (Header) and `conversions.cpp` for code.
- Files holding Functions with CUDA code are namend with a `.cuh` extension: `math.cuh`

### Code
- Functions with underscores: `mult`, `my_function` 
- Classes and Structs in camelcase beginning with a capital letter: `Sphere`, `SphereSimulatorEmbree`
- Class Functions are beginning lowercase and then camelcase: `mult`, `simulateRanges`

TODO: revise files to meet this styleguide



## Special Operators for Math Types

- `~`: Invert the element after
