# Math

The following descriptions are made reading the Code located in `rmagine/math/types.h`. So it is recommended to open the file alongside.

## Points and Translations

A floating coordinate can represent different things such as a point, a vector or the translational part of a transformation.
For all of them, we provide the same data structure: `Vector`.

- `Vector2`: x,y all fp32
- `Vector3`: x,y,z all fp32

Aliases:
- `Vector` = `Vector3`;
- `Point` = `Vector3`;
- `Vertex` = `Vector3`;

We also implemented commonly used functions `Vector`. Example Usage:

```cpp
#include <rmagine/math/types.h>
namespace rm = rmagine;

// ...

// initializations
rm::Vector3 p1;
p1.x = 0.0;
p1.y = 1.0;
p1.z = 2.0;
rm::Vector3 p2 = {1.0, 2.0, 3.0};

// functions
float p1_length = p1.l2norm();

// operators
rm::Vector3 p3;
p3 = p1 + p2;
p3 = p1 - p2;
p3 = p1 * 2.0;
p3 = p1 / 2.0;

// ...
```


## Rotations

In Rmagine we provide three different representations of rotations: Euler Angles, a Rotation Matrix and a Quaternion.
In general, we adhere to the ROS conventions, especially those that are listed in [REP-103](https://www.ros.org/reps/rep-0103.html).


```cpp
#include <rmagine/math/types.h>
namespace rm = rmagine;

int main(int argc, char** argv)
{
    // EulerAngles
    rm::EulerAngles e1;
    e1.roll = 0.0;
    e1.pitch = 0.0;
    e1.yaw = M_PI / 2.0;
    rm::EulerAngles e2 = {0.0, 0.0, M_PI / 2.0};
    rm::EulerAngles eI = rm::EulerAngles::Identity();
    
    // Quaternion
    rm::Quaternion q1;
    q1.x = 0.0;
    q1.y = 0.0;
    q1.z = 0.7071068;
    q1.w = 0.7071068;
    rm::Quaternion q2 = {0.0, 0.0, 0.7071068, 0.7071068};
    rm::Quaternion qI = rm::Quaternion::Identity();

    // Matrix3x3
    // - Storage Order: Column-Major
    // - Access via '()'-operator: Row-Major
    // - Access via '[]'-operator: Column-Major
    rm::Matrix3x3 M1;
    M1(0,0) =  0.0; M1(0,1) = -1.0; M1(0,2) =  0.0;
    M1(1,0) =  1.0; M1(1,1) =  0.0; M1(1,2) =  0.0;
    M1(2,0) =  0.0; M1(2,1) =  0.0; M1(2,2) =  1.0;
    rm::Matrix3x3 M2 = {{
        {0.0, 1.0, 0.0},
        {-1.0, 0.0, 0.0},
        {0.0, 0.0, 1.0}
    }};
    rm::Matrix3x3 MI = rm::Matrix3x3::Identity();

    return 0;
}
```

Conversions

```cpp
#include <rmagine/math/types.h>
namespace rm = rmagine;

int main(int argc, char** argv)
{
    // initializations
    rm::EulerAngles e;
    rm::Matrix3x3 M;
    rm::Quaterion q; 

    rm::Vector3 p = {1.0, 0.0, 0.0};

    // rotation 90 degree around z-axis counter-clockwise
    e.roll = 0.0;
    e.pitch = 0.0;
    e.yaw = M_PI / 2.0;

    // convert
    M.set(e); // set M values that express the same rotation as e
    q.set(e); // set q values that express the same rotation as e

    rm::Vector3 p1 = e * p;
    rm::Vector3 p2 = M * p;
    rm::Vector3 p3 = q * p;
    // p1, p2 and p3 should all contain the values {0.0, 1.0, 0.0}

    // other conversions:
    q.set(M);
    M.set(q);
    e.set(M);
    e.set(q);

    return 0;
}
```

Math

```cpp
#include <rmagine/math/types.h>
namespace rm = rmagine;
using namespace rmagine;

int main(int argc, char** argv)
{
    rm::EulerAngles e = {0.0, 0.0, M_PI/2.0};
    rm::Quaternion q; q.set(e);
    rm::Vector3 p = {1.0, 0.0, 0.0};

    // Rotate a point 90 degrees around the z axis counter-clockwise
    rm::Vector3 p1 = q * p;

    // Chaining Rotations
    // -> Rotate a point 90 degrees around the z axis clockwise
    rm::Quaternion q2 = q * q * q;
    rm::Vector3 p2 = q2 * p;

    // invert
    rm::Quaternion q2_inv = q2.inv();
    // or if 'using namespace rmagine;' was set, ~operator can be used
    q2_inv = ~q2;

    return 0;
}
```


Eigen Compatibility


```cpp
#include <rmagine/math/types.h>
#include <Eigen/Dense>
namespace rm = rmagine;

int main(int argc, char** argv)
{
    // Rmagine -> Eigen
    {
        // generate rmagine type
        rm::Matrix3x3    M_rm = rm::Matrix3x3::Identity();

        // rmagine's Matrix3x3 has the same Column-Major storage order as the Eigen default for Eigen::Matrix3f
        Eigen::Matrix3f& M_eig = *reinterpret_cast<Eigen::Matrix3f*>(&M_rm);

        // another way is to use Eigens functions for mapping raw buffers
        Eigen::Map<Eigen::Matrix3f> M_eig2(&M_rm(0,0));

        // changing an entry in M_rm should now change the same entry in M_eig and M_eig2 as well
    }

    // Eigen -> Rmagine
    {
        Eigen::Matrix3f M_eig = Eigen::Matrix3f::Identity();
        rm::Matrix3x3& M_rm = *reinterpret_cast<rm::Matrix3x3*>(&M_eig);
    }

    return 0;
}
```


## Transformations
In Rmagine, a Transformation is an operation that maps a source Euclidean space to a target Euclidean space, implemented as Isometry, since we only implemented the rotational and translational part (no scale). We decided to represent the rotational part as Quaternion to avoid Gimbal Locks that occur e.g. using a Euler Angles representation.

In Rmagine, we use a Transformation-Type for a pose as well.
A sensor pose entries correspond to a transformation that maps the space with the sensor as origin to the space where the pose is located:

```cpp
#include <rmagine/math/types.h>
namespace rm = rmagine;

int main(int argc, char** argv)
{
    // pose of the sensor in the map
    rm::Vector3 s_t = {0.0, 1.0, 2.0};
    rm::Quaternion s_R = rm::Quaternion::Identity();

    // has the same entries as the transform form sensor -> map
    rm::Transfrom T_sensor_to_map;
    T_sensor_to_map.R = s_R;
    T_sensor_to_map.t = s_t;

    return 0;
}
```

To express a Transformation that is not isometric, for example because it consists of a Scale part, use `rm::Matrix4x4` and the Linear Algebra Functions of `rmagine/math/linalg.h`instead:

```cpp
#include <rmagine/math/types.h>
namespace rm = rmagine;
using namespace rmagine;

int main(int argc, char** argv)
{
    // pose of the sensor in the map
    Transform T = {
        rm::Quaternion::Identity(), // rotation
        {0.0, 1.0, 2.0} // translation
    };
    rm::Vector3 s = {1.2, 1.2, 1.2}; // scale each dimension with 1.2


    // pack to Matrix4x4
    rm::Matrix4x4 M = rm::compose(T, s);

    // Use M
    rm::Vector3 p_sensor = {2.0, 1.0, 0.0};
    rm::Vector3 p_map = M * p_sensor;
    
    // unpack inverse: operator~ works only because of 'using namespace rmagine;'
    // - Alternative: Matrix4x4::inv() 
    rm::decompose(~M, T, s);

    return 0;
}
```