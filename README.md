# Instructions for installing Theano on Windows for PSIML

### 1. Installing VS2013 and CUDA
Install [Visual Studio Community 2013](http://go.microsoft.com/fwlink/?LinkId=517284)
Install [CUDA 6.5](https://developer.nvidia.com/cuda-toolkit-65)

### 2. Installing Microsoft Visual C++ Compiler for Python 2.7
First install .Net 3.5 - on Windows 8.1 Programs and Features -> Turn Windows Features on or off
Install [Microsoft Visual C++ Compiler for Python 2.7](http://www.microsoft.com/en-us/download/details.aspx?id=44266)
First donwload Microsoft Visual C++ Compiler for Python 2.7 and then from Adminsitrator command prompt (e.g. Windows + X, Command prompt (Admin)) execute
msiexec /i <path to MSI> ALLUSERS=1



Now some hacks to avoid nvcc error


These are already existing instructions for installing Theano on Windows [Installation of Theano on Windows](http://www.deeplearning.net/software/theano/install_windows.html#install-windows).
