# Data

For development and testing we include some meshes inside our repository in the `dat`-folder: 


## sphere.ply

![sphere.ply](/resources/img/rmagine_dat_sphere.png)

```bash
user@pc:~/rmagine/build$ ./bin/rmagine_map_info ../dat/sphere.ply
Rmagine Map Info
Inputs: 
- filename: ../dat/sphere.ply
Meshes: 1
  Mesh 0
    - name: 
    - vertices, faces: 642, 1280
    - primitives: TRIANGLE
    - normals: no
    - vertex color channels: 0
    - uv channels: 0
    - bones: 0
    - material index: 0
    - tangents and bitangents: no
    - aabb: AABB [v[0,0,0] - v[0,0,0]]
Textures: 0
Scene Graph: 
- name: 
- transform: 
  M4x4[
    1 0 0 0
    0 1 0 0
    0 0 1 0
    0 0 0 1
  ]
- meshes: 1
  - mesh ref 0 -> 0
- children: 0
```

## triangle.ply

![triangle.ply](/resources/img/rmagine_dat_triangle.png)

```bash
user@pc:~/rmagine/build$ ./bin/rmagine_map_info ../dat/triangle.ply
Rmagine Map Info
Inputs: 
- filename: ../dat/triangle.ply
Meshes: 1
  Mesh 0
    - name: 
    - vertices, faces: 3, 1
    - primitives: TRIANGLE
    - normals: no
    - vertex color channels: 0
    - uv channels: 0
    - bones: 0
    - material index: 0
    - tangents and bitangents: no
    - aabb: AABB [v[0,0,0] - v[0,0,0]]
Textures: 0
Scene Graph: 
- name: 
- transform: 
  M4x4[
    1 0 0 0
    0 1 0 0
    0 0 1 0
    0 0 0 1
  ]
- meshes: 1
  - mesh ref 0 -> 0
- children: 0
```

## box_rot_trans_scaled.dae

![box_rot_trans_scaled.dae](/resources/img/rmagine_dat_box_rot_trans_scaled.png)

```bash
user@pc:~/rmagine/build$ ./bin/rmagine_map_info ../dat/box_rot_trans_scaled.dae
Rmagine Map Info
Inputs: 
- filename: ../dat/box_rot_trans_scaled.dae
Meshes: 1
  Mesh 0
    - name: Cube-mesh
    - vertices, faces: 36, 12
    - primitives: TRIANGLE
    - normals: yes
    - vertex color channels: 0
    - uv channels: 1
    - bones: 0
    - material index: 0
    - tangents and bitangents: no
    - aabb: AABB [v[0,0,0] - v[0,0,0]]
Textures: 0
Scene Graph: 
- name: Scene
- transform: 
  M4x4[
    1 0 0 0
    0 1 0 0
    0 0 1 0
    0 0 0 1
  ]
- meshes: 0
- children: 3
  Node 0
    - name: Camera
    - transform: 
      M4x4[
        0.727676 0.305421 -0.61417 -6.92579
        -0.685921 0.324014 -0.651558 -7.35889
        0 0.895396 0.445271 4.95831
        0 0 0 1
      ]
    - meshes: 0
    - children: 0
  Node 1
    - name: Light
    - transform: 
      M4x4[
        0.955171 -0.199883 0.218391 1.00545
        0.290865 0.771101 -0.566393 -4.07624
        -0.0551891 0.604525 0.794672 5.90386
        0 0 0 1
      ]
    - meshes: 0
    - children: 0
  Node 2
    - name: Cube
    - transform: 
      M4x4[
        0.141421 0.141421 0 0
        -0.141421 0.141421 0 -5
        0 0 0.2 0
        0 0 0 1
      ]
    - meshes: 1
      - mesh ref 0 -> 0
    - children: 0
```

## two_cubes.dae

![two_cubes.dae](/resources/img/rmagine_dat_two_cubes.png)

