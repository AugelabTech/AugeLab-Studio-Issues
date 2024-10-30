# Installation manual of AugeLab Studio Headless on Linux/Docker
This documentation will walk you through installation of augelab studio on a Linux distro. You may follow the same steps to dockerize whole process.


## Install Python 3.9
```commandLine
apt-get update
apt-get install -y software-properties-common
add-apt-repository -y ppa:deadsnakes/ppa
apt-get update
apt-get install -y python3.9 python3.9-distutils python3.9-venv libpython3.9 udev ffmpeg libsm6 libxext6 libdmtx0b zbar-tools
```

## Make python3.9 the default Python version
```
update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.9 1
```

## Install pip

```
apt-get install -y python3-pip
```

[](https://github.com/user-attachments/files/17471688/requirements.txt)
[](https://github.com/user-attachments/files/17471694/requirements_linux.txt)
[](https://github.com/user-attachments/files/17471695/requirements_no_deps.txt)

## Install dependencies
```
wget -O https://github.com/user-attachments/files/17471688/requirements.txt requirements.txt
wget -O https://github.com/user-attachments/files/17471694/requirements_linux.txt requirements_linux.txt
wget -O https://github.com/user-attachments/files/17471695/requirements_no_deps.txt requirements_no_deps.txt
python3 -m pip install --upgrade pip
python3 -m pip install --upgrade --index-url http://studio-desktop-repo-v3.s3-website.eu-central-1.amazonaws.com --trusted-host studio-desktop-repo-v3.s3-website.eu-central-1.amazonaws.com studio augelab_file_utils augelab_platform boto3 botocore tee
python3 -m pip install --upgrade --extra-index-url http://studio-desktop-repo-v3.s3-website.eu-central-1.amazonaws.com --trusted-host studio-desktop-repo-v3.s3-website.eu-central-1.amazonaws.com -r requirements.txt  -r requirements_linux.txt
python3 -m pip install -r requirements_no_deps.txt --no-deps
python3 -m pip install vidgear[core]~=0.2.4 --no-deps
python3 -m pip install --upgrade --no-deps --force-reinstall urllib3==1.*
python3 -m pip install flask --ignore-installed
```

## Test success
```commandLine
python3 -c "import studio; print(studio.__version__)"
```

## Further Reading
To use the AugeLab Studio Headless, please refer to [Studio API](https://github.com/AugelabTech/AugeLab-Studio-Issues/blob/main/StudioAPI.md) documentation.
