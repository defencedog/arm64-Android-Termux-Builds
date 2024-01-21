# LXML WHL for ARM32 & PYTHON 3.9

This `7z` is created using following commands.
Within this archive is ready to use `lxml-5.1.0-cp39-cp39-linux_armv8l.whl`
located in `/dist` folder
```
wget https://files.pythonhosted.org/packages/2b/b4/bbccb250adbee490553b6a52712c46c20ea1ba533a643f1424b27ffc6845/lxml-5.1.0.tar.gz
tar -xf lxml-5.1.0.tar.gz
CC=gcc-11 python setup.py bdist_wheel --static-deps
cd dist
pip install ./lxml-5.1.0-cp39-cp39-linux_armv8l.whl
```

then install
```
pip install jupyter_contrib_nbextensions-0.7.0
```

