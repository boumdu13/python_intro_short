Modules and Packages
====================
Introduction
------------
The core Python programming language can be extended with so-called modules and packages. The standard library, which ships with every Python installation, is a collection of numerous modules for various purposes.

A *module* is a Python script that we can import in our own code in order to use whatever is defined in the module. A *package* organizes a collection of modules. We will see examples for both modules and packages later in this chapter.

Importing modules
-----------------
We have already used functions from the `math` module (which is part of the standard library). Before using functions defined in a module, we have to import either the whole module or only specific functions from the module. Usually (by PEP8 convention), we import all modules at the top of a script.

Here's an example where we import the whole `math` module and then use some of its names and functions:

```python
>>> import math
>>> math.pi
3.141592653589793
>>> math.e
2.718281828459045
>>> math.sin(math.pi)
1.2246467991473532e-16  # practically zero (limited float precision)
>>> math.cos(math.pi)
-1.0
```

After `import math`, we can access the contents of the module by prepending `math.` to the actual names contained in the module (`math.pi`, `math.e`, `math.sin`, and so on).

The `dir` function lists all definitions contained in a module (we can ignore the dunder names, which are reserved for internal use):

```python
>>> dir (math)
['__doc__', '__file__', '__loader__', '__name__', '__package__', '__spec__', 'acos', 'acosh', 'asin', 'asinh', 'atan', 'atan2', 'atanh', 'ceil', 'comb', 'copysign', 'cos', 'cosh', 'degrees', 'dist', 'e', 'erf', 'erfc', 'exp', 'expm1', 'fabs', 'factorial', 'floor', 'fmod', 'frexp', 'fsum', 'gamma', 'gcd', 'hypot', 'inf', 'isclose', 'isfinite', 'isinf', 'isnan', 'isqrt', 'ldexp', 'lgamma', 'log', 'log10', 'log1p', 'log2', 'modf', 'nan', 'perm', 'pi', 'pow', 'prod', 'radians', 'remainder', 'sin', 'sinh', 'sqrt', 'tan', 'tanh', 'tau', 'trunc']
```

If we only want to use a limited number of functions from a module, we can save us some typing by importing functions from a module like this:

```python
>>> from math import sqrt, pi
>>> sqrt(4)
2.0
>>> 2 * 3**2 * pi
56.548667764616276
```

That way, we can use the functions directly as `sqrt` and `pi` instead of `math.sqrt` and `math.pi`, respectively.

Writing custom modules
----------------------
We are not restricted to using predefined modules &ndash; Python lets us write our own modules. This is especially useful to structure a longer program into several files. For example, let's assume that we wrote our own `mean` function which computes the arithmetic mean of a list of numbers:

```python
def mean(values):
    s = 0
    for v in values:
        s += v
    return s / len(values)
```

We could put this function in our own module called `stats`. This is just a Python file called `stats.py`, and its sole contents is the function definition.

Note that this file should be saved in the current working directory, that is, the directory that the Python interpreter is working in. In IPython, we can find out what the working directory is by typing `pwd` (print working directory). If this is not the location we want, we can change the working directory using the `cd` (change directory) command (followed by the path to the directory we would like to use).

We can then import either our `stats` module or the `mean` function from the `stats` module directly:

```python
>>> import stats
>>> dir(stats)
['__builtins__', '__cached__', '__doc__', '__file__', '__loader__', '__name__', '__package__', '__spec__', 'mean']
```

The only non-dunder name is indeed our `mean` function, which we can now use as:

```python
>>> stats.mean(range(1, 101))
50.5
```

Alternatively, we just import the `mean` function:

```python
>>> from stats import mean
>>> mean(range(1, 101))
50.5
```

Optionally, we can rename either the imported module or the imported function during import:

```python
>>> import stats as st
>>> st.mean([1, 2, 3])
2.0
```

```python
>>> from stats import mean as m
>>> m(range(50, 76))
62.5
```

Sometimes, this is useful when modules or function names are very long. In addition, some popular third-party modules like NumPy are imported with shorter names by convention:

```python
>>> import numpy as np  # convention
```

Behind the scenes, nothing fancy is happening when we use alternative names for imported modules. This is just a shorter version for writing:

```python
>>> import numpy  # imports module and assigns the name numpy
>>> np = numpy  # create another name np (an alias)
>>> del numpy  # delete the original name numpy (we still have np)
```

Selected examples from the standard library
-------------------------------------------
We will now briefly discuss some common modules from the standard library. First, note that our `mean` function is already implemented in the `statistics` module:

```python
>>> import statistics
>>> statistics.mean(range(1, 101))
50.5
```

This module contains some additional useful statistical functions (check out the output of `dir(statistics)` for an exhaustive list).

Generating random numbers is a common task in many applications. Python includes the `random` module, which contains many functions related to random number generation.

