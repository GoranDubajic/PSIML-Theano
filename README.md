# Instructions for installing Theano on Windows for PSIML

There are already official instructions [Installation of Theano on Windows](http://www.deeplearning.net/software/theano/install_windows.html#install-windows) however please follow instructions bellow instead of official one.

We suppose that installation directory is `C:\SciSoft` and we used **x64** platform.

### 1. Installing VS2013 and CUDA
- Install [Visual Studio Community 2013](http://go.microsoft.com/fwlink/?LinkId=517284)
- Install [CUDA 6.5](https://developer.nvidia.com/cuda-toolkit-65)

### 2. Installing Microsoft Visual C++ Compiler for Python 2.7
- Install .Net 3.5 - on Windows 8.1 *Programs and Features -> Turn Windows Features on or off*
- Install [Microsoft Visual C++ Compiler for Python 2.7](http://www.microsoft.com/en-us/download/details.aspx?id=44266)
    1. Donwload Microsoft Visual C++ Compiler for Python 2.7
    2. From Administrator command prompt (e.g. *Windows + X, Command prompt (Admin)*) execute
```
msiexec /i <path to MSI> ALLUSERS=1
```

### 3. Installing GCC
- Install [TDM GCC](http://tdm-gcc.tdragon.net/download) to `C:\SciSoft\TDM-GCC-64`

### 4. Scientific Python distribution
- Install [WinPython](https://winpython.github.io/) to `C:\SciSoft\WinPython-64bit-2.7.10.1`. Please install version **2.7.10.1** as version *2.7.9.4* (which was used in offical Theano instructions) did not work for us.

### 5. Configuring the Environment
At this point, you should have installed all Theano dependencies. By default neither Python, GCC, nor Visual Studio was added to the PATH. Save the following shell script as `C:\SciSoft\env.bat` to configure the system path
```cmd
REM configuration of paths
set VSFORPYTHON="C:\Program Files (x86)\Common Files\Microsoft\Visual C++ for Python\9.0"
set SCISOFT=C:\SciSoft

REM add tdm gcc stuff
set PATH=%SCISOFT%\TDM-GCC-64\bin;%SCISOFT%\TDM-GCC-64\x86_64-w64-mingw32\bin;%PATH%

REM add winpython stuff
CALL %SCISOFT%\WinPython-64bit-2.7.10.1\scripts\env.bat

REM configure path for msvc compilers
REM for a 32 bit installation change this line to
REM CALL %VSFORPYTHON%\vcvarsall.bat
CALL %VSFORPYTHON%\vcvarsall.bat amd64

REM return a shell
cmd.exe /k
```

You can access the Python shell by double-clicking on `C:\SciSoft\env.bat`. Please do so, and verify that the following programs are found:
  1. `where gcc`
  2. `where gendef`
  3. `where cl`
  4. `where nvcc`

Finally we need to create a link library for GCC. Open up the Python shell by double-clicking on `C:\SciSoft\env.bat`. Then execute:
```
gendef WinPython-64bit-2.7.10.1\python-2.7.9.amd64\python27.dll
dlltool --dllname python27.dll --def python27.def --output-lib WinPython-64bit-2.7.10.1\python-2.7.10.amd64\libs\libpython27.a
```

### 6. Installing Theano
- Install [MSYSGIT](http://msysgit.github.io/) with component *Windows Explorer integration -> Git Bash Here*
- Open up the Git Shell in the directory (from file explorer right click in the directory and choose *Git Bash Here*) in which you want to install Theano and execute
```
git clone https://github.com/Theano/Theano.git --branch rel-0.7
```

### 7. Now some hacks to avoid *nvcc* error
From [How to set up Theano on Windows 8.1 64-bit](http://machinelearning.berlin/?p=383)
- Created Theano configuration file `C:\SciSoft\WinPython-64bit-2.7.10.1\settings\.theanorc` containing
```
[global]
device = cpu
floatX = float32
 
[nvcc]
compiler_bindir = C:\Program Files (x86)\Microsoft Visual Studio 12.0\VC\bin\amd64
LC=C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v6.5\lib\x64
```

- At the end you need to edit `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v6.5\bin\nvcc.profile` by setting this line 
```
INCLUDES        +=  "-I$(TOP)/include" "-I$(TOP)/include/cudart" "-IC:\Program Files (x86)\Microsoft Visual Studio 12.0\VC\include" $(_SPACE_)
```
