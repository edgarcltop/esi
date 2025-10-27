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


### FBX Output (New!)

Now you can choose to output the animation as a single fbx file instead of a sequence of obj files! Simply do following:

~~~bash
python demo.py --animated_bvh=1 --obj_output=0
cd blender_scripts
blender -b -P nbs_fbx_output.py -- --input ../demo --output ../demo/output.fbx
~~~

Note that you need to install Blender (>=2.80) to generate the fbx file. You may explore more options on the generated fbx file in the source code.

This code is contributed by [@huh8686](https://github.com/huh8686).

### Test on Customized Meshes

You may try to run our model with your own meshes by pointing the `--obj_path` argument to the input mesh. Please make sure your mesh is triangulated and has a consistent upright and front facing orientation. Since our model requires the input meshes are spatially aligned, please specify `--normalize=1`. Alternatively, you can try to scale and translate your mesh to align the provided `eval_constant/meshes/smpl_std.obj` without specifying `--normalize=1`.

### Evaluation

To reconstruct the quantitative result with the pretrained model, you need to download the test dataset from [Google Drive](https://drive.google.com/file/d/1RwdnnFYT30L8CkUb1E36uQwLNZd1EmvP/view?usp=sharing) or [Baidu Disk](https://pan.baidu.com/s/1c5QCQE3RXzqZo6PeYjhtqQ) (8b0f) and put the two extracted folders under `./dataset` and run

~~~bash
python evaluation.py
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

