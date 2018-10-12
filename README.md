# Install Mitsuba Renderer on Mac OS High Sierra
This document provides my working process of installing the latest Mitsuba renderer 0.6.0 on MacOS using Anaconda.

Note: Mac does not support OpenMP thus some function has to be run in single thread.

The latest Mitsuba version is 0.6.0 on GitHub, which has not been put on the official website.

# OS

MacOS High Sierra (V10.13.6)

# Preparation

A clean Python 2.7 environment (I use Anaconda to create), and make sure that the default python is 2.7 instead of 3.5/3.6, since Mitsuba configuration file is written in Python2.
```
$conda create -n mitsuba_py27 python=2.7
$source activate mitsuba_py27
```

Scons: > 2.0.0 (I use V3.0.1) 
```
$pip install SCons
```
or 
```
conda install -c anaconda scons
```

XCode (V10.0) and Xcode Command Line Tools (V10.13)

To install command line tools, open XCode: File -> Open Develop Tool -> More develop tools and install the correct version.


# Installation and Compilation

This is a little bit tricky here. But the following steps work for me.

1. Clone Mitsuba 0.6.0 Repo:
```
$git clone --recursive https://github.com/mitsuba-renderer/mitsuba.git
```
2. In the root directory of /mitsuba folder, clone Mitsuba dependencies V0.6.0 for Mac OS:
```
$git clone https://github.com/mitsuba-renderer/dependencies_macos.git
```
Note: Do NOT use the link provided in official Mitsuba manual for dependencies, which is:
```
$hg clone https://www.mitsuba-renderer.org/hg/dependencies_macos
```
Since this will download dependencies V0.5.0 which will encounter an incompatible error with Mitsuba V0.6.0.

The cloned dependencies should be in the /mitsuba root directory.

3. Change the name of independency folders
```
$mv dependencies_macos dependencies
```

4. Copy /mitsuba/build/config-macos10.12-clang-x86_64.py configuration file into the root /mitsuba directory:
```
$cp ./build/config-macos10.12-clang-x86_64.py config.py
```


5. If your render XML is written in Python 2.7 and use the new XCode command line tools, please open config.py and change from:
```
PYTHON27INCLUDE= ['/System/Library/Frameworks/Python.framework/Versions/2.7/Headers']
```
to
```
PYTHON27INCLUDE= ['/System/Library/Frameworks/Python.framework/Versions/2.7/include/python2.7/']
```
This is because the latest XCode command line tools change the location of header files.

6. Open config.py and change the Mac OS SDK path from:
```
'/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.12.sdk'
```
to
```
'/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk'
```
Note that there are two changes of this path should be made in CCFLAGS and LINKFLAGS.


7. Run Scons to compile Mitsuba:
```
$scons
```
It will take several minutes to finish.

8. To run the renderer from the command line:
```
source setpath.sh
```

Now you will see Mitsuba application icon in /mitsuba root directory. Now start rendering!

If you have any questions, please email:
```
shz338@eng.ucsd.edu
```

Shilin Zhu



