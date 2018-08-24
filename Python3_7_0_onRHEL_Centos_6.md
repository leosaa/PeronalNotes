# How to install Python 3.7.0 on RHEL/Centos 6.9

This is an introduction to building and installing software from source on Red Hat or Centos 6.
Unfortunately python 3.7.0 requires and RHEL/Centos 6.9 provides previous versions:
So previos to configure pyhton, you have to configure zlib and openssl

## Requirements
- zlib 1.1.3 (better 1.1.4) or upper
- openssl 1.1.0 or upper
- Also, you will need libffi-devel rpm


## Step 1: Set Environment
```
PYTHON_VERSION=3.7.0
PYTHON_URL=https://www.python.org/ftp/python/${PYTHON_VERSION}/Python-${PYTHON_VERSION}.tgz
mkdir $HOME/opt
LOCAL=$HOME/opt
mkdir $HOME/tmp
DL=$HOME/tmp
yum install libffi-devel
```

## Building zlib 1.2.11
```
cd ${DL}
wget -c https://zlib.net/zlib-1.2.11.tar.gz
tar xzvf zlib-1.2.11.tar.gz
cd zlib-1.2.11
./configure --prefix=${LOCAL}
make
make install
```

## Building OpenSSL 1.1.0
```
cd ${DL}
wget -c https://www.openssl.org/source/openssl-1.1.0i.tar.gz
tar xzvf openssl-1.1.0i.tar.gz
cd openssl-1.1.0i
./config --prefix=${LOCAL}/openssl \
--openssldir=${LOCAL}/openssl \
--with-zlib-include=${LOCAL}/lib
export LD_LIBRARY_PATH=${LOCAL}/lib
make
make test TESTS=01-test_sanity V=1
make install
```

## Building Python 3.7.0
```
cd ${DL}
wget -c https://www.python.org/ftp/python/3.7.0/Python-3.7.0.tgz
tar xzvf Python-3.7.0.tgz
cd Python-3.7.0
./configure --enable-optimizations \
--with-openssl=${LOCAL}/openssl \
--with-ssl-default-suites=openssl \
--prefix=${LOCAL}/python-3.7.0
export LD_LIBRARY_PATH=${LOCAL}/openssl/lib:$LD_LIBRARY_PATH
make
make test <optional>
make altinstall
```
It is critical that you use make altinstall when you install your custom version of Python. If you use the normal make install you will end up with two different versions of Python in the filesystem both named python. This can lead to problems that are very hard to diagnose.

## Using Python 3.7.0

```
export LD_LIBRARY_PATH=${LOCAL}/openssl/lib:${LOCAL}/opt/lib
export PATH=${LOCAL}/python-3.7.0/bin:$PATH
```

### Check version
```
python3.7 -V
 Python 3.7.0
python3.7
 >>> import sys
 >>> print(sys.version)
 3.7.0 (default, Aug 24 2018, 12:03:48) 
 [GCC 4.4.7 20120313 (Red Hat 4.4.7-18)]
 >>> 
```
