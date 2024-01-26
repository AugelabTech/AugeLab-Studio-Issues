# Installation Guide for Python Package on Raspberry Pi 4B

This guide provides step-by-step instructions for installing a specific Python package on a Raspberry Pi 4B. It is designed for users with minimal knowledge of the Raspberry Pi.

## Prerequisites
1. **Raspberry Pi 4B**: Ensure you have a Raspberry Pi 4B.
2. **OS Image**: Download the recommended OS image from [Raspberry Pi Downloads](https://downloads.raspberrypi.com/raspios_armhf/images/raspios_armhf-2022-09-26/2022-09-22-raspios-bullseye-armhf.img.xz).

## Steps

### Step 1: Flashing the OS
1. Flash the downloaded OS image onto an SD card. Use the official Raspberry Pi Imager, available at the [Raspberry Pi website](https://www.raspberrypi.com/software/).

### Step 2: Initial Setup
1. Insert the flashed SD card into your Raspberry Pi 4B.
2. Connect your Raspberry Pi to a monitor, keyboard, and mouse.
3. Power up your Raspberry Pi and complete the initial setup instructions.

### Step 3: Connect to a wifi
Find a stable wifi connection and connect your raspberry pi to the internet.
To test the internet connection, open terminal and type:

```bash
ping -c 4 google.com
```

If you are seeing ping information and no errors, go on the next step.

### Step 4: Environment Setup and Dependencies
Run the following commands in the terminal to set up the environment and install necessary dependencies such as Python3.9:

```bash
echo "Environment: "
uname -a
echo "Updating environment"
sudo apt-get update
sudo apt update -y
sudo apt install -y python3-dev libopenjp2-7 ffmpeg
sudo apt install -y build-essential cmake pkg-config libjpeg-dev libopenblas-dev libtiff5-dev libpng-dev libavcodec-dev libavformat-dev libswscale-dev libv4l-dev libxvidcore-dev libx264-dev libfontconfig1-dev libcairo2-dev libgdk-pixbuf2.0-dev libpango1.0-dev libgtk2.0-dev libgtk-3-dev libatlas-base-dev gfortran libhdf5-dev libhdf5-serial-dev libhdf5-103 libqt5gui5 libqt5webkit5 libqt5test5 python3-pyqt5 python3-dev
sudo apt-get install libdmtx0b
sudo apt install -y software-properties-common
echo "Building python and setting python3(python3.9) as default command"
cd ..
wget https://www.python.org/ftp/python/3.9.15/Python-3.9.15.tar.xz
tar xf Python-3.9.15.tar.xz
cd Python-3.9.15
./configure --enable-optimizations --prefix=/usr
sudo make altinstall
cd ..
sudo rm -r Python-3.9.15
sudo rm Python-3.9.15.tar.xz
exec bash
sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.9 1
python3 --version
```

### Step 5: Installing the Python Package
Run the following commands to install the Python package using the files below:

[requirements.txt](https://github.com/AugelabTech/AugeLab-Studio-Issues/files/14061860/requirements.txt)
[requirements_armv7l.txt](https://github.com/AugelabTech/AugeLab-Studio-Issues/files/14061870/requirements_armv7l.txt)
[requirements_no_deps.txt](https://github.com/AugelabTech/AugeLab-Studio-Issues/files/14061874/requirements_no_deps.txt)

```bash
sudo apt install python3-pip -y
python3 --version
echo "Installing requirements"
python3 -m pip install --upgrade pip
python3 -m pip install --upgrade pip setuptools wheel
python3 -m pip install --pre -i https://pypi.anaconda.org/scipy-wheels-nightly/simple scipy
python3 -m pip install --pre -i https://pypi.anaconda.org/scipy-wheels-nightly/simple scikit-image
python3 -m pip install --pre -i https://pypi.anaconda.org/scipy-wheels-nightly/simple scikit-learn
python3 -m pip install ./build_dist/resources/rpi4_armv7l/packages/opencv_contrib_python-4.7.0.72-cp39-cp39-manylinux_2_31_armv7l.whl
python3 -m pip install vidgear[core]==0.2.4 --no-deps
mkdir requirements
wget --directory-prefix=./requirements/ https://github.com/AugelabTech/AugeLab-Studio-Issues/files/14061860/requirements.txt https://github.com/AugelabTech/AugeLab-Studio-Issues/files/14061870/requirements_armv7l.txt https://github.com/AugelabTech/AugeLab-Studio-Issues/files/14061874/requirements_no_deps.txt
python3 -m pip install --index-url http://studio-desktop-repo-v3.s3-website.eu-central-1.amazonaws.com --trusted-host studio-desktop-repo-v3.s3-website.eu-central-1.amazonaws.com studio==2.1.0 augelab_file_utils==1.0.2 augelab_platform==1.0.2 opencv-contrib-python==4.7.0 boto==1.24.51 botocore==1.27.51 s3transfer==0.6.0
python3 -m pip install -r ./requirements/requirements.txt
python3 -m pip install -r ./requirements/requirements_armv7l.txt
python3 -m pip install -r ./requirements/requirements_no_deps.txt --no-deps
python3 -c "import studio; print(studio.__version__)"
```

**Note**: Ensure that you are connected to the internet during the installation process. If any command fails, check your internet connection and try again.

### Completion
Once all the steps are completed without errors, studio should be successfully installed on your Raspberry Pi 4B.


### Further Guides
Current RPI4b release does not support a user interface. Which means you need to program your user interface or logical loop. For further guide, go to [Studio API Guide](StudioAPI.md)

---

**Disclaimer**: This guide is based on specific software versions and hardware. Future updates or different models may require adjustments to these instructions.
