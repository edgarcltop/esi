# Learning ESI

This repository provides an end-to-end library for automatic character rigging, skinning, and blend shapes generation, as well as a visualization tool. It is based on our work [Learning Skeletal Articulations with Neural Blend Shapes]hat is published in SIGGRAPH 2021.



## Prerequisites

Our code has been tested on Ubuntu 18.04. Before starting, please configure your Anaconda environment by

~~~bash
conda env create -f environment.yaml
conda activate neural-blend-shapes
~~~

Or you may install the following packages (and their dependencies) manually:

- pytorch 1.8
- tensorboard
- tqdm
- chumpy

Note the provided environment only includes the PyTorch CPU version for compatibility consideration.

## Quick Start

We provide a pretrained model that is dedicated for biped characters. Download and extract the pretrained model from [Google Drive](https://drive.google.com/file/d/1S_JQY2N4qx1V6micWiIiNkHercs557rG/view?usp=sharing) 

~~~bash
python demo.py --pose_file=./eval_constant/sequences/greeting.npy --obj_path=./eval_constant/meshes/maynard.obj
~~~

## Train from Scratch

We provide instructions for retraining our model.

Note that you may need to reinstall the PyTorch CUDA version since the provided environment only includes the PyTorch CPU version.

To train the model from scratch, you need to get the training set.

The training process contains tow stages, each stage corresponding to one branch. To train the first stage, please run

~~~bash
python train.py --envelope=1 --save_path=[path to save the model] --device=[cpu/cuda:0/cuda:1/...]
~~~

For the second stage, it is strongly recommended to use a pre-process to extract the blend shapes basis then start the training for much better efficiency by

~~~bash
python preprocess_bs.py --save_path=[same path as the first stage] --device=[computing device]
python train.py --residual=1 --save_path=[same path as the first stage] --device=[computing device] --lr=1e-4
~~~

