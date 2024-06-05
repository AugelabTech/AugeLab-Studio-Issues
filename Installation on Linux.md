# Installation manual of AugeLab Studio Headless on Linux/Docker
This documentation will walk you through installation of augelab studio on a Linux distro. You may follow the same steps to dockerize whole process.

> [!TIP]
> When dockerizing the whole process, remove `apt-get update` commands to enable caching. Removing is not necessary but significantly shorthens consecutive building times.


## Install Python 3.9
```commandLine
apt-get update
apt-get install -y software-properties-common
add-apt-repository -y ppa:deadsnakes/ppa
apt-get update
apt-get install -y python3.9 python3.9-distutils python3.9-venv
apt-get install -y libpython3.9
apt-get install -y udev ffmpeg libsm6 libxext6
```

## Make python3.9 the default Python version
```
update-alternatives --install /usr/bin/python python /usr/bin/python3.9 1 && \
    update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.9 1
```

## Install pip

```
apt-get install -y python3-pip
```

[](https://github.com/user-attachments/files/15573418/requirements.txt)
[](https://github.com/user-attachments/files/15573404/requirements_linux.txt)
[](https://github.com/user-attachments/files/15573405/requirements_no_deps.txt)

## Install dependencies
```
wget "https://github.com/user-attachments/files/15573418/requirements.txt"
wget "https://github.com/user-attachments/files/15573404/requirements_linux.txt"
wget "https://github.com/user-attachments/files/15573405/requirements_no_deps.txt"
python3 -m pip install --upgrade pip
python3 -m pip install --index-url http://studio-desktop-repo-v3.s3-website.eu-central-1.amazonaws.com --trusted-host studio-desktop-repo-v3.s3-website.eu-central-1.amazonaws.com studio augelab_file_utils augelab_platform boto3 botocore
python3 -m pip install --extra-index-url http://studio-desktop-repo-v3.s3-website.eu-central-1.amazonaws.com --trusted-host studio-desktop-repo-v3.s3-website.eu-central-1.amazonaws.com studio==2.1.0 -r requirements.txt  -r requirements_linux.txt
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
