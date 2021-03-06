---
layout: post
title: Numpy vs PyTorch for Linear Algebra
category: Machine Learning
tags:
- machinelearning
---

Numpy is one of the most popular linear algebra libraries right now. There’s also PyTorch - an open source deep learning framework developed by Facebook Research. While the latter is best known for its machine learning capabilities, it can also be used for linear algebra, just like Numpy.

The most important difference between the two frameworks is naming. Numpy calls tensors (high dimensional matrices or vectors) arrays while in PyTorch there’s just called tensors. Everything else is quite similar.

## Why PyTorch?
Even if you already know Numpy, there are still a couple of reasons to switch to PyTorch for tensor computation. The main reason is the GPU acceleration. As you’ll see, using a GPU with PyTorch is super easy and super fast. If you do large computations, this is beneficial because it speeds things up a lot.

The other reason is the integration with other parts of the PyTorch framework. Most people use linear algebra for some kind of machine learning nowadays. In this case, using PyTorch is probably a better choice because the data can be used with the rest of the framework.

```python
import torch
```

## Why Numpy?
Numpy is the most commonly used computing framework for linear algebra. A good use case of Numpy is quick experimentation and small projects because Numpy is a light weight framework compared to PyTorch.

Moreover, PyTorch lacks a few advanced features as you’ll read below so it’s strongly recommended to use numpy in those cases.

```python
import numpy as np
```

## Using Both
Fortunately, using one framework doesn’t exclude the other. You can get the best of both worlds by converting between Numpy arrays and PyTorch tensors.

```python
# Numpy -> PyTorch
tensor = torch.from_numpy(np_array)

# PyTorch -> Numpy
ndarray = tensor.numpy()
```

## New tensors 
Numpy:
```python
zeros  = np.zeros((4, 4))
ones   = np.ones((4, 4))
random = np.random.random((4, 4))
```

PyTorch:
```python
zeros  = torch.zeros(4, 4)
ones   = torch.ones(4, 4)
random = torch.rand(4, 4)
```

## Basic Linear Algebra
### Indexing
Numpy:
```python
# Index item
array[0, 0] # returns a float

# Index row
array[0, :] # returns an array
```

PyTorch:
```python
# Index item
torch[0, 0] # returns a tensor

# Index row
torch[0, :] # returns a tensor
```

### Addition &amp; subtraction
Numpy:
```python
array + array2
array - array2
```

PyTorch:
```python
tensor + tensor2
tensor - tensor2
```

### (Element wise) multiplication
Numpy:
```python
# Element wise
array * array

# Matrix multiplication
array @ array
```

PyTorch:
```python
# Element wise
tensor * tensor

# Matrix multiplication
tensor @ tensor
```

### Shape and dimensions
Numpy:
```python
shap    = array.shape
num_dim = array.ndim
```

PyTorch:
```python
shape   = tensor.shape
shape   = tensor.size() # equal to `.shape`
num_dim = tensor.dim()
```

### Reshaping
Numpy:
```python
new_array = array.reshape((8, 2))
```

PyTorch:
```python
new_tensor = tensor.view(8, 2)
```

### Determinant 
Numpy:
```python
np.linalg.det(array)
```

PyTorch:
```python
# not natively supported
```

### Inverse and Moore-Pensore inverse
Numpy:
```python
# Inverse
np.linalg.inv(array)

# Moore Pensore inverse
np.linalg.pinv(array)
```

PyTorch:
```python
# Inverse
tensor.inverse()

# Moore Pensore inverse
tensor.pinverse()
```

### Sum/mean/std
These functions return floating point numbers in Numpy where PyTorch returns 1 by 1 tensors.

Numpy:
```python
# Sum
array.sum()

# Mean
array.mean()

# Standard Deviation
array.std()
```

PyTorch:
```python
# Sum
tensor.sum()

# Mean
tensor.mean()

# Standard Deviation
tensor.std()
```

### Transpose
Numpy:
```python
array.T
array.transpose()
```

PyTorch:
```python
tensor.t()
```

## Saving to disk
Saving results to disk is a huge time server. Here’s how you’d do it in Numpy or PyTorch.

Numpy:
```python
np.save('file.npy', array)
np.load('file.npy')
```

PyTorch:
```python
torch.save(tensor, 'file')
torch.load('file')
```

## Using the GPU
This is where PyTorch really shines. By copying an array to the GPU memory, there’s a huge potential for performance improvements due to heavy parallelization. Note that PyTorch uses CUDA under the hood so only NVIDIA GPUs are supported.

```python
# put the tensor in GPU memory
gpu_tensor = tensor.gpu()
```

In Google Colab I got a 20.9 time speed up in multiplying a 10000 by 10000 matrix by a scaler when using the GPU.

If you do an operation on two arrays, both must be either on the CPU or GPU.

## Further reading
* [PyTorch Tensor Documentation](https://pytorch.org/docs/stable/tensors.html)
* [Numpy Array Documentation](https://docs.scipy.org/doc/numpy/reference/arrays.html)

If there’s anything you’d like to see added, tweet me at [@rickwierenga](https://twitter.com/rickwierenga).

<p class="text-muted">A huge thanks to <a target="_blank" href="https://twitter.com/miserendino_sam">Sam Miserendino</a> for proofreading this post!</p>
