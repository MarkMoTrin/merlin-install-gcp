# merlin-install-gcp

Instructions for installing Merlin on GCP, not using Vertex AI endpoints. The goal of this set of instructions is to be able to run [Merlin](https://github.com/NVIDIA-Merlin/Merlin) examples on a GCP VM.

More specifically, we will be testing the [Merlin-models Session-based recommender examples](https://github.com/NVIDIA-Merlin/models/blob/main/examples/usecases/transformers-next-item-prediction.ipynb) 

**Note**, The easiest way to get started is using [one-click deploy on Vertex AI](https://catalog.ngc.nvidia.com/orgs/nvidia/collections/vertexaiworkbench), (sometimes the containers may not be the latest release from the Merlin project). This will **not** be the focus of this tutorial

## Prerequisites

- Sign up for [NVIDIA GPU Cloud - NGC](https://catalog.ngc.nvidia.com/), this allows you to pull NVIDIA containers to use our software. This is by far the easiest way. 
  - Generate a [NGC API Key](https://docs.nvidia.com/ngc/ngc-overview/index.html#generating-api-key) - This enables you to pull containers from the command line withing your VM/system
