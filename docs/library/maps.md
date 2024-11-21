## Rmagine Scene Graph

Rmagine provides datatypes for scene graphs.
They are seperated into Embree and OptiX scene graphs to store a Scene Graph either on CPU or on GPU.
Both of these Scene Graphs have a very similar interface but are slightly different.
Thus are real shared interface is not implemented yet.
In Rmagine, a scene graph holds Geometries, Instances and Scenes.

### Geometry

A geometry is an abstract description of a physical object.
Implemented Geometries are:

| Concept | Embree       | OptiX     |
|---------|--------------|-----------|
| Mesh    | EmbreeMesh   | OptixMesh |
| Points  | EmbreePoints | -         |

We also implemented some shortcut meshes that are used in later examples.
They are located in the files `rmagine/map/embree/embree_shapes.h` and `rmagine/map/optix/optix_shapes.h`.

| ShortCut Mesh | Embree           | OptiX           |
|---------------|------------------|-----------------|
| Sphere        | `EmbreeSphere`   | `OptixSphere`   |
| Cube          | `EmbreeCube`     | `OptixCube`     |
| Plane         | `EmbreePlane`    | `OptixPlane`    |
| Cylinder      | `EmbreeCylinder` | `OptixCylinder` |

### Scene

A scene is a set of geometries or a set of instances, each of which is assigned a position, an orientation, and a scale.

### Instance

An instance instantiates a given scene or geometry at a certain pose.
Thus things can be instantiated without duplicating their memory.
A classic example for that is 3D asteroids where the same asteroid geometry has to be spawned several times.
Using different geometries would then bring the GPU memory to its limits.
Instead, Instantiating one geometry several times leads to a more memory friendly way of solving this problem.
In robotics one can think of a known geometry as a class, e.g. a chair.
This chair can be placed several times in the map by instantiating it.

## Scene Graph Embree

### Simple

```cpp
#include <rmagine/map/embree/EmbreeScene.hpp>
// shortcut meshes
#include <rmagine/map/embree/embree_shapes.h>

namespace rm = rmagine;

int main(int argc, char** argv)
{
    // create a scene    
    rm::EmbreeScenePtr scene = std::make_shared<rm::EmbreeScene>();

    // create a sphere
    rm::EmbreeMeshPtr sphere = std::make_shared<rm::EmbreeSphere>(1.0);
    { // sphere settings
        Transform T = Transform::Identity();
        T.t.x = 1.0; // move sphere one unit along the x-axis 
        sphere->setTransform(T);
        sphere->apply(); // apply transform related changes
    }
    sphere->commit(); // commit sphere

    scene->add(sphere); // add sphere to scene
    scene->commit(); // commit scene

    // add the scene to a map. 
    rm::EmbreeMapPtr map = std::make_shared<rm::EmbreeMap>(scene);

    // The map can then be used for simulations ...
    return 0;
}
```

### Instances

```cpp
#include <rmagine/map/embree/EmbreeScene.hpp>
#include <rmagine/map/embree/EmbreeInstance.hpp>
// shortcut meshes
#include <rmagine/map/embree/embree_shapes.h>

namespace rm = rmagine;

/**
 * Create a Scene consisting of a sphere, a cube, and a cylinder
 */
rm::EmbreeScenePtr create_scene()
{
    rm::EmbreeScenePtr scene = std::make_shared<rm::EmbreeScene>();

    rm::EmbreeMeshPtr sphere = std::make_shared<rm::EmbreeSphere>(1.0);
    { // sphere settings
        Transform T = Transform::Identity();
        T.t.y = -2.0; // move sphere two units right 
        sphere->setTransform(T);
        sphere->apply(); // apply transform related changes
    }
    sphere->commit(); // commit sphere
    scene->add(sphere); // add sphere to scene

    rm::EmbreeMeshPtr cube = std::make_shared<rm::EmbreeCube>();
    cube->commit(); // commit cube
    scene->add(cube); // add cube to scene

    auto cylinder = std::make_shared<rm::EmbreeCylinder>();
    { // cylinder settings
        Transform T = Transform::Identity();
        T.t.y = 2.0; // move cylinder two units left 
        cylinder->setTransform(T);
        cylinder->apply(); // apply transform related changes
    }
    cylinder->commit(); // commit cylinder
    scene->add(cylinder); // add cylinder to scene

    scene->commit(); // commit scene
    return scene;
}

int main(int argc, char** argv)
{
    // create a scene    
    auto scene = std::make_shared<rm::EmbreeScene>();

    auto subscene = create_scene();

    // add 100 instances of a subscene to a scene
    for(size_t i=0; i<100; i++)
    {
        rm::EmbreeInstancePtr subscene_inst = subscene->instantiate();
        { // subscene_inst settings
            rm::Transform T = rm::Transform::Identity();
            T.t.x = (float)i; // moving instance 5 units to front
            subscene_inst->setTransform(T);
            subscene_inst->apply(); // apply transform related changes
        }
        subscene_inst->commit(); // commit instances
        scene->add(subscene_inst); // add instance to scene
    }

    scene->commit();

    // add the scene to a map. 
    rm::EmbreeMapPtr map = std::make_shared<rm::EmbreeMap>(scene);

    // The map can then be used for simulations ...
    return 0;
}
```

