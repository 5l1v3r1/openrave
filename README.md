OpenRAVE-py3
------------
* This branch allows the user to build OpenRAVE, the Open Robotics Automation Virtual Environment, and its Python3 bindings on macOS and Linux.

* Specify the installation path of `OpenRAVE` by `OPENRAVE_INSTALL_DIR`. 
 
One can assign this macro during the initialization of terminal, for example, by executing
```
echo 'export OPENRAVE_INSTALL_DIR='$HOME'/Projects/openrave' >> ~/.bashrc
```
when using `bash`, or
```
echo 'export OPENRAVE_INSTALL_DIR='$HOME'/Projects/openrave' >> ~/.zprofile
```
when using `zsh` (e.g. in macOS Catalina).

* The default cmake arguments are
```
-DODE_USE_MULTITHREAD:BOOL=ON -DOPT_IKFAST_FLOAT32:BOOL=OFF -DNATIVE_COMPILE_FLAGS:STRING="-march=native -mtune=native" -DOPT_LOG4CXX:BOOL=ON -DUSE_PYBIND11_PYTHON3_BINDINGS:BOOL=ON -DCMAKE_INSTALL_PREFIX=$OPENRAVE_INSTALL_DIR
```

* Install `pybind11`: https://github.com/pybind/pybind11 with cmake arguments
```
-DPYBIND11_TEST=0 -DCMAKE_INSTALL_PREFIX=$OPENRAVE_INSTALL_DIR
```

Install Python3 and Boost on macOS
----------------------------------
* Install Homebrew.
```
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
See <https://brew.sh/> for more details.

* Install third-party dependencies with Homebrew.
```
$ brew install python3 libxml2 boost boost-python3 qt open-scene-graph
```

Homebrew installs these packages under `/usr/local/Cellar`. Normally, the header files are linked to `/usr/local/include`, and the libraries are linked to `/usr/local/lib`. Sometimes Homebrew installs a package as "keg-only", that is without linking. Then the user needs to do `brew link` manually; see https://docs.brew.sh/FAQ#what-does-keg-only-mean.

A more preferrable way to install Python3 on macOS is using its 64-bit installer (`python-3.x.y-macosx10.9.pkg`), which one can download from https://www.python.org/downloads/mac-osx/. It installs Python3 under 
```
/Library/Frameworks/Python.framework/Versions/3.x
```

Install Python3 and Boost on Linux
----------------------------------
* Specify the installation path of `OpenRAVE` by a macro `OPENRAVE_INSTALL_DIR`, similar to in macOS above.

* Install Python3.x and Boost 1.7x

We show how to build Python 3.8 and Boost 1.71 from source files. Other similar versions can be installed analogously.

```
# Install Python 3.8
wget https://www.python.org/ftp/python/3.8.0/Python-3.8.0.tgz
tar xvfz Python-3.8.0.tgz
cd Python-3.8.0
./configure --prefix=$OPENRAVE_INSTALL_DIR --enable-shared
make && make install
cd $OPENRAVE_INSTALL_DIR/bin
./pip3 install numpy ipython
```

```
# Install Boost 1.71 
wget https://dl.bintray.com/boostorg/release/1.71.0/source/boost_1_71_0.tar.gz
tar xvfz boost_1_71_0.tar.gz
cd boost_1_71_0
bash bootstrap.sh --prefix=$OPENRAVE_INSTALL_DIR --with-python=$OPENRAVE_INSTALL_DIR/bin/python3
./b2 --prefix=$OPENRAVE_INSTALL_DIR install
```

Install OpenRAVE-py3
--------------------
- Add the subdirectory `lib` of `OPENRAVE_INSTALL_DIR` into the library search paths by
```
echo 'export LD_LIBRARY_PATH='$OPENRAVE_INSTALL_DIR/lib':$LD_LIBRARY_PATH' >> ~/.bashrc
echo 'export DYLD_LIBRARY_PATH='$OPENRAVE_INSTALL_DIR/lib':$DYLD_LIBRARY_PATH' >> ~/.bashrc
```

- Clone this repository
```
git clone git@github.com:rdiankov/openrave.git
git checkout macOS
```

In `CMakeLists.txt`, edit accordingly Python3's version, Boost's version, and installation paths and version numbers of other third-party dependencies. (To be written ...)

- Build
```
cd openrave
mkdir build
cd build
cmake -DODE_USE_MULTITHREAD:BOOL=ON -DOPT_IKFAST_FLOAT32:BOOL=OFF -DNATIVE_COMPILE_FLAGS:STRING="-march=native -mtune=native" -DOPT_LOG4CXX:BOOL=ON -DCMAKE_INSTALL_PREFIX=$OPENRAVE_INSTALL_DIR ..
make -j4
make install
```

License
-------
* LGPL <https://www.gnu.org/licenses/license-list.html#LGPL>
