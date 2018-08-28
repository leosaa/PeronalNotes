# How to install python packages locally
There are (at least) three possible ways to install a python package in your $HOME/.local directory. 
All of them have to be followed by a configuration of your environment, more specifically the PYTHONPATH

## install with PIP
pip is the easy way to install python packages
```
pip install numpy --user
Collecting numpy
  Downloading https://files.pythonhosted.org/packages/1a/2e/4e298c92b1fced64a4414ada9af3253a91083b92b131c2b10c057c507982/numpy-1.15.1-cp37-cp37m-manylinux1_x86_64.whl (13.8MB)
    100% |################################| 13.9MB 1.7MB/s 
Installing collected packages: numpy
Successfully installed numpy-1.15.1
export PYTHONPATH=~/.local/lib/python3.7/site-packages
python3.7
Python 3.7.0 (default, Aug 24 2018, 12:03:48) 
[GCC 4.4.7 20120313 (Red Hat 4.4.7-18)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import numpy
>>> print (numpy.__version__)
1.15.1
>>>
```

## install from the sources
```
wget -c https://github.com/astropy/astropy/archive/v3.0.4.tar.gz
tar xzvf v3.0.4.tar.gz
cd astropy-3.0.4/
python3.7 setup.py install --prefix=$HOME/.local
 pip3.7 list
Package    Version
---------- -------
astropy    3.0.4  
numpy      1.15.1 
pip        10.0.1 
setuptools 39.0.1 
python3.7
Python 3.7.0 (default, Aug 24 2018, 12:03:48) 
[GCC 4.4.7 20120313 (Red Hat 4.4.7-18)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import astropy

```