```bash
user@pc:~/rmagine/build$ ./bin/rmagine_map_info ../dat/two_cubes.dae
Rmagine Map Info
Inputs: 
- filename: ../dat/two_cubes.dae
Meshes: 2
  Mesh 0
    - name: Cube_001-mesh
    - vertices, faces: 36, 12
    - primitives: TRIANGLE
    - normals: yes
    - vertex color channels: 0
    - uv channels: 1
    - bones: 0
    - material index: 1
    - tangents and bitangents: no
    - aabb: AABB [v[0,0,0] - v[0,0,0]]
  Mesh 1
    - name: Cube-mesh
    - vertices, faces: 36, 12
    - primitives: TRIANGLE
    - normals: yes
    - vertex color channels: 0
    - uv channels: 1
    - bones: 0
    - material index: 0
    - tangents and bitangents: no
    - aabb: AABB [v[0,0,0] - v[0,0,0]]
Textures: 0
Scene Graph: 
- name: Scene
- transform: 
  M4x4[
    1 0 0 0
    0 1 0 0
    0 0 1 0
    0 0 0 1
  ]
- meshes: 0
- children: 4
  Node 0
    - name: Cube_001
    - transform: 
      M4x4[
        1 0 0 5.01877
        0 1 0 3.78582
        0 0 1 1.01026
        0 0 0 1
      ]
    - meshes: 1
      - mesh ref 0 -> 0
    - children: 0
  Node 1
    - name: Camera
    - transform: 
      M4x4[
        0.685921 -0.324014 0.651558 7.35889
        0.727676 0.305421 -0.61417 -6.92579
        0 0.895396 0.445271 4.95831
        0 0 0 1
      ]
    - meshes: 0
    - children: 0
  Node 2
    - name: Light
    - transform: 
      M4x4[
        -0.290865 -0.771101 0.566393 4.07624
        0.955171 -0.199883 0.218391 1.00545
        -0.0551891 0.604525 0.794672 5.90386
        0 0 0 1
      ]
    - meshes: 0
    - children: 0
  Node 3
    - name: Cube
    - transform: 
      M4x4[
        1.64731 1.34142 -0.299005 5.7392
        -1.28721 1.34237 -1.06938 -4.66034
        -0.481559 1.00054 1.83561 1.69496
        0 0 0 1
      ]
    - meshes: 1
      - mesh ref 0 -> 1
    - children: 0
```


## many_objects.dae

![many_objects.dae](/resources/img/rmagine_dat_many_objects.png)

