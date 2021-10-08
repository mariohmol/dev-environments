
## Mac

Follow the [Mac Environment Docs](../dev/MacEnvironment.md) to install `Xcode`, `Apple Command Line Tools` and `Homebrew`.

Then install Make Compiler:

```
brew install cmake
```


## Open CV

You might need some utils from OpenCV as `opencv_createsamples`, `opencv_traincascade` and others.

To install that, first download the [OpenCV sources](https://sourceforge.net/projects/opencvlibrary/files/) and extract that into a folder.


To compile the sources you need to install cmake: `brew install cmake`

After that you should run the following commands


```sh
mkdir build
cd build
cmake -D WITH_TBB=OFF -D BUILD_NEW_PYTHON_SUPPORT=OFF -D BUILD_FAT_JAVA_LIB=OFF -D BUILD_TBB=OFF -D BUILD_EXAMPLES=ON -D CMAKE_CXX_COMPILER=g++ CMAKE_CC_COMPILER=gcc -D CMAKE_OSX_ARCHITECTURES=x86_64 -D BUILD_opencv_java=OFF -G "Unix Makefiles" ..
make -j8
sudo make install
```

You can access all the modules inside the bin folder of this directory