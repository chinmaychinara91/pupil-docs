+++
date = "2017-01-19T16:40:20+07:00"
title = "linux"
section_weight = 4
page_weight = 1.1
+++

## Linux Dependencies

These installation instructions are tested using **Ubuntu 16.04 or higher** running on many machines. Do not run Pupil on a VM unless you know what you are doing.

### Install Dependencies

Let's get started! Its time for `apt`!  Just copy paste into the terminal and listen to your machine purr.

```
sudo apt install -y pkg-config git cmake build-essential nasm wget python3-setuptools libusb-1.0-0-dev  python3-dev python3-pip python3-numpy python3-scipy libglew-dev libglfw3-dev libtbb-dev
```

> ffmpeg >= 3.2

```
sudo add-apt-repository ppa:jonathonf/ffmpeg-3
sudo apt-get update
sudo apt install libavformat-dev libavcodec-dev libavdevice-dev libavutil-dev libswscale-dev libavresample-dev ffmpeg libav-tools x264 x265
```

> OpenCV

```
# The requisites for opencv to build python3 cv2.so library are:
# (1) python3 interpreter found
# (2) libpython***.so shared lib found (make sure to install python3-dev)
# (3) numpy for python3 installed.
# If cv2.so was not build, delete the build folder, recheck the requisites and try again.

git clone https://github.com/itseez/opencv
cd opencv
mkdir build
cd build
cmake -D CMAKE_BUILD_TYPE=RELEASE -D BUILD_TBB=ON -D WITH_TBB=ON ..
make -j2
sudo make install
sudo ldconfig
```

> Turbojpeg

```
wget -O libjpeg-turbo.tar.gz https://sourceforge.net/projects/libjpeg-turbo/files/1.5.1/libjpeg-turbo-1.5.1.tar.gz/download
tar xvzf libjpeg-turbo.tar.gz
cd libjpeg-turbo-1.5.1
./configure --with-pic --prefix=/usr/local
sudo make install
sudo ldconfig
```

> libuvc

```
git clone https://github.com/pupil-labs/libuvc
cd libuvc
mkdir build
cd build
cmake ..
make && sudo make install
```

> udev rules for running libuvc as normal user

```
echo 'SUBSYSTEM=="usb",  ENV{DEVTYPE}=="usb_device", GROUP="plugdev", MODE="0664"' | sudo tee /etc/udev/rules.d/10-libuvc.rules > /dev/null 
sudo udevadm trigger
```

> Install packages with `pip`

```
sudo pip3 install numexpr
sudo pip3 install cython
sudo pip3 install psutil
sudo pip3 install pyzmq
sudo pip3 install msgpack_python
sudo pip3 install pyopengl
sudo pip3 install git+https://github.com/zeromq/pyre
sudo pip3 install git+https://github.com/pupil-labs/PyAV
sudo pip3 install git+https://github.com/pupil-labs/pyuvc
sudo pip3 install git+https://github.com/pupil-labs/pyndsi
sudo pip3 install git+https://github.com/pupil-labs/pyglui
```

> Finally, we install 3D eye model dependencies

```bash
sudo apt-get install libboost-dev
sudo apt-get install libboost-python-dev
sudo apt-get install libgoogle-glog-dev libatlas-base-dev libeigen3-dev
# sudo apt-get install software-properties-common if add-apt-repository is not found
sudo add-apt-repository ppa:bzindovic/suitesparse-bugfix-1319687
sudo apt-get update
sudo apt-get install libsuitesparse-dev
# install ceres-solver
git clone https://ceres-solver.googlesource.com/ceres-solver
cd ceres-solver
mkdir build && cd build
cmake .. -DBUILD_SHARED_LIBS=ON 
make -j3
make test
sudo make install
sudo sh -c 'echo "/usr/local/lib" > /etc/ld.so.conf.d/ceres.conf'
sudo ldconfig
```
