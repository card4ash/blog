Title: Python Packaging
Published: 12/07/2020
Tags:
    - Introduction
    - Python
    - Package
---

# Python Packaging Step by Step Guide

1. Create a src folder
Optionally create virtual environment 
```python -m venv env```
Activate virtual environment
```.\env\Scripts\activate```
install the follwing package (need it later)
```pip install wheel```

2. Create a file helloworld.py and put it in src folder
```python
def say_hello(name=None):
    if name is None:
        return "Hello, World!"
    else:
        return f"Hello, {name}!"
```
3. Creat a file setup.py in the same dir as src 
```python
from setuptools import setup

setup(
    name='helloworld',
    version='0.0.1',
    description='Say hello!',
    py_modules=["helloworld"],
    package_dir={'':'src'},
)
```
4. Run the below code
```python setup.py bdist_wheel```   
(```pip install wheel``` if in a venv)
5. testing our packaging---->>> ```pip install -e .```
	5.1 type python in current dir 
	```python
	>>>from helloworld import say_hello
	>>> say_hello()
	'Hello, World!'
	>>> say_hello("Everybody")
	'Hello, Everybody'
6. https://www.toptal.com/developers/gitignore
	6.1 Add gitignore file
7. https://choosealicense.com/licenses/mit/#
	7.1 Add LICENSE.txt
8. https://pypi.org/classifiers/
	8.1 Example.https://pythonhosted.org/an_example_pypi_project/setuptools.html
setup.py looks like as follows
```python
from setuptools import setup

setup(
    name='helloworld',
    version='0.0.1',
    description='Say hello!',
    py_modules=["helloworld"],
    package_dir={'':'src'},
    classifiers=[
        "Development Status :: 3 - Alpha",
        "Topic :: Utilities",
        "Programming Language :: Python :: 3.7",
        "Programming Language :: Python :: 3.7",
        "Programming Language :: Python :: 3.8",
        "License :: OSI Approved :: MIT License",
    ],
    
)
```
9. Add README.md
10. install_requires
11. Install dev depandancy
setup.py looks as follows
```python
from setuptools import setup

with open("README.md","r") as fh:
    long_description=fh.read()

setup(
    name='helloworld',
    version='0.0.1',
    description='Say hello!',
    py_modules=["helloworld"],
    package_dir={'':'src'},
    classifiers=[
        "Development Status :: 3 - Alpha",
        "Topic :: Utilities",
        "Programming Language :: Python :: 3.7",
        "Programming Language :: Python :: 3.7",
        "Programming Language :: Python :: 3.8",
        "License :: OSI Approved :: MIT License",
    ],
    long_description=long_description,
    long_descrioption_content_type="text/markdown",
    install_requires=[
        "blessings~=1.7",
    ],
    extras_require={
        "dev":[
            "pytest>=3.7",
        ],
    },
)
```
12. See below

# Developing Hello World
To install helloworld, along with the tools you need to develop and run tests, run the following in your virtualenv:
```bash
pip install -e .[dev]
```
13. source distribution
```python setup.py sdist```
14. 
```
pip install check-manifest
check-manifest --create
```

final setup.py looks as follows:
```
from setuptools import setup

with open("README.md", "r") as fh:
    long_description = fh.read()

setup(
    name='helloworld-ashcoder',
    version='0.0.1',
    description='Say hello!',
    py_modules=["helloworld"],
    package_dir={'':'src'},
    classifiers=[
        "Development Status :: 3 - Alpha",
        "Topic :: Utilities",
        "Programming Language :: Python :: 3.7",
        "Programming Language :: Python :: 3.7",
        "Programming Language :: Python :: 3.8",
        "License :: OSI Approved :: MIT License",
        "Operating System :: OS Independent",
    ],
    long_description=long_description,
    long_descrioption_content_type="text/markdown",
    install_requires=[
        "blessings~=1.7",
    ],
    extras_require={
        "dev":[
            "pytest >= 3.7",
            "check-manifest",
            "twine",
        ],
    },
    url="https://github.com/card4ash/pythonpackaging",
    author="Ashraful Alam",
    author_email="card4ash@live.com"
)
```

15.
```
python setup.py bdist_wheel sdist
```

16. 
```
twine upload dist/*
```

# Links
1. [https://github.com/judy2k/helloworld](https://github.com/judy2k/helloworld)
2. [https://github.com/card4ash/pythonpackaging](https://github.com/card4ash/pythonpackaging)
3. [https://www.youtube.com/watch?v=GIF3LaRqgXo&t=1500s&ab_channel=CodingTech](https://www.youtube.com/watch?v=GIF3LaRqgXo&t=1500s&ab_channel=CodingTech)