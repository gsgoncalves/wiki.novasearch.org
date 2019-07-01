---
title: Installing Software
permalink: wiki/Installing_Software/
layout: wiki
---

IN PROGRESS: Some guidelines/tutorials for software installation in the
Rocks Cluster.

Python Stuff
------------

### General Advice

Compute nodes have a set of useful libraries installed like numpy,
scipy, pandas, etc... The pip tool, which allows one to easily install
new libraries and is also available. It may be used to install any
python library from the Python Package Index or from a git repository.
Example of a command to install a package/library:

`pip install `<package_name>

Since we are simple peasants on the cluster and we don't have sudo (it
makes a lot of sense :)) we need to install libraries locally. To
achieve this, one option is to add the option `--user` to the pip
install command:

`pip install `<package_name>` --user`

For more info see:
[1](http://pip-python3.readthedocs.org/en/latest/user_guide.html#user-installs)

To upgrade a library we specify the argument `--upgrade`, like this:

`pip install --upgrade `<package_name>` --user`

### Machine Learning General Packages

#### Scipy

The Scipy package[2](https://www.scipy.org/) provides a collection of
mathematics, science, and engineering related libraries. It includes
packages like numpy, matplotlib,pandas, ipython, among others.

As any other package, Scipy can be installed by issuing the command:

`pip install --upgrade scipy --user`

However, it is likely that it will fail due to the fact that python
isn't able to find a BLAS library. Since our cluster Rocks, we have MKL
from Intel! To use it, we need to load it and tell pip which MKL version
we wan't numpy to use.

To load mkl on the cluster:

`module load mkl` (confirm if lapack is needed!)

-   Create a configuration file for numpy named `.numpy-site.cfg` in the
    $HOME folder:

  
  
`touch /home/dsemedo/.numpy-site.cfg`

-   Copy the contents of a sample numpy config file available at
    [3](https://github.com/numpy/numpy/blob/master/site.cfg.example) to
    the created file.

Change the MKL section to:

`# MKL`  
`# ----`  
`# MKL is Intel's very optimized yet proprietary implementation of BLAS and`  
`# Lapack.`  
`# For recent (9.0.21, for example) mkl, you need to change the names of the`  
`# lapack library. Assuming you installed the mkl in /opt, for a 32 bits cpu:`  
`[mkl]`  
`library_dirs = /opt/intel/composer_xe_2013_sp1.2.144/mkl/lib/intel64 `  
`include_dirs = /opt/intel/composer_xe_2013_sp1.2.144/mkl/include`  
`mkl_libs = mkl_rt`

After this, Scipy installs correctly and MKL is used in Numpy.

#### scikit-learn

`pip install --upgrade scikit-learn --user`

### Deep Learning Packages

#### Theano

Theano is a Python library that allows the definition, optimization, and
evaluation of mathematical expressions involving multi-dimensional
arrays efficiently.

Install the latest version of Theano using the git repository:

`pip install --upgrade `[`https://github.com/Theano/Theano/archive/master.zip`](https://github.com/Theano/Theano/archive/master.zip)` --user`

Theano supports CPU and GPU. To select which we want to use the Theano
flags must be set:

-   Setting the flags for each execution:

  
  
<code>THEANO\_FLAGS='floatX=float32,device=gpu0' python

<script>

.py</code>

-   Alternatively we can create a config file:

  
  
`echo -e "[global]\nfloatX=float32\ndevice = gpu0\n" > ~/.theanorc`

With this config, Theano will attempt to use the GPU for computations.
If it fails to find a GPU, it will fallback to the CPU.

Additionally, we want Theano to also use MKL:

-   Modify Theano config file by adding:

`[blas]`  
`ldflags = -L/opt/intel/composer_xe_2013_sp1.2.144/mkl/lib/intel64 -L/opt/intel/composer_xe_2013_sp1.2.144/compiler/lib/intel64 -lmkl_gf_lp64 -lmkl_intel_lp64 -lmkl_intel_thread -lmkl_gnu_thread -lmkl_core -lmkl_vml_avx -lmkl_def -ldl -lpthread -lm -lmkl_rt -liomp5`

For more info about Theano flags
see[4](http://deeplearning.net/software/theano/library/config.html).

#### Lasagne

Lasagne is a lightweight library to build and train neural networks in
Theano. It depends on Theano, therefore it must be installed first. To
install Lasagne:

`pip install --upgrade `[`https://github.com/Lasagne/Lasagne/archive/master.zip`](https://github.com/Lasagne/Lasagne/archive/master.zip)` --user`

Lasagne documentation:
[5](http://lasagne.readthedocs.org/en/latest/index.html)

#### Keras

"Keras is a minimalist, highly modular neural networks library, written
in Python and capable of running on top of either TensorFlow or Theano.
It was developed with a focus on enabling fast experimentation. Being
able to go from idea to result with the least possible delay is key to
doing good research."

Dependencies:

-   cv2 -\> `pip install cv2 --user`

`pip install git+git://github.com/Theano/Theano.git --user`

Keras Documentation: [6](http://keras.io/)

### Computer Vision Packages

#### scikit-image

`pip install --upgrade scikit-image --user`
