# Running docker containers with access to Nvidia GPUs
Docker is great for abstracting away underlying infrastructure - neatly packaging the application and its environment to be run on any hardware. Some dockerized applications require access to GPU, e.g. machine learning applications with Tensorflow. 

This article describes how dokcerized applications can utilize nvidia GPU hardware.

**Note**: Instructions are for **Arch linux** users, but should be applicable, with minor adjustments, for any linux user.

## tl;dr
 - Running GPU enabled docker containers decouples CUDA installations from your host system. This is great!
 - Nvidia supports docker. Package: nvidia-container-runtime ([Link](https://githubcom/NVIDIA/nvidia-container-runtime)) needs to be installed on the host system
 - Nvidia drivers installed on host machine must support the CUDA version that is run in the dockerized container
 - Start a docker with GPU by running: **docker run --gpus all**

## Benefits
Remove the tedious process of setting up the correct CUDA, CUDNN and nvidia driver versions on your system. With a docker approach you only need to make sure that the the nvidia driver, installed on your host system, supports a certain version of CUDA/CUDNN.

You can find a compability matrix between nvidia drivers and CUDA here: [Link](https://docs.nvidia.com/deploy/cuda-compatibility/index.html)


## 1. Install nvidia-container-runtime
This is an open source package, provided by Nvidia, that allows dockers to access the GPU. 

Install by doing the following.
```sh
git clone https://aur.archlinux.org/nvidia-container-runtime.git

# Install the AUR package
cd nvidia-container-toolkit/
makepkg -si
```

## 2. Check your version of nvidia drivers
The nvidia driver will determine what version of CUDA you will be able to run in the docker container. 
``` sh
nvidia-smi
```

```sh
# Output:
Tue Oct  6 19:45:42 2020
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 435.21       Driver Version: 435.21
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  GeForce GTX 108...  Off  | 00000000:01:00.0  On |                  N/A |
|  0%   54C    P0    68W / 275W |   1085MiB / 11175MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|    0       759      G   /usr/lib/Xorg                                120MiB |
|    0      1071      G   /usr/lib/Xorg                                307MiB |
|    0      1142      G   /usr/bin/gnome-shell                         569MiB |
|    0      2554      G   /usr/lib/firefox/firefox                       2MiB |
|    0      2741      G   /usr/lib/firefox/firefox                       2MiB |
|    0      2863      G   /usr/lib/firefox/firefox                       2MiB |
|    0      3277      G   /usr/lib/firefox/firefox                       2MiB |
|    0      3825      G   ...AAAAAAAAAAAACAAAAAAAAAA= --shared-files    41MiB |
+-----------------------------------------------------------------------------+
```
With driver version **435** I will be able to run CUDA version up to **10.1**

##  3. Run a docker image with CUDA installed
Now you are ready to run a docker utilizing the underlying GPU. Either you build your image and install a CUDA version you need, or you start out with an already existing image like: nvidia/cuda:9.0-base

```sh
# Run the docker with GPU enabled (all, to pass all gpus- in case of several) and execute nvidia-smi in the docker.
docker run --gpus all nvidia/cuda:9.0-base nvidia-smi
```

```sh
# Output from within the docker:
Tue Oct  6 19:45:42 2020
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 435.21       Driver Version: 435.21       CUDA Version:9.0       |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  GeForce GTX 108...  Off  | 00000000:01:00.0  On |                  N/A |
|  0%   54C    P0    68W / 275W |   1085MiB / 11175MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|                                                                             |
+-----------------------------------------------------------------------------+
```