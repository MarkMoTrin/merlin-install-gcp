# merlin-install-gcp

Instructions for installing Merlin on GCP, not using Vertex AI endpoints. The goal of this set of instructions is to be able to run [Merlin](https://github.com/NVIDIA-Merlin/Merlin) examples on a GCP VM.

More specifically, we will be testing the [Merlin-models Session-based recommender examples](https://github.com/NVIDIA-Merlin/models/blob/main/examples/usecases/transformers-next-item-prediction.ipynb) 

**Note**, The easiest way to get started is using [one-click deploy on Vertex AI](https://catalog.ngc.nvidia.com/orgs/nvidia/collections/vertexaiworkbench), (sometimes the containers may not be the latest release from the Merlin project). This will **not** be the focus of this tutorial.
- See [Vertex Ai instructions here](https://github.com/GoogleCloudPlatform/nvidia-merlin-on-vertex-ai)

## Prerequisites

- Sign up for [NVIDIA GPU Cloud - NGC](https://catalog.ngc.nvidia.com/), this allows you to pull NVIDIA containers to use our software. This is by far the easiest way. 
  - Generate a [NGC API Key](https://docs.nvidia.com/ngc/ngc-overview/index.html#generating-api-key) - This enables you to pull containers from the command line withing your VM/system
- Have access to GPU enabled GCP VM
  - Make sure to use at least one V100 16GB. Preferably A100s for larger dataset testing

## High-level Steps (simple)
- Get access to NVIDIA containers
- Launch GPU-enabled VM - this will determine the complexity of the installation. 
  - If your VM already has CUDA Drivers and nvidia-docker installed then things, installation will be be much simpler. 
  - If not, 
- Pull NVIDIA Merlin containers
- Launch containers with Jupyterlab
- Build great performing models

# Detailed Steps Option #1
- Start a GCP instance with Ubuntu 18.04 base image + GPU (**no Pascal GPUs, only Volta, Tesla and Ampere**)
  - V100 with 16GB (GCP has no V100 with 32GB), A100 with 40GB is better for larger datasets

```
sudo apt update -y && \
    sudo apt install -y build-essential && \
    sudo apt install -y --no-install-recommends software-properties-common
```

- Installing NVIDIA Driver + Docker + NVIDIA Docker Support
```
# Install NVIDIA Driver
wget https://developer.download.nvidia.com/compute/cuda/11.4.1/local_installers/cuda_11.4.1_470.57.02_linux.run
sudo sh cuda_11.4.1_470.57.02_linux.run

# Install Docker
sudo apt install docker.io

# Install NVIDIA Support for Docker
distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
      && curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
      && curl -s -L https://nvidia.github.io/libnvidia-container/$distribution/libnvidia-container.list | \
            sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
            sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
sudo apt-get -y update
sudo apt install nvidia-container-toolkit
sudo nvidia-ctk runtime configure --runtime=docker
sudo systemctl restart docker
```

- Pull Container and start it
```
# Test if GPU support for Docker is installed
sudo docker run --rm --runtime=nvidia --gpus all nvidia/cuda:11.6.2-base-ubuntu20.04 nvidia-smi
```

- Launch Merlin Containers - Note, Merlin containers already have Jupyter Installed
```
# Pull Merlin Container
sudo docker pull nvcr.io/nvidia/merlin/merlin-tensorflow:22.12
sudo docker run --rm -it --runtime=nvidia --gpus all -p 8888:8888 -p 8797:8787 -p 8796:8786 nvcr.io/nvidia/merlin/merlin-tensorflow:22.12 /bin/bash
```
- Connecting via Jupyter Notebook (
  - Install jupyter lab inside the docker container and start it if not available) 
  - add public SSH key to VM instance (via webinterface): https://cloud.google.com/compute/docs/connect/add-ssh-keys (you can add SSH keys to a current running machine)
  - connect and tunnel the ports to the machine 
  
  ```
  ssh -i <private key> -L localhost:8888:localhost:8888 <username>@<external ip> - Note: that the username needs to match the one used to install and needs to match the ssh key (you can modify the last characters of the public key)
  ```


