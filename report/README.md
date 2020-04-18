*Windows 10*

## Linux Subsystem
*Install Ubuntu 18.04 from the Microsoft Store*

*Open Ubuntu and set up your username and password*

Update Ubuntu
```bash
/.../.$ sudo apt-get update
/.../.$ sudo apt-get install build-essential
```

Install desired python versions
```bash
/.../.$ sudo apt-get install python3.7-dev
/.../.$ sudo apt-get install python3.8-dev
```

Set up python pointers
```bash
/.../.$ sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.7 1
/.../.$ sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.8 2
```

Choose a pointer
```bash
/.../.$ sudo update-alternatives --config python <<< 1
```

Check out the python version being used
```bash
/.../.$ python -V
```

Install virtualenv
```bash
/.../.$ sudo apt-get install virtualenv
```

## Virtual Environment Linux Build
Given the following folder structure:
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

*Click on the file explorer path, type 'bash', and hit 'Enter' to open up the linux command line*

Pick which python version to use in the virtual environment
```bash
/.../.$ sudo update-alternatives --config python <<< 1
```

Create a virtual environment
```bash
/.../.$ rm -r env
/.../.$ virtualenv --python=python env
```

Install module requirements
```bash
/.../.$ export CPYTHON_VERSION=3.8
/.../.$ env/bin/pip install -r requirements.txt
```

Start up the virtual environment
```bash
/.../.$ source env/bin/activate
(env) /.../.$ invoke clean
```

Check the formatting of your code
```bash
(env) /.../.$ invoke fmtcheck
```

Check the data typing in your code
```bash
(env) /.../.$ invoke typecheck
```

Score your code
```bash
(env) /.../.$ invoke lint
```

See if the module can work in developer mode
```bash
(env) /.../.$ invoke scan
(env) /.../.$ invoke build
```

See if the module can work in production mode
```bash
(env) /.../.$ invoke uninstall
(env) /.../.$ invoke install
(env) /.../.$ invoke test
```

Go to the wheel file
```bash
(env) /.../.$ cd dist
```

Shutdown the virtual environment
```bash
(env) /.../.$ deactivate
```

Delete the virtual environment
```bash
/.../.$ rm -r env
```

## Docker Build
Given the following folder structure:
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

*Click on the file explorer path, type 'cmd', and hit 'Enter' to open up the windows command line*

Stop all docker containers
```cmd
...\.> FOR /f "tokens=*" %i IN ('docker ps -a -q') DO docker stop %i
```

Delete all stopped containers, networks, images, and build cache
```cmd
...\.> docker system prune -af
```

Delete all persisted data
```cmd
...\.> docker volume prune -f
```

See if the module can be built
```cmd
...\.> docker build . -t buildpython
```

## Virtual Environment Windows Wheel File
Install a version of python:
* Look for Windows executable installer
* Don't put python on PATH
* Remember path to Python**
*https://www.python.org/downloads/release/python-380/*

Given the following folder structure:
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

*Click on the file explorer path, type 'cmd' or 'bash', and hit 'Enter' to open up the windows or linux command line*

Create a virtual environment using your desired python version
```cmd
...\.> virtualenv env -p C:\Users\morrisn\AppData\Local\Programs\Python\Python38\python.exe
```

Start up the virtual environment
```cmd
...\.> .\env\Scripts\activate
```

Install library requirements
```cmd
(env) ...\.> pip install -r requirements.txt
```

Build a .whl file of the library
```cmd
(env) ...\.> python setup.py bdist_wheel
```

Go to the wheel file
```cmd
...\.> cd dist
```

## Unit Test
Given the following folder structure:
```txt
You are here: .
              └── module
                  ├── test
                  |   └── .py test files
                  └── .py source files
```

*Click on the file explorer path, type 'cmd', and hit 'Enter' to open up the windows command line*

Format your source and test code
```cmd
...\.> black module\.
```

Score your source and test code
```cmd
...\.> pylint module\.
```

Get a coverage report on your unit tests
```cmd
...\.> pytest module\test\ --cov-branch --cov-report term-missing --cov=module\
```

## Examples

```python
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
