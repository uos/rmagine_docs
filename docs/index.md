
![rmagine_teaser_image](resources/img/sensor_models_3d.png)
# Rmagine

Rmagine allows a robot to simulate sensor data for arbitrary range sensors directly on board via raytracing. Since robots typically only have limited computational resources, Rmagine aims at being flexible and lightweight, while scaling well even to large environment maps. It runs on several platforms like Laptops or embedded computing boards like Nvidia Jetson by putting an unified API over specific proprietary libraries provided by the hardware manufacturers. This work is designed to support the future development of robotic applications depending on simulation of range data that could previously not be computed in reasonable time on mobile systems.

## Citation

We presented this work at ICRA'23 in London. The paper gives valuable insights of the design concepts of this library.
When using the Rmagine library or any related ideas in your scientific work, please reference the following paper:

```bib
@inproceedings{mock2023rmagine,
  title={{Rmagine: 3D Range Sensor Simulation in Polygonal Maps via Ray Tracing for Embedded Hardware on Mobile Robots}}, 
  author={Mock, Alexander and Wiemann, Thomas and Hertzberg, Joachim},
  booktitle={IEEE International Conference on Robotics and Automation (ICRA)}, 
  year={2023},
  doi={10.1109/ICRA48891.2023.10161388}
}
```

## Table of Contents

**Getting Started**

- [Overview](getting_started/overview)
- [Installation](getting_started/installation)
- [Integration](getting_started/integration)
- [Maps](getting_started/maps)
- [Sensors](getting_started/sensors)
- [Simulation](getting_started/simulation)
- [Problem Modelling](getting_started/problem_modelling)
- [Noise](getting_started/noise)

**Library**

- [Concepts](library/concepts)
- [Math](library/math)
- [Memory](library/memory)
- [Maps](library/maps)

**Extra**

- [Tools](extra/tools)
- [Data](extra/data)
- [Embree 3](extra/embree3)
- [CMake - Advanced](extra/cmake_advanced)
- [Contributions](extra/contributions)
- [News](extra/news)

## Contributions

You are welcome to contribute to the docs of [Rmagine](https://github.com/uos/rmagine)! Thorough and clear documentation is essential. You can help us by correcting mistakes, improving content, or adding examples that facilitate user navigation and usage of the project. Please submit any documentation-related issues to the repository https://github.com/uos/rmagine_docs. If you're making fixes or adding examples, don’t hesitate to submit a pull request afterward!

### PR workflow

How to contribute to this documentation via pull requests:

1. Fork the repository: https://github.com/uos/rmagine_docs.
2. Make changes on your forked repository.
3. Check locally on your machine if mkdocs is able to compile your changes ([instructions](https://github.com/uos/rmagine_docs)).
4. Go to Github and click "Pull Request", select this repository's "main" branch as target.
5. If you added new content, please provide a brief explanation of why you believe it is beneficial for the documentation.
6. Send PR

