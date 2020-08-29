Title: Python Getting Started Cheet Sheet
Published: 11/25/2019
Tags:
    - Python
    - Django
    - Introduction
---

Python Django GS
=================================
Installing virtualenv and virtualenvwrapper
**********************************************

Configure virtualenvwrapper and django (Windows)
**************************************************
```shell
    pip install virtualenvwrapper-win
	mkvirtualenv test
	workon test
	pip install django
	django-admin --version
	django-admin startproject hellodj
```

Install the virtualenv package
********************************

```shell
	python -m pip install --user virtualenv
```

Checking Installation 
**************************

```shell
	python -m virtualenv --version
```

Activating and Shutting down an enviroment
********************************************

```shell
	workon test
	deactivate
```


Quick Start Guide(recommended)
**********************************

```shell
    mkdir hellodj   
    cd hellodj
    python -m venv env
    .\env\Scripts\activate
    mkdir src
    pip install django
	pip freeze
    cd src
    django-admin --version
    django-admin --help
```


Downgrade or Install Specific python version Inside venv using conda
************************************************************************

create a virtual environment, install then switch to python 3.6.5

```python
	$ conda create -n tensorflow python=3.7
	$ conda activate tensorflow
	$ conda install python=3.6.5
	$ pip install tensorflow
    $ conda deactivate
```

conda environment path ```UserDirectory(C:\Users\ashraful.alam)\Anaconda3\envs```

Remove conda environment

```
    conda env remove --name myenv
```

Clear screen in shell
************************

```shell
    import os
    cls = lambda: os.system('cls')

    >>> cls()
```

colored text in Python Shell
*****************************

```
    from colorama import Fore
    from colorama import Style
    
    print ('This is {Fore.GREEN}color{Style.RESET_ALL}!')
```

Django Python Shell
*********************

where manage.py located cd into this location. Go to python shell with below command 
```shell
    python manage.py shell
```


