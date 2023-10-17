# How to install CUDA, cuDNN, VSCode, Miniconda, Pytorch on Ubuntu 22.04

## Install NVIDIA drivers

### Update & upgrade
```shell
sudo apt update && sudo apt upgrade
```

### Remove previous NVIDIA installation (if any)
```shell
!sudo apt autoremove nvidia* --purge
```

### Check Ubuntu devices
```shell
ubuntu-drivers devices
```
You will install the NVIDIA driver whose version is tagged with __recommended__


### Install Ubuntu drivers
```shell
sudo ubuntu-drivers autoinstall
```

### Install NVIDIA drivers
My __recommended__ version is 535, adapt to yours

```shell
sudo apt install nvidia-driver-535
```

### Reboot & Check
```shell
reboot
```
after restart verify that the following command works
```shell
nvidia-smi
```

## Install CUDA drivers

### Update & upgrade
```shell
sudo apt update && sudo apt upgrade
```

### Install CUDA toolkit
```shell
sudo apt install nvidia-cuda-toolkit
```

### Check CUDA install
```shell
export "PATH=/usr/local/cuda/bin:$PATH"
nvcc --version
```

### export path to the .bashrc file
You need to edit .bashrc file with administrator privileges so you need not to export local cuda path manually everytime you start terminal.
Please add following line in .bashrc file

export "PATH=/usr/local/cuda/bin:$PATH"

```shell
sudo nano ./.bashrc
export "PATH=/usr/local/cuda/bin:$PATH"
```

## Install cuDNN

### Download cuDNN .deb file
You can download cuDNN file [here](https://developer.nvidia.com/rdp/cudnn-download). You will need an Nvidia account.
Select the cuDNN version for the appropriate CUDA version, which is the version that appears when you run:
```shell
nvcc --version
```

### Install cuDNN
```shell
cd ~/Downloads
sudo apt install ./cuda-repo-ubuntu2204-12-2-local_12.2.2-535.104.05-1_amd64.deb
rm ./Downloads/cudnn-local-repo-ubuntu2204-8.9.5.29_1.0-1_amd64.deb
sudo cp /var/cudnn-local-repo-ubuntu2204-8.9.5.29/cudnn-local-275FA572-keyring.gpg /usr/share/keyrings/
```

My cuDNN version is 8, adapt the following to your version:

```shell
sudo apt update
sudo apt install /var/cudnn-local-repo-ubuntu2204-8.9.5.29/libcudnn8_8.9.5.29-1+cuda12.2_amd64.deb
sudo apt install /var/cudnn-local-repo-ubuntu2204-8.9.5.29/libcudnn8-dev_8.9.5.29-1+cuda12.2_amd64.deb
sudo apt install /var/cudnn-local-repo-ubuntu2204-8.9.5.29/libcudnn8-samples_8.9.5.29-1+cuda12.2_amd64.deb
```

## Install VS Code
```shell
sudo dpkg -i ./Downloads/code_1.83.1-1696982868_amd64.deb 
rm ./Downloads/code_1.83.1-1696982868_amd64.deb
```
## Install pip and check installation directory
```shell
sudo apt install python3-pip
which -a pip
```

## Install Miniconda
```shell
sudo bash ~/Downloads/Miniconda3-latest-Linux-x86_64.sh
rm ~/Downloads/Miniconda3-latest-Linux-x86_64.sh
conda config --set auto_activate_base false
conda deactivate
conda update conda
```


## Test CUDA on Pytorch within a virtual conda environment

### Create Miniconda virtual environment and activate it
```shell
conda info --env
conda create --name env_torch
conda activate env_torch
```

### Install pytorch
```shell
conda install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia
```

### Open Python kernel in VSCODE and execute a test
```python
import torch
print(torch.cuda.is_available()) # should be True

t = torch.rand(10, 10).cuda()
print(t.device) # should be CUDA
```