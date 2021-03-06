# Python and Jupyter

Zhonghua Zheng (zhonghua.zheng@outlook.com)

Last update: 2022/02/17

## Introduction

Here we would 

- use Jupyter notebook on HPC with a GPU

## Prerequisites

Make sure you have a conda environment ("partmc") available.

If the "partmc" conda environment is not available, please follow the "**Conda Installation**"

## use Jupyter notebook on HPC with a GPU

**step 1: run the following script**

Please change the partition and gpu configuration accordingly

```bash
#!/bin/bash

# if use gpu:
srun --partition=ctessum --nodes=1 --time=03:00:00 --gres=gpu:QuadroRTX6000:1 --pty bash -i
# cpu only:
# srun --partition=ctessum --nodes=1 --time=03:00:00 --pty bash -i
source activate partmc
echo "ssh -N -L 8880:`hostname -i`:8880 $USER@cc-login.campuscluster.illinois.edu"
jupyter notebook --port=8880 --no-browser --ip=`hostname -i`
```
Note: if the command from "echo" doesn't work. Please use the command below as a replacement
```bash
echo "ssh -t -t $USER@cc-login.campuscluster.illinois.edu -L 8880:localhost:8880 ssh `hostname` -L 8880:localhost:8880"

# a reference for UCAR's HPC
echo "ssh -N -L 8880:`hostname`:8880 $USER@`hostname`.ucar.edu"
jupyter notebook --port=8880 --no-browser --ip=`hostname`
```

Note: if you are using **keeling**:

```bash
qsub -I -l select=1:ncpus=32 -l walltime=24:00:00
source activate
conda activate partmc
echo "ssh -N -L 8880:127.0.0.1:8880 $USER@keeling.earth.illinois.edu"
jupyter notebook --port=8880 --no-browser --ip=127.0.0.1
```

**step 2: launch a new terminal, copy and paste the command printed by the "echo" command, and log in**

**step 3: open your browse (e.g., Google Chrome), type `https://localhost:8880`**

## Trouble Shooting 

GPU relevant command

```bash
$ lspci | grep -i nvidia
$ nvidia-smi
```

kill session:

```bash
$ ps -u your_netid -f | grep ssh
$ kill -9 session_id
```

for nfs:

```bash
$ lsof | grep nfs00000
$ kill -9 session_id
```

