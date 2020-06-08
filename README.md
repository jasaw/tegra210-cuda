# tegra210-cuda

CUDA libraries and header files for Tegra210 platform

nVidia does not provide a convinient way to download and install aarch64 CUDA libraries for the purpose of cross compiling CUDA enabled applications and libraries. This project packages the libraries into a more cross-compile friendly format.

JetPack 4.3 contents:
- CUDA version 10.0
- libcudnn version 7.6.3.28

JetPack 4.4 contents:
- CUDA version 10.2



## How to extract CUDA libraries from JetPack

These were the steps taken to extract and repackage the CUDA libraries.

### Download JetPack 4.3 libraries.

We have 3 options here:
1. Download from the direct links below:
   - https://developer.nvidia.com/assets/embedded/secure/tools/files/jetpack-sdks/jetpack-4.3/JETPACK_43_b132/cuda-repo-cross-aarch64-10-0-local-10.0.326_1.0-1_all.deb
   - https://developer.nvidia.com/assets/embedded/secure/tools/files/jetpack-sdks/jetpack-4.3/JETPACK_43_b132/cuda-repo-ubuntu1804-10-0-local-10.0.326-410.108_1.0-1_amd64.deb
   - https://developer.nvidia.com/assets/embedded/secure/tools/files/jetpack-sdks/jetpack-4.3/JETPACK_43_b132/cuda-repo-l4t-10-0-local-10.0.326_1.0-1_arm64.deb
   - https://developer.nvidia.com/assets/embedded/secure/tools/files/jetpack-sdks/jetpack-4.3/JETPACK_43_b132/libcudnn7_7.6.3.28-1+cuda10.0_arm64.deb
   - https://developer.nvidia.com/assets/embedded/secure/tools/files/jetpack-sdks/jetpack-4.3/JETPACK_43_b132/libcudnn7-dev_7.6.3.28-1+cuda10.0_arm64.deb
2. Download SDK Manager from https://developer.nvidia.com/nvidia-sdk-manager
   - Install the SDK Manager. It is usually installed here: `/opt/nvidia/sdkmanager/sdkmanager-gui`
   - Use it to download JetPack 4.3 for Jetson Nano. At the time of this writing, it has a bug that prevents it from installing the packages automatically.

3. Use SDK Manager to generate `sdkml3_jetpack_l4t_43_ga.json` file, then download straight from the URLs in the file.
   - https://developer.nvidia.com/assets/embedded/secure/tools/files/jetpack-sdks/jetpack-4.3/JETPACK_43_b132/ubuntu1804/cuda-repo-ubuntu1804-10-0-local-10.0.326-410.108_1.0-1_amd64.deb
   - https://developer.nvidia.com/assets/embedded/secure/tools/files/jetpack-sdks/jetpack-4.3/JETPACK_43_b132/cuda-repo-cross-aarch64-10-0-local-10.0.326_1.0-1_all.deb
   - https://developer.nvidia.com/assets/embedded/secure/tools/files/jetpack-sdks/jetpack-4.3/JETPACK_43_b132/cuda-repo-l4t-10-0-local-10.0.326_1.0-1_arm64.deb
   - https://developer.nvidia.com/assets/embedded/secure/tools/files/jetpack-sdks/jetpack-4.3/JETPACK_43_b132/libcudnn7_7.6.3.28-1+cuda10.0_arm64.deb
   - https://developer.nvidia.com/assets/embedded/secure/tools/files/jetpack-sdks/jetpack-4.3/JETPACK_43_b132/libcudnn7-dev_7.6.3.28-1+cuda10.0_arm64.deb

### Install the downloaded packages

1. Manually install the downloaded packages. The downloaded packages are usually found in `~/Downloads/nvidia/sdkm_downloads` directory.
2. On x86_64 host, install `cuda-repo-cross-aarch64-10-0-local-10.0.326_1.0-1_all.deb` and `cuda-repo-ubuntu1804-10-0-local-10.0.326-410.108_1.0-1_amd64.deb`
   - `sudo dpkg --force-all -i cuda-repo-cross-aarch64-10-0-local-10.0.326_1.0-1_all.deb`
   - `sudo dpkg --force-all -i cuda-repo-ubuntu1804-10-0-local-10.0.326-410.108_1.0-1_amd64.deb`
   - `sudo apt-key add /var/cuda-repo-10-0-local-10.0*/*.pub'`
   - `sudo apt-get -y update`
   - `sudo apt install -y gnupg libgomp1 libfreeimage-dev libopenmpi-dev openmpi-bin`
   - `sudo apt-get -y --allow-downgrades install cuda-toolkit-10-0 cuda-cross-aarch64-10-0`
   - Development files will be installed in `/usr/local/cuda-10.0` directory.
3. On a Jetson Nano running Ubuntu, install `cuda-repo-l4t-10-0-local-10.0.326_1.0-1_arm64.deb`
   - `sudo dpkg --force-all -i cuda-repo-l4t-10-0-local-10.0.326_1.0-1_arm64.deb`
   - `sudo apt-key add /var/cuda-repo-10-0-local-10.0*/*.pub'`
   - `sudo apt-get -y update`
   - `sudo apt install -y gnupg libgomp1 libfreeimage-dev libopenmpi-dev openmpi-bin`
   - `sudo apt-get -y --allow-downgrades install cuda-toolkit-10-0 cuda-cross-aarch64-10-0`
   - Development files will be installed in `/usr/local/cuda-10.0` directory.
4. On a Jetson Nano running Ubuntu, extract `libcudnn7_7.6.3.28-1+cuda10.0_arm64.deb` and `libcudnn7-dev_7.6.3.28-1+cuda10.0_arm64.deb`
   - `mkdir -p ~/libcudnn-7.6.3.28/dev`
   - `mkdir -p ~/libcudnn-7.6.3.28/rt`
   - `mkdir -p /tmp/libcudnn-7.6.3.28/dev`
   - `mkdir -p /tmp/libcudnn-7.6.3.28/rt`
   - `cd /tmp/libcudnn-7.6.3.28/rt`
   - `ar x libcudnn7_7.6.3.28-1+cuda10.0_arm64.deb && tar xf data.tar.xz`
   - `mv usr ~/libcudnn-7.6.3.28/rt/`
   - `cd /tmp/libcudnn-7.6.3.28/dev`
   - `ar x libcudnn7-dev_7.6.3.28-1+cuda10.0_arm64.deb && tar xf data.tar.xz`
   - `mv usr ~/libcudnn-7.6.3.28/dev/`
5. Remove docs and samples from `/usr/local/cuda-10.0` and `~/libcudnn-7.6.3.28/dev/`.

### Compress the libraries

1. On a Jetson Nano running Ubuntu, compress CUDA core libraries
   - `mkdir -p ~/cuda-10.0-tegra210/usr/local`
   - `cp -arf /usr/local/cuda-10.0 ~/cuda-10.0-tegra210/usr/local`
   - `cd ~`
   - `XZ_OPT=-9 tar cfJ cuda-10.0-tegra210.tar.xz cuda-10.0-tegra210`
2. On a Jetson Nano running Ubuntu, compress CUDA DNN library
   - `cd ~`
   - `XZ_OPT=-9 tar cfJ libcudnn-7.6.3.28-tegra210.tar.xz libcudnn-7.6.3.28`

### Alternatively, run the repack script

1. Place all deb files in the `dl` directory.
2. Run `./repack_deb_lib.sh`.
