# tegra210-cuda

CUDA libraries and header files for Tegra210 platform

nVidia does not provide a convinient way to download and install aarch64 CUDA libraries for the purpose of cross compiling CUDA enabled applications and libraries. This project packages the libraries into a more cross-compile friendly format.

JetPack 4.3 contents:
- CUDA version 10.0
- libcudnn version 8.0.0.145

JetPack 4.4 contents:
- CUDA version 10.2
- libcudnn version 8.0.0.145



## How to extract CUDA libraries from JetPack

These were the steps taken to extract and repackage the CUDA libraries.

### Download JetPack 4.4 libraries.

We have 3 options here:
1. Download from the direct links below:
   - https://developer.nvidia.com/embedded/secure/tools/files/jetpack-sdks/jetpack-4.4-dp/JETPACK_44_b144/cuda-repo-cross-aarch64-10-2-local-10.2.89_1.0-1_all.deb
   - https://developer.nvidia.com/embedded/secure/tools/files/jetpack-sdks/jetpack-4.4-dp/JETPACK_44_b144/ubuntu1804/cuda-repo-ubuntu1804-10-2-local-10.2.89-440.40_1.0-1_amd64.deb
   - https://developer.nvidia.com/embedded/secure/tools/files/jetpack-sdks/jetpack-4.4-dp/JETPACK_44_b144/cuda-repo-l4t-10-2-local-10.2.89_1.0-1_arm64.deb
   - https://developer.nvidia.com/embedded/secure/tools/files/jetpack-sdks/jetpack-4.4-dp/JETPACK_44_b144/libcudnn8_8.0.0.145-1+cuda10.2_arm64.deb
   - https://developer.nvidia.com/embedded/secure/tools/files/jetpack-sdks/jetpack-4.4-dp/JETPACK_44_b144/libcudnn8-dev_8.0.0.145-1+cuda10.2_arm64.deb
2. Download SDK Manager from https://developer.nvidia.com/nvidia-sdk-manager
   - Install the SDK Manager. It is usually installed here: `/opt/nvidia/sdkmanager/sdkmanager-gui`
   - Use it to download JetPack 4.4 for Jetson Nano. At the time of this writing, it has a bug that prevents it from installing the packages automatically.

3. Use SDK Manager to generate `sdkml3_jetpack_l4t_44_dp.json` file, then download straight from the URLs in the file.
   - https://developer.nvidia.com/embedded/secure/tools/files/jetpack-sdks/jetpack-4.4-dp/JETPACK_44_b144/ubuntu1804/cuda-repo-ubuntu1804-10-2-local-10.2.89-440.40_1.0-1_amd64.deb
   - https://developer.nvidia.com/embedded/secure/tools/files/jetpack-sdks/jetpack-4.4-dp/JETPACK_44_b144/cuda-repo-cross-aarch64-10-2-local-10.2.89_1.0-1_all.deb
   - https://developer.nvidia.com/embedded/secure/tools/files/jetpack-sdks/jetpack-4.4-dp/JETPACK_44_b144/cuda-repo-l4t-10-2-local-10.2.89_1.0-1_arm64.deb
   - https://developer.nvidia.com/embedded/secure/tools/files/jetpack-sdks/jetpack-4.4-dp/JETPACK_44_b144/libcudnn8_8.0.0.145-1+cuda10.2_arm64.deb
   - https://developer.nvidia.com/embedded/secure/tools/files/jetpack-sdks/jetpack-4.4-dp/JETPACK_44_b144/libcudnn8-dev_8.0.0.145-1+cuda10.2_arm64.deb

### Install the downloaded packages