```bash
user@pc:~/rmagine/build$ ./bin/rmagine_map_info ../dat/many_objects.dae
Rmagine Map Info
Inputs: 
- filename: ../dat/many_objects.dae
Meshes: 7
  Mesh 0
    - name: Torus-mesh
    - vertices, faces: 3456, 1152
    - primitives: TRIANGLE
    - normals: yes
    - vertex color channels: 0
    - uv channels: 1
    - bones: 0
    - material index: 0
    - tangents and bitangents: no
    - aabb: AABB [v[0,0,0] - v[0,0,0]]
  Mesh 1
    - name: Suzanne-mesh
    - vertices, faces: 2901, 967
    - primitives: TRIANGLE
    - normals: yes
    - vertex color channels: 0
    - uv channels: 1
    - bones: 0
    - material index: 0
    - tangents and bitangents: no
    - aabb: AABB [v[0,0,0] - v[0,0,0]]
  Mesh 2
    - name: Cone-mesh
    - vertices, faces: 186, 62
    - primitives: TRIANGLE
    - normals: yes
    - vertex color channels: 0
    - uv channels: 1
    - bones: 0
    - material index: 0
    - tangents and bitangents: no
    - aabb: AABB [v[0,0,0] - v[0,0,0]]
  Mesh 3
    - name: Cylinder-mesh
    - vertices, faces: 372, 124
    - primitives: TRIANGLE
    - normals: yes
    - vertex color channels: 0
    - uv channels: 1
    - bones: 0
    - material index: 0
    - tangents and bitangents: no
    - aabb: AABB [v[0,0,0] - v[0,0,0]]
  Mesh 4
    - name: Plane-mesh
    - vertices, faces: 6, 2
    - primitives: TRIANGLE
    - normals: yes
    - vertex color channels: 0
    - uv channels: 1
    - bones: 0
    - material index: 0
    - tangents and bitangents: no
    - aabb: AABB [v[0,0,0] - v[0,0,0]]
  Mesh 5
    - name: Cube_001-mesh
    - vertices, faces: 36, 12
    - primitives: TRIANGLE
    - normals: yes
    - vertex color channels: 0
    - uv channels: 1
    - bones: 0
    - material index: 1
    - tangents and bitangents: no
    - aabb: AABB [v[0,0,0] - v[0,0,0]]
  Mesh 6
    - name: Cube-mesh
    - vertices, faces: 36, 12
    - primitives: TRIANGLE
    - normals: yes
    - vertex color channels: 0
    - uv channels: 1
    - bones: 0
    - material index: 0
    - tangents and bitangents: no
    - aabb: AABB [v[0,0,0] - v[0,0,0]]
Textures: 0
Scene Graph: 
- name: Scene
- transform: 
  M4x4[
    1 0 0 0
    0 1 0 0
    0 0 1 0
    0 0 0 1
  ]
- meshes: 0
- children: 10
  Node 0
    - name: Torus
    - transform: 
      M4x4[
        -0.0934659 -0.290695 2.78847 -9.244
        0.942502 2.62438 0.30518 5.28275
        -2.64041 0.94707 0.0102276 3.4012
        0 0 0 1
      ]
    - meshes: 1
      - mesh ref 0 -> 0
    - children: 0
  Node 1
    - name: Suzanne
    - transform: 
      M4x4[
        -0.247416 -0.65867 0.71059 -6.41007
        0.442643 -0.729225 -0.521823 -2.72992
        0.861889 0.18543 0.471977 1.71339
        0 0 0 1
      ]
    - meshes: 1
      - mesh ref 0 -> 1
    - children: 0
  Node 2
    - name: Cone
    - transform: 
      M4x4[
        1 0 0 1.73173
        0 1 0 -7.66226
        0 0 1 0.999599
        0 0 0 1
      ]
    - meshes: 1
      - mesh ref 0 -> 2
    - children: 0
  Node 3
    - name: Cylinder
    - transform: 
      M4x4[
        1 0 0 1.34234
        0 1 0 8.77874
        0 0 1 0.959374
        0 0 0 1
      ]
    - meshes: 1
      - mesh ref 0 -> 3
    - children: 0
  Node 4
    - name: Plane
    - transform: 
      M4x4[
        11 0 0 0
        0 11 0 0
        0 0 11 0
        0 0 0 1
      ]
    - meshes: 1
      - mesh ref 0 -> 4
    - children: 0
  Node 5
    - name: Light_001
    - transform: 
      M4x4[
        -0.290865 -0.771101 0.566393 4.07624
        0.955171 -0.199883 0.218391 1.00545
        -0.0551891 0.604525 0.794672 5.90386
        0 0 0 1
      ]
    - meshes: 0
    - children: 0
  Node 6
    - name: Cube_001
    - transform: 
      M4x4[
        0.84132 0.52049 0.145848 5.72826
        -0.452094 0.52966 0.717685 3.24672
        0.296298 -0.669739 0.680923 3.1261
        0 0 0 1
      ]
    - meshes: 1
      - mesh ref 0 -> 5
    - children: 0
  Node 7
    - name: Camera
    - transform: 
      M4x4[
        0.685921 -0.324014 0.651558 7.35889
        0.727676 0.305421 -0.61417 -6.92579
        0 0.895396 0.445271 4.95831
        0 0 0 1
      ]
    - meshes: 0
    - children: 0
  Node 8
    - name: Light
    - transform: 
      M4x4[
        -0.290865 -0.771101 0.566393 4.07624
        0.955171 -0.199883 0.218391 1.00545
        -0.0551891 0.604525 0.794672 5.90386
        0 0 0 1
      ]
    - meshes: 0
    - children: 0
  Node 9
    - name: Cube
    - transform: 
      M4x4[
        1 0 0 4.91178
        0 1 0 -2.96374
        0 0 1 1.06244
        0 0 0 1
      ]
    - meshes: 1
      - mesh ref 0 -> 6
    - children: 0
```
