### The Future of MLOps and How Did We Get Here?
### Chris Sterry | Vice President of Operations | Dotscience
- They started this coming from the DevOps mindset (DevOps for ML)

*MLOps requirements*
1. Reproducible: You can re-train a model built 9 months ago and reproduce it within tolerance
2. Accountable: You can recover the raw data, training data, and build procedure of every model in production
3. Collaborative: You can prove someone else's model works correctly without talking to them
4. Continuous: You can statistically monitor your models to correct abnormal behavior in production

"In the deployment what I’ve typically found in talking to ML teams is right now it seems like a data scientist is expected to be a software engineer and, in some organizations, to be a master of DevOps as well. And often the gap between data scientist and DevOps and software engineering is pretty broad; And when you have to wear all those hats it’s: how do you simplify that? Then from a monitoring standpoint, I always say that if you knew the right answer then you wouldn't need machine learning. Models can go wonky quickly in production without normal monitoring techniques, and so you need to do statistical monitoring."

*Workflows*
```txt
DevOps
------
Code─>─Test─>─Deploy─├
  └─<───Monitor──<───└

MLOps
-----
Data runs   Code─>─├   Parameters─├
 ├  └───>───Model runs──────<─────└
 |         └─>─Models/metrics─>─Deploy─├
 └───────<────────Monitor──────<───────└
    
```








You are here: .
              ├── src
              |   └── module
              |       └── .py source files
              ├── test
              |   └── .py test files
              ├── setup.cfg
              ├── setup.py
              ├── tasks.py
              ├── requirements.txt
              └── Dockerfile









- the

*Windows 10*

```txt
You are here: .
              ├── src
              |   └── module
              |       └── .py source files
              ├── test
              |   └── .py test files
              ├── setup.cfg
              ├── setup.py
              ├── tasks.py
              ├── requirements.txt
              └── Dockerfile
```

## Examples

```py
"""
Testing the speeds of compilers

https://docs.sympy.org/latest/modules/codegen.html#ufuncify-method
https://ojensen.wordpress.com/2010/08/10/fast-ufunc-ish-hydrogen-solutions/
https://stackoverflow.com/questions/36250303/fortran-sources-but-no-fortran-compiler-found
"""


import timeit
import numpy as np
from sympy import sin
from sympy.abc import x, y
from sympy.physics.hydrogen import R_nl
from sympy.utilities.autowrap import ufuncify
from sympy.utilities.lambdify import lambdify

expr = x + y
f = ufuncify((x, y), expr, backend='numpy')

N = int(1e6)
v1 = np.linspace(1, N, N)
v2 = np.linspace(1, N, N)

timeit.timeit('f(v1, v2)', 'from __main__ import f, v1, v2', number=10000)    

timeit.timeit('v1 + v2', 'from __main__ import v1, v2', number=10000)    

expr = x**2
f = ufuncify((x), expr, backend='numpy')
f(4)

expr = sin(x)/x
f = ufuncify((x), expr, backend='numpy')

expr = R_nl(3, 1, x, 6)
expr

fn_lamb = lambdify(x, expr, 'numpy')
fn_ufun = ufuncify((x), expr, backend='numpy')

xx = np.linspace(0, 1, 5)
fn_lamb(xx)
fn_ufun(xx)

timeit.timeit('fn_lamb(xx)', 'from __main__ import fn_lamb, xx', number=10000)    

timeit.timeit('fn_ufun(xx)', 'from __main__ import fn_ufun, xx', number=10000)    

```