```python
>>> import random
```

Note that computers cannot produce true random numbers. Instead, they use sophisticated algorithms to generate pseudo-random numbers, which behave like true random numbers but can be generated by a deterministic algorithm. It is good practice to initialize the random number generator with `random.seed` before drawing random numbers, because this makes the code reproducible. Setting the random seed makes sure that we get the exact same random numbers whenever we run our script. Let's look at an example, which draws three random integers from the range of 0 to 100:

```python
>>> random.seed(42)  # initialize the random number generator
>>> random.randint()
>>> random.randint(0, 100)
81
>>> random.randint(0, 100)
14
>>> random.randint(0, 100)
3
```

The initial function call `random.seed(42)` puts the random number generator in a defined state. The concrete number `42` is not important, but if we use the same number every time we re-run the script, we will get the same random numbers. In other words, if we run the previous five lines again, we will get the same random numbers 81, 14, and 3.

The `random` module has many more extremely useful functions. For example, we can randomly sample from a list with `random.choice` (we do not set `random.seed` every time for the sake of brevity):

```python
>>> x = ["a", "b", "c", "d", "e"]
>>> random.choice(x)
'c'
>>> random.choice(range(1, 1001))
251
```

There is also a function that randomly shuffles a the elements of a list in-place:

```python
>>> x = ["a", "b", "c", "d", "e"]
>>> random.shuffle(x)
>>> x
['b', 'd', 'a', 'c', 'e']
```

This concludes our extremely short tour of some important modules in the standard library.

Packages
--------
A package organizes several modules into a hierarchy. This is useful, because it allows us to group related modules (which makes each module shorter instead of having one module containing a lot of code).

Python uses the directory structure to define a package. If a directory contains an (empty) file named `__init__.py`, Python treats the directory as a package. This way, several modules can be combined in a package like in the following example. Let's assume we have a number of modules related to sound processing (this is an [example from the official Python documentation](https://docs.python.org/3/tutorial/modules.html#packages)). We would like to create a `sound` package to organize all of our modules. Here's a directory structure we could use for our package:

```
sound/
      __init__.py
      formats/
              __init__.py
              wavread.py
              wavwrite.py
              aiffread.py
              aiffwrite.py
              auread.py
              auwrite.py
              ...
      effects/
              __init__.py
              echo.py
              surround.py
              reverse.py
              ...
      filters/
              __init__.py
              equalizer.py
              vocoder.py
              karaoke.py
              ...
```

Each directory containing a file called `__init__.py` is a package. In our example, we have a `sound` package, which contains three sub-packages called `formats`, `effects`, and `filters`. Each of these three sub-packages contains a number of modules (`wavread.py`, `wavwrite.py`, ...).

Let's see how we can import the contents of the package in Python. We can import individual modules with dot notation:

```python
>>> import sound.effects.echo
```

We can then use functions defined in that module, but we have to prepend the full name. For example, calling the function `echofilter` in that module need to be written like this:

```python
>>> sound.effects.echo.echofilter(input, output, delay=0.7, atten=4)
```

Alternatively, we can import the module `echo` as:

```python
>>> from sound.effects import echo
```

This makes calling functions from that module shorter:

```python
>>> echo.echofilter(input, output, delay=0.7, atten=4)
```

Finally, we could also import just the function `echofilter`:

```python
>>> from sound.effects.echo import echofilter
>>> echofilter(input, output, delay=0.7, atten=4)
```

Or even shorter:
```python
>>> from sound.effects.echo import echofilter as ef
>>> ef(input, output, delay=0.7, atten=4)
```

The packages NumPy, SciPy, Pandas, and Matplotlib are essential tools for performing data science with Python. By convention, these packages are imported as follows:

```python
import numpy as np
import scipy as sp
import pandas as pd
import matplotlib.pyplot as plt
```

Exercises
---------

1. Import the `math` module and print $\pi$ on the screen. Furthermore, list all objects contained in the module.

2. Import the variance function from the `statistics` module as `var`. Compute the result of this expression:
   ```python
   var([13, 27.75, 11.56, 22, 17, 32.22, 26.7])
   ```

3. Create a module called `textutils`, which contains a function `is_palindrome`. This function should check whether its argument is a palindrome or not by returning `True` or `False`, respectively. Call the function from this module in a script with two different strings.

4. The `scipy` package contains many functions for scientific computing. Among others, `scipy` contains a `stats` subpackage. Find a function which computes the Pearson correlation coefficient, and use this function to calculate the correlation between the following two lists of numbers:
   ```python
   x = [1, 2, 3, 4]
   y = [11, 7, 9, 3]
   ```

---
![https://creativecommons.org/licenses/by-nc-sa/4.0/](cc_license.png) This document is licensed under the [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) by Clemens Brunner.