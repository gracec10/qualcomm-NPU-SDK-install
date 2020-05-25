# qualcomm-NPU-SDK-install
How to ACTUALLY install Qualcomm's NPU SDK (modified from https://developer.qualcomm.com/software/qualcomm-neural-processing-sdk/getting-started)

Assumed prerequisites (very important):

Ubuntu 16.04

Install latest Android Studio

Install latest SDK and NDK

Python 3.5.2

Caffe installation (modified from https://gist.github.com/arundasan91/b432cb011d1c45b65222d0fac5f9232c - without Anaconda (2)):

sudo apt-get update && sudo apt-get upgrade && sudo apt-get dist-upgrade && sudo apt-get autoremove

sudo apt-get -y install libopenblas-dev

echo 'deb http://archive.ubuntu.com/ubuntu trusty main restricted universe multiverse' >>/tmp/multiverse.list

sudo cp /tmp/multiverse.list /etc/apt/sources.list.d/

rm /tmp/multiverse.list

sudo apt-get update && sudo apt-get install libboost-all-dev

sudo apt-get -y install --fix-missing libboost-all-dev

sudo apt-get install libfaac-dev

sudo apt-get -y remove ffmpeg x264 libx264-dev

sudo add-apt-repository ppa:mc3man/trusty-media

sudo apt-get update && sudo apt-get install ffmpeg gstreamer0.10-ffmpeg

sudo apt-get install build-essential

sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev

sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev

sudo apt-get install libtiff4-dev libopenexr-dev libeigen2-dev yasm libopencore-amrnb-dev libtheora-dev libvorbis-dev libxvidcore-dev

sudo apt-get install python-tk libeigen3-dev libx264-dev libqt4-dev libqt4-opengl-dev sphinx-common texlive-latex-extra libv4l-dev default-jdk ant libvtk5-qt4-dev

cd ~

wget http://sourceforge.net/projects/opencvlibrary/files/opencv-unix/2.4.9/opencv-2.4.9.zip

unzip opencv-2.4.9.zip

cd opencv-2.4.9

mkdir build

cd build

cmake -D WITH_TBB=ON -D BUILD_NEW_PYTHON_SUPPORT=ON -D WITH_V4L=ON -D INSTALL_C_EXAMPLES=ON -D INSTALL_PYTHON_EXAMPLES=ON -D BUILD_EXAMPLES=ON -D WITH_QT=ON -D WITH_OPENGL=ON -D WITH_VTK=ON ..

sudo make -j4

sudo make install

sudo sh -c 'echo "/usr/local/lib" > /etc/ld.so.conf.d/opencv.conf'

sudo ldconfig

export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/opencv/lib

cd ~

sudo apt-get install python3-pip

sudo apt-get install python-numpy python-scipy python-matplotlib ipython ipython-notebook python-pandas python-sympy python-nose

sudo pip3 install cython

sudo pip3 install scikit-image scikit-learn

sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev

sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev protobuf-compiler

sudo pip3 install protobuf

sudo apt-get install python-pydot

git clone https://github.com/BVLC/caffe

cd caffe

cp Makefile.config.example Makefile.config

in Makefile.config:

1. Uncomment (No space in the beginning):

    CPU_ONLY := 1

2. Uncomment:

    USE_PKG_CONFIG := 1
    
3. change INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include to INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial/

4. change LIBRARIES += glog gflags protobuf boost_system boost_filesystem m hdf5_hl hdf5 to LIBRARIES += glog gflags protobuf boost_system boost_filesystem m hdf5_serial_hl hdf5_serial

5. create symbolic link (find the first file by going to /usr/lib/x86_64-linux-gnu/ and listing the contents. The versions should be 10.1.0 and 10.0.2 respectively): sudo ln -s libhdf5_serial_hl.so.10.0.2 libhdf5_hl.so and sudo ln -s libhdf5_serial.so.10.1.0 libhdf5.so

6. Uncomment the Python3 section 

7. In the Python3 section, change boost_python3 to boost_python-py35

cd ~/caffe/python

sudo pip3 install -r requirements.txt

sudo pip3 install tensorflow==1.0.0 (must be tensorflow 1.0.0)

cd ~/caffe

sudo make all -j4

sudo make pycaffe

sudo make distribute

sudo make test

sudo make runtest

Add this to the .bashrc file in /home/user/, replacing <username> with your system's username:
  
export CAFFE_ROOT=/home/<username>/caffe/
    
export PYTHONPATH=/home/<username>/caffe/distribute/python:$PYTHONPATH
    
export PYTHONPATH=/home/<username>/caffe/python:$PYTHONPATH

sudo reboot

python3

import caffe (should have no error messages)

Setup the SDK:

Download the latest Qualcomm Neural Processing SDK from the site

Unpack the .zip file to the ~/snpe-sdk folder

sudo apt-get install python-dev python-matplotlib python-numpy python-protobuf python-scipy python-skimage python-sphinx wget zip

sudo pip3 install mako sphinx wget zip python-dateutil pyyaml scikit-image matplotlib scipy numpy (just in case)

source ~/snpe-sdk/bin/dependencies.sh

source ~/snpe-sdk/bin/check_python_depends.sh

Initialize the Qualcomm Neural Processing SDK environment (do this for every future console):

cd ~/snpe-sdk/

export ANDROID_NDK_ROOT=~/Android/Sdk/ndk

source ./bin/envsetup.sh -c ~/caffe

source ./bin/envsetup.sh -t ~/tensorflow

Download the ML Models and convert them to .DLC:

Download and convert a pre-trained Alexnet example in Caffe format:

cd $SNPE_ROOT

python3 ./models/alexnet/scripts/setup_alexnet.py -a ./temp-assets-cache -d

Download and convert a pre-trained inception_v3 example in Tensorflow format:

cd $SNPE_ROOT

python3 ./models/inception_v3/scripts/setup_inceptionv3.py -a ./temp-assets-cache -d

Build the Example Android App:

cd $SNPE_ROOT/examples/android/image-classifiers

cp ../../../android/snpe-release.aar ./app/libs

bash ./setup_alexnet.sh

bash ./setup_inceptionv3.sh

Launch Android Studio

Open the project in the ~/snpe-sdk/examples/android/image-classifiers folder

Upgrade all the build components as suggested by Android Studio.

Press Run app to build and run the APK.

If using a Google Pixel 4, and the device is not showing up, go to the device settings, go to connected devices, and change
the USB mode to file transfer, and then accept all permissions.
