# Learning ESI

This repository provides an end-to-end library for automatic character rigging, skinning, and blend shapes generation, as well as a visualization tool. 

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

We provide a pretrained model that is dedicated for biped characters.
~~~bash
python demo.py --pose_file=./eval_constant/sequences/greeting.npy --obj_path=./eval_constant/meshes/maynard.obj
~~~
