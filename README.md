### DEPRECATED REPO

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
> [!TIP]
> The model weights can easily reach sizes between 300G - 500G. We recommend downloading the weights to a persistant high-performant storage volume such as Turbo or scratch (note; scratch is not a persistant storage, refer to ARC's scratch storage policy for more info).

## Setting Up COSMOS Model Weights and Parameters
To configure the COSMOS repository to use model weights and parameters stored in a shared nfs turbo location, follow these steps:

1) Place the Checkpoints in the Turbo Location
Ensure the COSMOS model weights and parameters (referred to as "checkpoints") are stored in the shared nfs turbo location. For example:
    `/nfs/turbo/arcts-sw-ops/cosmos/checkpoints`
   
2) Create a Symbolic Link in the Cosmos Repository
Navigate to the root of your local Cosmos repository and create a symbolic link pointing to the turbo location:
    ```
    cd /path/to/your/Cosmos
    ln -s /nfs/turbo/arcts-sw-ops/cosmos/checkpoints checkpoints
    ```
4) Verify the Setup
Run the following command to confirm that the symbolic link was created successfully:
    ```
    ls -l
    lrwxrwxrwx  1 user group    42 Jan 23 15:20 checkpoints -> /nfs/turbo/arcts-sw-ops/cosmos/checkpoints
    ```
This indicates that the checkpoints symlink in your Cosmos repository correctly points to the desired turbo location.

5) Use the Checkpoints
The repository will now use the model weights and parameters stored in the turbo location whenever it accesses the checkpoints directory.