1. Manually install the downloaded packages. The downloaded packages are usually found in `~/Downloads/nvidia/sdkm_downloads` directory.
2. On x86_64 host, install `cuda-repo-cross-aarch64-10-2-local-10.2.89_1.0-1_all.deb` and `cuda-repo-ubuntu1804-10-2-local-10.2.89-440.40_1.0-1_amd64.deb`
   - `sudo dpkg --force-all -i cuda-repo-cross-aarch64-10-2-local-10.2.89_1.0-1_all.deb`
   - `sudo dpkg --force-all -i cuda-repo-ubuntu1804-10-2-local-10.2.89-440.40_1.0-1_amd64.deb`
   - `sudo apt-key add /var/cuda-repo-10-2-local-10.2*/*.pub'`
   - `sudo apt-get -y update`
   - `sudo apt install -y gnupg libgomp1 libfreeimage-dev libopenmpi-dev openmpi-bin`
   - `sudo apt-get -y --allow-downgrades install cuda-toolkit-10-2 cuda-cross-aarch64-10-2`
   - Development files will be installed in `/usr/local/cuda-10.2` directory.
3. On a Jetson Nano running Ubuntu, install `cuda-repo-l4t-10-2-local-10.2.89_1.0-1_arm64.deb`
   - `sudo dpkg --force-all -i cuda-repo-l4t-10-2-local-10.2.89_1.0-1_arm64.deb`
   - `sudo apt-key add /var/cuda-repo-10-2-local-10.2*/*.pub'`
   - `sudo apt-get -y update`
   - `sudo apt install -y gnupg libgomp1 libfreeimage-dev libopenmpi-dev openmpi-bin`
   - `sudo apt-get -y --allow-downgrades install cuda-toolkit-10-2 cuda-cross-aarch64-10-2`
   - Development files will be installed in `/usr/local/cuda-10.2` directory.
4. On a Jetson Nano running Ubuntu, extract `libcudnn8_8.0.0.145-1+cuda10.2_arm64.deb` and `libcudnn8-dev_8.0.0.145-1+cuda10.2_arm64.deb`
   - `mkdir -p ~/libcudnn-8.0.0.145/dev`
   - `mkdir -p ~/libcudnn-8.0.0.145/rt`
   - `mkdir -p /tmp/libcudnn-8.0.0.145/dev`
   - `mkdir -p /tmp/libcudnn-8.0.0.145/rt`
   - `cd /tmp/libcudnn-8.0.0.145/rt`
   - `ar x libcudnn8_8.0.0.145-1+cuda10.2_arm64.deb && tar xf data.tar.xz`
   - `mv usr ~/libcudnn-8.0.0.145/rt/`
   - `cd /tmp/libcudnn-8.0.0.145/dev`
   - `ar x libcudnn8-dev_8.0.0.145-1+cuda10.2_arm64.deb && tar xf data.tar.xz`
   - `mv usr ~/libcudnn-8.0.0.145/dev/`
5. Remove docs and samples from `/usr/local/cuda-10.2` and `~/libcudnn-8.0.0.145/dev/`.

### Compress the libraries

1. On a Jetson Nano running Ubuntu, compress CUDA core libraries
   - `mkdir -p ~/cuda-10.2-tegra210/usr/local`
   - `cp -arf /usr/local/cuda-10.2 ~/cuda-10.2-tegra210/usr/local`
   - `cd ~`
   - `XZ_OPT=-9 tar cfJ cuda-10.2-tegra210.tar.xz cuda-10.2-tegra210`
2. On a Jetson Nano running Ubuntu, compress CUDA DNN library
   - `cd ~`
   - `XZ_OPT=-9 tar cfJ libcudnn-8.0.0.145-tegra210.tar.xz libcudnn-8.0.0.145`

### Alternatively, run the repack script

1. Place all deb files in the `dl` directory.
2. Run `./repack_deb_lib.sh`.



### Files too big, split and reassemble

To split, run: `split -b 1800M -a 1 --numeric-suffixes=0 cuda-10.2-ubuntu1804.tar.xz cuda-10.2-ubuntu1804.tar.xz.part`

To reassemble, run: `cat cuda-10.2-ubuntu1804.tar.xz.part? > cuda-10.2-ubuntu1804.tar.xz`
