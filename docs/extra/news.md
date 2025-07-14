# News

## 12.09.2024

Alongside the new version 2.2.6 we released a minimal viewer that demonstrates the sensor models of rmagine. Check it out here: https://github.com/amock/rmagine_viewer.

## 05.12.2023

New version 2.2.2 is available now and brings convenience updates for ROS-users. Just place Rmagine into your ROS-workspace and it will compile. Via `find_package(rmagine COMPONENTS [...])` you can still find Rmagine's components as if you would install it globally on your system. We tested it with 
- ROS1 - noetic
- ROS2 - humble

Normally you would set `OptiX_INCLUDE_DIR` via cmake flags. Now we provide an additional option: Set the environment variable `OPTIX_HEADER_DIR` for example in your `.bashrc`-file:

```bash
export OPTIX_INCLUDE_DIR=~/software/optix/NVIDIA-OptiX-SDK-7.4.0-linux64-x86_64/include
```

Especially if you place Rmagine into your ROS-workspace this option becomes very handy.

## 27.09.2023

From version >= 2.2.0 we enabled component-wise compilation and packaging for easier installation of Rmagine. In "Releases" section you can find the first pre-compiled binaries. Install the core library via

```console
sudo dpkg -i rmagine-core_2.2.1_amd64.deb
```

Then additionally for the Embree backend:

```console
sudo dpkg -i rmagine-embree_2.2.1_amd64.deb
```

And if you have a NVIDIA GPU:

```console
sudo dpkg -i rmagine-cuda_2.2.1_amd64.deb
sudo dpkg -i rmagine-optix_2.2.1_amd64.deb
```

Using the pre-compiled binaries, you are not required to download the OptiX-headers anymore. However, CUDA and Embree are still required to be installed on your system.
