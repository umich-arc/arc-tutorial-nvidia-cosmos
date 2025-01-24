# arc-tutorial-nvidia-cosmos
Instructions for how to install and run NVidia's COSMOS model on ARC HPC systems

### Instructions for Great Lakes / Lighthouse
1) Downloading NVIDIAs/COSMOS model
    ```
    git clone https://github.com/NVIDIA/Cosmos.git 
    (or over SSH) git clone git@github.com:NVIDIA/Cosmos.git
    ```
1) Create environment
    ```
    cd Cosmos
    wget https://raw.githubusercontent.com/umich-arc/arc-tutorial-nvidia-cosmos/refs/heads/main/cosmos.yml
    ```
1) Load dependencies & create mamba environment
    ```
    module load gcc cuda/12.6 cudnn/12.6 mamba/py3.11
    source /sw/pkgs/arc/mamba/py3.11/etc/profile.d/conda.sh
    mamba env create -f cosmos.yml
    mamba activate cosmos_transformer
    ```
> [!WARNING]
> If CUDA dependent build errors occur with `mamba env create -f cosmos.yml`, then try running that command from within a GPU compute node.

For example

    salloc --partition=gpu --mem=30GB --gpus=1 --time=02:00:00
    module load gcc cuda/12.6 cudnn/12.6 mamba/py3.11
    source /sw/pkgs/arc/mamba/py3.11/etc/profile.d/conda.sh
    mamba env create -f cosmos.yml
    mamba activate cosmos_transformer

4) After successfully installing. Return to https://github.com/NVIDIA/Cosmos.git and follow instrunctions on downloading model weights and running the inference pipeline.
