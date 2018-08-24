# Compiling Python 3.7.0 on RHEL/Centos 6.9
Unfortunately python 3.7.0 requires:
- zlib 1.1.3 (better 1.1.4) or upper
- openssl 1.1.0 or upper
- Also, you will need libffi-devel rpm


## Set Environment
```
PYTHON_VERSION = 3.7.0
PYTHON_URL = https://www.python.org/ftp/python/${PYTHON_VERSION}/Python-${PYTHON_VERSION}.tgz
mkdir $HOME/opt
export LOCAL=$HOME/opt
mkdir $HOME/tmp
```

## Compiling zlib 1.2.11
```
cd $HOME/tmp
wget -c https://zlib.net/zlib-1.2.11.tar.gz
tar xzvf zlib-1.2.11.tar.gz
cd zlib-1.2.11
./configure --prefix=$LOCAL
make
make install
```

## Compiling OpenSSL 1.1.0
```
cd $HOME/tmp
wget -c https://www.openssl.org/source/openssl-1.1.0i.tar.gz
tar xzvf openssl-1.1.0i.tar.gz
cd openssl-1.1.0i
./config --prefix=$LOCAL/openssl --openssldir=$LOCAL/openssl \
--with-zlib-include=/root/opt/lib
export LD_LIBRARY_PATH=$LOCAL/lib
make
make test TESTS=01-test_sanity V=1
make install
```

## Compiling Python 3.7.0
```
cd $HOME/tmp
wget -c https://www.python.org/ftp/python/3.7.0/Python-3.7.0.tgz
tar xzvf Python-3.7.0.tgz
cd Python-3.7.0
 ./configure --enable-optimizations --with-openssl=$LOCAL/openssl --with-ssl-default-suites=openssl  --prefix=$LOCAL/python-3.7.0
export LD_LIBRARY_PATH=$LOCAL/openssl/lib:$LD_LIBRARY_PATH
make
make test <optional>
make altinstall
```
## Using Python 3.7.0
```
export LD_LIBRARY_PATH=$LOCAL/openssl/lib:$LOCAL/opt/lib
export PATH=$LOCAL/python-3.7.0/bin:$PATH
python3.7
>>> import sys
>>> print(sys.version)
```

