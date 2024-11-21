# Supported Sensor Models

Rmagine supports several configurations of commonly used range sensors as Spherical, Pinhole or even fully customizable O1Dn and OnDn models.
The following image shows how the results could look like using these different models.

![sensor_models_3d](/resources/img/sensor_models_3d.png) 
![sensor_models_ortho](/resources/img/sensor_models_ortho.png)

The next instructions show how to initialize each model individually.

## Spherical

Spherical model for LiDARs.

```c++
struct SphericalModel
{
    DiscreteInterval phi;
    DiscreteInterval theta;
    Interval range;
};
```

Example:

```c++
#include <rmagine/types/sensor_models.h>
namespace rm = rmagine;

// ...

rm::SphericalModel model;

model.theta.min = -M_PI;
model.theta.inc = 0.4 * M_PI / 180.0;
model.theta.size = 900;

model.phi.min = -15.0 * M_PI / 180.0;
model.phi.inc = 2.0 * M_PI / 180.0;
model.phi.size = 16;

model.range.min = 0.0;
model.range.max = 100.0;
```

## Pinhole

Pinhole model for depth cameras.

```c++
struct PinholeModel
{
    uint32_t width;
    uint32_t height;

    Interval range;

    float f[2]; // focal lengths fx, fy
    float c[2]; // centroid cx, cy
};
```

Example:

```c++
#include <rmagine/types/sensor_models.h>
namespace rm = rmagine;

...

rm::PinholeModel model;
model.width = 200;
model.height = 150;
model.c[0] = 100.0;
model.c[1] = 75.0;
model.f[0] = 100.0;
model.f[1] = 100.0;
model.range.min = 0.0;
model.range.max = 100.0;
```

## O1Dn

Fully customizable model with one origin and N directions.

```c++
struct O1DnModel
{
    uint32_t width;
    uint32_t height;

    // maximum and minimum allowed range
    Interval range;

    // i-th ray = orig, dirs[i]
    Vector orig; // One origin
    Memory<Vector> dirs; // N directions
};
```

Example:

```c++
#include <rmagine/types/sensor_models.h>
namespace rm = rmagine;

...

rm::O1DnModel model;

model.orig.x = 0.0;
model.orig.y = 0.0;
model.orig.z = 0.0;

model.width = 200;
model.height = 1;

model.dirs.resize(model.width * model.height);

float step_size = 0.05;

for(int i=0; i<200; i++)
{
    float y = - static_cast<float>(i - 100) * step_size;
    float x = cos(y) * 2.0 + 2.0;
    float z = -1.0;

    model.dirs[i].x = x;
    model.dirs[i].y = y;
    model.dirs[i].z = z;

    model.dirs[i].normalize();
}

model.range.min = 0.0;
model.range.max = 100.0;
```

## OnDn

Fully customizable model with N origins and N directions.

```c++
struct OnDnModel
{
    uint32_t width;
    uint32_t height;

    Interval range;

    // i-th ray = origs[i], dirs[i]
    Memory<Vector> origs; // N origins
    Memory<Vector> dirs; // N directions
};
```

Example:

```c++
#include <rmagine/types/sensor_models.h>
namespace rm = rmagine;

...

rm::OnDnModel model;

model.width = 200;
model.height = 1;

model.dirs.resize(model.width * model.height);
model.origs.resize(model.width * model.height);

float step_size = 0.05;

for(int i=0; i<200; i++)
{
    float percent = static_cast<float>(i) / static_cast<float>(200);
    float step = - static_cast<float>(i - 100) * step_size;
    float y = sin(step);
    float x = cos(step);

    model.origs[i].x = 0.0;
    model.origs[i].y = y * percent;
    model.origs[i].z = x * percent;

    model.dirs[i].x = 1.0;
    model.dirs[i].y = 0.0;
    model.dirs[i].z = 0.0;
}

model.range.min = 0.0;
model.range.max = 100.0;
```

## Predefined models

Rmagine also provides some example models for testing. These are located in the `rmagine/types/sensors.h` Header-file and will be expanded in the future to include additional range sensors.

```c++
#include <rmagine/types/sensors.h>
namespace rm = rmagine;


...

// Velodyne VLP-16 with different horizontal resoultions
rm::SphericalModel  velo_model_1 = rm::vlp16_900();
rm::SphericalModel  velo_model_2 = rm::vlp16_360();


// another examples for testing:
rm::SphericalModel  ex_lidar    = rm::example_spherical();
rm::PinholeModel    ex_pinhole  = rm::example_pinhole();
rm::O1DnModel       ex_o1dn     = rm::example_o1dn();
rm::OnDnModel       ex_ondn     = rm::example_ondn();
```