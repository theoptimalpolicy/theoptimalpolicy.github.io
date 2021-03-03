<<<<<<< HEAD
---
=======
```
>>>>>>> 6d718d15be31a404ddc69f0e6158f88a462893c2
title: "How do we call a C function from Python?"
subtitle: "Need to speed up things by calling a C function from your Python script? Check this."
layout: post
author: "Andrea Bonvini"
header-style: text
tags:
  - Programming
  - C
  - Python
mathjax: true
<<<<<<< HEAD
---
=======
```
>>>>>>> 6d718d15be31a404ddc69f0e6158f88a462893c2

This will be a really short blogpost which mainly serves as a personal reminder on how to call a C function from Python... the title is pretty self-explanatory.

I will show how to call some really simple `c` functions wich return a double or an array (which will be a `np.array` in Python).

Here we have our simple `c` file (`cfunctions.c`):

```python
import ctypes
import numpy as np

def factorial(num: int) -> int:
	py_cfunctions.factorial.restype = ctypes.c_long
	return py_cfunctions.factorial(num)

def dotproduct(dim: int, a: np.array, b: np.array) -> float:
	# Convert np.array to ctype doubles
	a_data = a.astype(np.double)
	a_data_p = a_data.ctypes.data_as(c_double_p)
	b_data = b.astype(np.double)
	b_data_p = b_data.ctypes.data_as(c_double_p)
	py_cfunctions.dotproduct.restype = ctypes.c_double
	# Compute result...
	return py_cfunctions.dotproduct(dim,a_data_p,b_data_p)
	
def elementwiseproduct(dim: int, a: np.array, b: np.array):
	# Convert np.array to ctype doubles
	a_data = a.astype(np.double)
	a_data_p = a_data.ctypes.data_as(c_double_p)
	b_data = b.astype(np.double)
	b_data_p = b_data.ctypes.data_as(c_double_p)

	py_cfunctions.elementwiseproduct.restype = np.ctypeslib.ndpointer(dtype=ctypes.c_double,shape=(dim,))
	# Compute result...
	return py_cfunctions.elementwiseproduct(dim,a_data_p,b_data_p)
	

# so_file genreated with:
# cc -fPIC -shared -o cfunctions.so cfunctions.c
so_file = 'MYPATH/cfunctions.so'  # <<<--------------------------------------------- CHANGE THIS LINE OBVIOUSLY!
py_cfunctions = ctypes.CDLL(so_file)
c_double_p = ctypes.POINTER(ctypes.c_double)
py_cfunctions.factorial.argtypes = [ctypes.c_int] 
py_cfunctions.dotproduct.argtypes= [ctypes.c_int, c_double_p, c_double_p]
py_cfunctions.elementwiseproduct.argtypes = [ctypes.c_int, c_double_p, c_double_p]
```

We then create a shared object witht the following command:

```bash
cc -fPIC -shared -o cfunctions.so cfunctions.c
```

Then we can write a Python script (`py_cfunctions.py`) such as:

```python
import ctypes
import numpy as np

def factorial(num: int):
	py_cfunctions.factorial.restype = ctypes.c_long
	return py_cfunctions.factorial(num)

def dotproduct(dim: int, a: np.array, b: np.array):
	# Convert np.array to ctype doubles
	a_data = a.astype(np.double)
	a_data_p = a_data.ctypes.data_as(c_double_p)
	b_data = b.astype(np.double)
	b_data_p = b_data.ctypes.data_as(c_double_p)
	py_cfunctions.dotproduct.restype = ctypes.c_double
	# Compute result...
	return py_cfunctions.dotproduct(dim,a_data_p,b_data_p)
	
def elementwiseproduct(dim: int, a: np.array, b: np.array):
	# Convert np.array to ctype doubles
	a_data = a.astype(np.double)
	a_data_p = a_data.ctypes.data_as(c_double_p)
	b_data = b.astype(np.double)
	b_data_p = b_data.ctypes.data_as(c_double_p)

	py_cfunctions.elementwiseproduct.restype = np.ctypeslib.ndpointer(dtype=ctypes.c_double,shape=(dim,))
	# Compute result...
	return py_cfunctions.elementwiseproduct(dim,a_data_p,b_data_p)
	

# so_file genreated with:
# cc -fPIC -shared -o cfunctions.so cfunctions.c
so_file = 'MY_PATH/cfunctions.so'
py_cfunctions = ctypes.CDLL(so_file)
c_double_p = ctypes.POINTER(ctypes.c_double)
py_cfunctions.factorial.argtypes = [ctypes.c_int] 
py_cfunctions.dotproduct.argtypes= [ctypes.c_int, c_double_p, c_double_p]
py_cfunctions.elementwiseproduct.argtypes = [ctypes.c_int, c_double_p, c_double_p]
```

And that's it! You now can import `py_cfunctions` and call `factorial()`, `dotproduct()` and `elementwiseproduct()`.

