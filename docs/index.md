
![rmagine_teaser_image](resources/img/sensor_models_3d.png)
# Rmagine

Rmagine allows a robot to simulate sensor data for arbitrary range sensors directly on board via raytracing. Since robots typically only have limited computational resources, Rmagine aims at being flexible and lightweight, while scaling well even to large environment maps. It runs on several platforms like Laptops or embedded computing boards like Nvidia Jetson by putting an unified API over specific proprietary libraries provided by the hardware manufacturers. This work is designed to support the future development of robotic applications depending on simulation of range data that could previously not be computed in reasonable time on mobile systems.

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
- [News](extra/news)
- [Embree 3](extra/embree3)
- [Contributions](extra/contributions)

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
