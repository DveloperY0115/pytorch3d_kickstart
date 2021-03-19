# Introduction

This short document covers the following:

- How to configure Anaconda virtual environments on Ubuntu
- How to install Pytorch3D and its dependencies
- Sample code (brought from official tutorial) for validating installation

Below is the specification of the machine used (March 19th, 2021):
- OS: Ubuntu 18.04
- GPU: Nvidia RTX 3090 (Ampere architecture, **compute_86**)
- GPU Driver Version: 460.56       
- CUDA Version: 11.2 

# Install Anaconda

This is a brief summary of official Anaconda installation guide. You can find the full text [here](https://docs.anaconda.com/anaconda/install/linux/).

You can get the installer using the following command:

```bash
cd ~    
wget https://repo.anaconda.com/archive/Anaconda3-2020.11-Linux-x86_64.sh
```

To get the latest installer, visit [Download Anaconda Installer](https://www.anaconda.com/products/individual#Downloads). Then right click the installer of your preference, and copy & paste the link after `wget` command.

After downloading, you can find `.sh` file for Anaconda installation run:

> (Optional but Recommended) You may want to check whether the downloaded file is corrupted or not. If you do, open a terminal and run:
`sha256num /path/filename` 

```bash
bash {filename}.sh
```

**NOTE: Use the bash command regardless of whether or not you are using Bash shell.**

You can verify the installation by using any `conda` commands in your terminal. 

> However, if you're using `zsh` rather than `bash` (which I'm using), you may have to register the path to Anaconda binary in order to use it.

# Install Pytorch and Pytorch3D

## Setup Environmen & Install Pytorch

**The first thing to do is to make a conda virtual environment.** 

> It's okay to install everything on the system Python, but it will make things harder when things start to messed up. So I strongly recommend you to do it in virtual environments.
 
```bash
conda create -n pytorch3d python=3.8
conda activate pytorch3d
pip install torch==1.7.1+cu110 torchvision==0.8.2+cu110 -f https://download.pytorch.org/whl/torch_stable.html
python -c "import torch; print(torch.__version__); import torchvision; print(torchvision.__version__); print(torch.cuda.is_available())"
```

 Then the outputs should be:

```bash
1.7.1+cu110
0.8.2+cu110
True
 ```

## Install Additional Dependencies

 And we should install `cudatoolkit` as well (The latest release at this time is version *11.0.221*):

 ```bash
 conda install -c anaconda cudatoolkit 
 ```

 We have just installed Pytorch in this environment. However, Pytorch3D requires more dependencies and they can be installed by:

 ```bash
 conda install -c conda-forge -c fvcore -c iopath fvcore iopath
conda install -c bottler nvidiacub
 ```

 ## Pytorch3D

 > Before we begin, there's something to clarify. I spent time to resolve installation issue (mainly on Pytorch3D) and below is the way I succeeded. However, there might be factors that I didn't even notice, but are crucial to success. **This is not a perfect way to do it so if you have problem, please let me know.**

 According to this discussion([How to use it with cuda11 and rtx3090?](https://github.com/facebookresearch/pytorch3d/issues/421)), it seems that Pytorch3D didn't worked well at its first release.  
 > There were compatibility problems with CUDA 11.1 as well. Fortunately we're using 11.2.     

Furthermore, neither of these `conda` command didn't work for me.

```bash
# Anaconda Cloud (Stable build)
conda install pytorch3d -c pytorch3d

# Anaconda Cloud (Unstable build)
conda install pytorch3d -c pytorch3d-nightly
```

**So my walk around for this problem was to use the third option mentioned in [Pytorch3D INSTALL.md](https://github.com/facebookresearch/pytorch3d/blob/master/INSTALL.md).** 

Since I'm using `Python 3.8`, `Pytorch 1.7.1` and `CUDA 11.+`, the command used was:

```bash
pip install pytorch3d -f https://dl.fbaipublicfiles.com/pytorch3d/packaging/wheels/py38_cu110_pyt171/download.html

```

This successfully installed `Pytorch3D 0.4.0` on my machine, and I was good to go.

# Run Example Code

To verify the installation, I used one of the tutorial codes introduced in [Pytorch3D Github repository](https://github.com/facebookresearch/pytorch3d).

If you clone this repository, everything you need is in it. Thus, simply run the entire notebook and check whether the installation was successful or not. 

# Additional Note

I extracted my conda environment settings to `requirements.txt` file. So you may use it to compare your env with mine or to install everything from scratch by using (but this may not work since I installed some packages using `pip`):

```bash
cd /path/to/repo
conda env create -n {EnvName} -f requirements.txt
```

Thank you for reading :)
 
