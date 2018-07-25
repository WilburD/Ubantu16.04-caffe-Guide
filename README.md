# Ubantu16.04-caffe-Guide
Roadmap to caffe congratuation
Guide:Ubantu 16.04+cuda8.0+cudnn6.0+opencv3.6+python2.7/3.5(GPU)

1st: install all the independence related to all the applications.

	>  sudo apt-get update
	>  sudo apt-get upgrade
	>  sudo apt-get install -y build-essential cmake git pkg-config
	>  sudo apt-get install -y libprotobuf-dev libleveldb-dev libsnappy-dev libhdf5-   serial-dev protobuf-compiler libatlas-base-dev libboost-all-dev libgflags-dev libgoogle-glog-dev liblmdb-dev
 	>  sudo apt-get install --no-install-recommends libboost-all-dev
  	>  sudo apt-get install libopenblas-dev liblapack-dev libatlas-base-dev
  	>  sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev


# (Python general)
    > sudo apt-get install -y python-pip

# (Python 2.7 development files)
	> sudo apt-get install -y python-dev
	> sudo apt-get install -y python-numpy python-scipy

# (or, Python 3.5 development files)
	> sudo apt-get install -y python3-dev
	> sudo apt-get install -y python3-numpy python3-scipy
 
# (OpenCV 2.4)
	> sudo apt-get install -y libopencv-dev

(or, OpenCV 3.3 - see the instructions below)

2nd: install the Nvidia driver. Before installing the driver,u need to search for the version responding to ur sys. conf. by running the command below:

   	> sudo ubuntu-drivers devices
	
	> sudo add-apt-repository -qy ppa:graphics-drivers/ppa
	> sudo apt-get -qy update
	> sudo apt-get -qy install nvidia-XXX
	> sudo apt-get -qy install mesa-common-dev
	> sudo apt-get -qy install freeglut3-dev
	> sudo reboot

3rd: install cuda 8.0
   The LATEST version of Cuda Toolkit 8.0 is available from the NVIDIA website. Download the Cuda Toolkit 8.0 network installer from the NVIDIA site, after registering and filling out the forms. Note that all we need is to obtain the runfile package by clicking the link below.
	>  https://developer.nvidia.com/cuda-downloads
	Then follow the instruction in the terminal window
	
    > chmod u+x ./cuda_8.0.27_linux.run
    > sudo ./cuda_8.0.27_linux.run –tmpdir=/tmp –override
	
   As u have installed the Nvidia accelerated graphics driver above,pay more attiontion that u need to type no option when asking u whether installing the driver.
   After finishing installing,there would be a summary shown as below,if same to urs.congtats,u make it.The cuda has been installed succesfully.
	
	>  source ~/.bashrc
	
  Adding the following instruction  to the file at the end.

	> export PATH=/usr/local/cuda-8.0/bin${PATH:+:${PATH}}
	> export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64${LD_LIBRARY_PATH:+:$ {LD_LIBRARY_PATH}}

4th:install cudnn6.0

Register an account in order to download and install the Debian (.deb) package that contains the CUDNN library from https://developer.nvidia.com/cudnn.

	> sudo dpkg -i libcudnn7_7.0.2.38-1+cuda8.0_amd64.deb
	> sudo dpkg -i libcudnn7-dev_7.0.2.38-1+cuda8.0_amd64.deb
	> reboot
	
Then type as follows to verify whether it works or not.

	> Nvidia-smi
	> cd ~/NVIDIA_CUDA-8.0_Samples/1_Utilities/deviceQuery
	> make
	> ./deviceQuery

5th:install opencv3.6

Firstly,all the dependences should be installed for the work environment for opencv3.6.

	> sudo apt-get install --assume-yes build-essential cmake git
	> sudo apt-get install --assume-yes pkg-config unzip ffmpeg qtbase5-dev python-dev python3-dev python-numpy python3-numpy
	> sudo apt-get install --assume-yes libopencv-dev libgtk-3-dev libdc1394-22 libdc1394-22-dev libjpeg-dev libpng12-dev libtiff5-dev libjasper-dev
	> sudo apt-get install --assume-yes libavcodec-dev libavformat-dev libswscale-dev libxine2-dev libgstreamer0.10-dev libgstreamer-plugins-base0.10-dev
	> sudo apt-get install --assume-yes libv4l-dev libtbb-dev libfaac-dev libmp3lame-dev libopencore-amrnb-dev libopencore-amrwb-dev libtheora-dev
	> sudo apt-get install --assume-yes libvorbis-dev libxvidcore-dev v4l-utils vtk6
	> sudo apt-get install --assume-yes liblapacke-dev libopenblas-dev libgdal-dev checkinstall

Secondly,Download the latest source archive for OpenCV 3.3 from the website below. https://github.com/opencv/opencv/archive/3.3.0.zip
Enter the unpacked directory. Execute:

	> mkdir build
  	> cd build/
	> cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D FORCE_VTK=ON -D WITH_TBB=ON -D WITH_V4L=ON -D WITH_QT=ON -D WITH_OPENGL=ON -D WITH_CUBLAS=ON -D CUDA_NVCC_FLAGS="-D_FORCE_INLINES" -D WITH_GDAL=ON -D WITH_XINE=ON -D BUILD_EXAMPLES=ON ..
	> make -j $(($(nproc) + 1))

Well now lets install the opencv3.6,at first start,using make as follows:

	> sudo make install
	> sudo /bin/bash -c 'echo "/usr/local/lib" > /etc/ld.so.conf.d/opencv.conf'
	> sudo ldconfig
	> sudo apt-get update
	> reboot

6th:caffe
Go to the https://github.com/BVLC/caffe and download the zip archive. Unpack it to ~/bin/ or any other location. Enter the caffe-master directory in the terminal window. U will see there is the file termed Makefile.config.example.Copy the Makefile.config.example to Makefile.config.and open it for editing (with a text editor).

There are many things should be modified in Makefile and Makefile.config. File where responding to ur device configuration. But still there are loads of common things could be changed. Such as :

Makefile.config:
Change the first instruction to the second one.

	> INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include
	> INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial/

Also open the file CMakeLists.txt and add the following line:

	> # ---[ Includes 
	set(${CMAKE_CXX_FLAGS} "-D_FORCE_INLINES ${CMAKE_CXX_FLAGS}")

Makefile:
Change the first instruction to the second one.

	> LIBRARIES += glog gflags protobuf boost_system boost_filesystem m hdf5_hl hdf5
	> LIBRARIES += glog gflags protobuf boost_system boost_filesystem m hdf5_serial_hl hdf5_serial

Change the first instruction to the second one.

	> LIBRARIES += boost_thread stdc++
	> LIBRARIES += boost_thread stdc++ boost_regex

Change the first instruction to the second one.

	> NVCCFLAGS += -ccbin=$(CXX) -Xcompiler -fPIC $(COMMON_FLAGS)
	> NVCCFLAGS += -D_FORCE_INLINES -ccbin=$(CXX) -Xcompiler -fPIC $(COMMON_FLAGS)

Still there exits some errors,u need to back to the two files above related to the error and change some settings.

7th: build caffe

	> make all
	> make test
	> make runtest
	> make pycaffe
	> make distribute
	
Note as ur building caffe or pycaffe,search more on the internet to find out how to solve all the problems errors shown in the terminal window,as u dealt with all the errors,all the environment u established will work to the caffe models,and I wanna tell u,u made it.