### Custom Meshes

```cpp
rm::EmbreeMeshPtr create_custom_mesh()
{
    size_t Nvertices = 3;
    size_t Nfaces = 1;
    auto mesh = std::make_shared<rm::EmbreeMesh>(Nvertices, Nfaces);

    // reference to data as MemoryView objects
    rm::MemoryView<rm::Vertex, rm::RAM> vertices = mesh->vertices();
    rm::MemoryView<rm::Face, rm::RAM>   faces    = mesh->faces();
    
    faces[0] = {0, 1, 2};
    vertices[0] = {1.0, 0.0, 0.0};
    vertices[1] = {0.0, 1.0, 0.0};
    vertices[2] = {0.0, 0.0, 0.0};

    return mesh;
}

int main(int argc, char** argv)
{
    auto scene = std::make_shared<rm::EmbreeScene>();
    
    auto my_mesh = create_custom_mesh();
    my_mesh->commit();

    scene->add(my_mesh);
    scene->commit();
    // do something with scene ...
    return 0;
}
```

## Scene Graph OptiX

### Simple


```cpp
#include <rmagine/map/optix/OptixScene.hpp>
// shortcut meshes
#include <rmagine/map/optix/optix_shapes.h>

namespace rm = rmagine;

int main(int argc, char** argv)
{
    // create a scene    
    rm::OptixScenePtr scene = std::make_shared<rm::OptixScene>();

    // create a sphere
    rm::OptixMeshPtr sphere = std::make_shared<rm::OptixSphere>(1.0);
    { // sphere settings
        Transform T = Transform::Identity();
        T.t.x = 1.0; // move sphere one unit along the x-axis 
        sphere->setTransform(T);
        sphere->apply(); // apply transform related changes
    }
    sphere->commit(); // commit sphere

    scene->add(sphere); // add sphere to scene
    scene->commit(); // commit scene

    // add the scene to a map. 
    rm::OptixMapPtr map = std::make_shared<rm::OptixMap>(scene);

    // The map can then be used for simulations ...
    return 0;
}
```

### Instances

```cpp
#include <rmagine/map/optix/OptixScene.hpp>
#include <rmagine/map/optix/OptixInstance.hpp>
// shortcut meshes
#include <rmagine/map/optix/optix_shapes.h>

namespace rm = rmagine;

/**
 * Create a Scene consisting of a sphere, a cube, and a cylinder
 */
rm::OptixScenePtr create_scene()
{
    rm::OptixScenePtr scene = std::make_shared<rm::OptixScene>();

    rm::OptixMeshPtr sphere = std::make_shared<rm::OptixSphere>(1.0);
    { // sphere settings
        Transform T = Transform::Identity();
        T.t.y = -2.0; // move sphere two units right 
        sphere->setTransform(T);
        sphere->apply(); // apply transform related changes
    }
    sphere->commit(); // commit sphere
    scene->add(sphere); // add sphere to scene

    rm::OptixMeshPtr cube = std::make_shared<rm::OptixCube>();
    cube->commit(); // commit cube
    scene->add(cube); // add cube to scene

    auto cylinder = std::make_shared<rm::OptixCylinder>();
    { // cylinder settings
        Transform T = Transform::Identity();
        T.t.y = 2.0; // move cylinder two units left 
        cylinder->setTransform(T);
        cylinder->apply(); // apply transform related changes
    }
    cylinder->commit(); // commit cylinder
    scene->add(cylinder); // add cylinder to scene

    scene->commit(); // commit scene
    return scene;
}

int main(int argc, char** argv)
{
    // create a scene    
    auto scene = std::make_shared<rm::OptixScene>();

    auto subscene = create_scene();

    // add 100 instances of a subscene to a scene
    for(size_t i=0; i<100; i++)
    {
        rm::OptixInstancePtr subscene_inst = subscene->instantiate();
        { // subscene_inst settings
            rm::Transform T = rm::Transform::Identity();
            T.t.x = (float)i; // moving instance 5 units to front
            subscene_inst->setTransform(T);
            subscene_inst->apply(); // apply transform related changes
        }
        subscene_inst->commit(); // commit instance
        scene->add(subscene_inst); // add instance to scene
    }

    scene->commit();

    // add the scene to a map. 
    rm::OptixMapPtr map = std::make_shared<rm::OptixMap>(scene);

    // The map can then be used for simulations ...
    return 0;
}
```