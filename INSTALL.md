## Installation

### Requirements:
- PyTorch >= 1.0. Installation instructions can be found in https://pytorch.org/get-started/locally/.
- torchvision==0.2.1
- cocoapi
- yacs
- matplotlib
- GCC >= 4.9,< 6.0
- (optional) OpenCV for the webcam demo

### pytorch torchvision CUDA 和python的版本要匹配！！
[参考文献](https://blog.csdn.net/qq_40263477/article/details/106577790)
### 同时pytorch 和 torchvision的版本和CUDA 版本可以稍微浮动！
### 这里的 pytorch=1.0.1 torchvision=0.2.2 cudatoolkit=9.0，这个ATSS可以运行。这里的python=3.6。

这里最好能够查看一下cuda的版本：

​ 1.cat /usr/local/cuda/version.txt
​ 2.或者 nvcc -v
<br>
建议参考[文献](https://www.cnblogs.com/alanchens/p/13950902.html)
<br>
### Option 1: Step-by-step installation



```bash
# first, make sure that your conda is setup properly with the right environment
# for that, check that `which conda`, `which pip` and `which python` points to the
# right path. From a clean conda env, this is what you need to do

conda create --name ATSS
conda activate ATSS

# this installs the right pip and dependencies for the fresh python
conda install ipython

# ATSS and coco api dependencies
pip install ninja yacs cython matplotlib tqdm

# follow PyTorch installation in https://pytorch.org/get-started/locally/
# we give the instructions for CUDA 9.0
conda install -c pytorch torchvision=0.2.1 cudatoolkit=9.0
# 这里一定要下【注意】pytorch torchvision 和 cudatookit 的版本和关系
 
export INSTALL_DIR=$PWD

# install pycocotools. Please make sure you have installed cython.
cd $INSTALL_DIR
git clone https://github.com/cocodataset/cocoapi.git
cd cocoapi/PythonAPI
python setup.py build_ext install

# install PyTorch Detection
cd $INSTALL_DIR
git clone https://github.com/sfzhang15/ATSS.git
cd ATSS

# the following will install the lib with
# symbolic links, so that you can modify
# the files if you want and won't need to
# re-build it
python setup.py build develop --no-deps


unset INSTALL_DIR

# or if you are on macOS
# MACOSX_DEPLOYMENT_TARGET=10.9 CC=clang CXX=clang++ python setup.py build develop
```

### Option 2: Docker Image (Requires CUDA, Linux only)
*The following steps are for original maskrcnn-benchmark. Please change the repository name if needed.* 

Build image with defaults (`CUDA=9.0`, `CUDNN=7`, `FORCE_CUDA=1`):

    nvidia-docker build -t maskrcnn-benchmark docker/
    
Build image with other CUDA and CUDNN versions:

    nvidia-docker build -t maskrcnn-benchmark --build-arg CUDA=9.2 --build-arg CUDNN=7 docker/
    
Build image with FORCE_CUDA disabled:

    nvidia-docker build -t maskrcnn-benchmark --build-arg FORCE_CUDA=0 docker/
    
Build and run image with built-in jupyter notebook(note that the password is used to log in jupyter notebook):

    nvidia-docker build -t maskrcnn-benchmark-jupyter docker/docker-jupyter/
    nvidia-docker run -td -p 8888:8888 -e PASSWORD=<password> -v <host-dir>:<container-dir> maskrcnn-benchmark-jupyter
