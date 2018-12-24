# Installing CUDA Toolkit 9.0 and cuDNN-7.0 for Torch (Lua)

Most of the content is reference from [Zhanwen Chen](https://medium.com/@zhanwenchen/install-cuda-and-cudnn-for-tensorflow-gpu-on-ubuntu-79306e4ac04e)

Download CUDA-9.0 Runfiles and cuDNN-7.0

## Installing CUDA-9.0

Provide the run file with authority and extract to desired foler
```
$ chmod +x cuda_9.0.176_384.81_linux-run
$ ./cuda_9.0.176_384.81_linux-run --extract=$HOME
```

At folder where files being extracted, you'll see three different files
```
cuda-linux.9.0.176-22781540.run          NVIDIA-Linux-x86_64-384.81.run
cuda-samples.9.0.176-22781540-linux.run
```
In which, ```NVIDIA-Linux-x86_64-384.81.run``` is the NVIDIA driver that can/should be ignore if you already have installed previously

Then, we will install CUDA Toolkit 9.0 by entering the following command
```
sudo ./cuda-linux.9.0.176-22781540.run 
```

Note that the cuda toolkit will be install at
```
Windows platform:

%ProgramFiles%\NVIDIA GPU Computing Toolkit\CUDA\v#.#

Linux platform:

/usr/local/cuda-#.#

Mac platform:

/Developer/NVIDIA/CUDA-#.#
```

After installation is completed, you'll need to make sure
```
PATH includes /usr/local/cuda-9.0/bin
LD_LIBRARY_PATH includes /usr/local/cuda-9.0/lib64, or, configure the runtime library.
```

Add /usr/local/cuda-9.0/bin to PATH

Firstly, manually add```Add /usr/local/cuda-9.0/bin``` to ```PATH``` variable using any editor

```
sudo gedit /etc/environment
```
add the path below to the end the PATH variable
```
:/usr/local/cuda-9.0/bin
```

* Configure the runtime library
```
$ sudo bash -c "echo /usr/local/cuda/lib64/ > /etc/ld.so.conf.d/cuda.conf"
$ sudo ldconfig
```

Now, reboot! We are almost done with CUDA Toolkit installation!

Next, we will need to test our installation of CUDA Toolkit by invoking our test.

Go back the folder, where all CUDA Toolkit runfiles located, and run the installation of sample by

```
sudo ./cuda-samples.9.0.176-22781540-linux.run
```

It will then install the file at directory ```/usr/local/cuda-9.0/samples```

Then,

```
$ cd /usr/local/cuda-9.0/samples
$ sudo make
```

After *successful* building the samples (do not worry about irrelevant warnings about deprecated architectures (`sm_20` and such ancient GPUs) )

Run the following test

```
$ cd /usr/local/cuda/samples/bin/x86_64/linux/release
$ ./deviceQuery
```

The result should be something looks like this

```
./deviceQuery Starting...
 CUDA Device Query (Runtime API) version (CUDART static linking)
Detected 1 CUDA Capable device(s)
Device 0: "GeForce GTX 1060"
  CUDA Driver Version / Runtime Version          9.0 / 9.0
  CUDA Capability Major/Minor version number:    6.1
  Total amount of global memory:                 6073 MBytes (6367739904 bytes)
  (10) Multiprocessors, (128) CUDA Cores/MP:     1280 CUDA Cores
  GPU Max Clock rate:                            1671 MHz (1.67 GHz)
  Memory Clock rate:                             4004 Mhz
  Memory Bus Width:                              192-bit
  L2 Cache Size:                                 1572864 bytes
  Maximum Texture Dimension Size (x,y,z)         1D=(131072), 2D=(131072, 65536), 3D=(16384, 16384, 16384)
  Maximum Layered 1D Texture Size, (num) layers  1D=(32768), 2048 layers
  Maximum Layered 2D Texture Size, (num) layers  2D=(32768, 32768), 2048 layers
  Total amount of constant memory:               65536 bytes
  Total amount of shared memory per block:       49152 bytes
  Total number of registers available per block: 65536
  Warp size:                                     32
  Maximum number of threads per multiprocessor:  2048
  Maximum number of threads per block:           1024
  Max dimension size of a thread block (x,y,z): (1024, 1024, 64)
  Max dimension size of a grid size    (x,y,z): (2147483647, 65535, 65535)
  Maximum memory pitch:                          2147483647 bytes
  Texture alignment:                             512 bytes
  Concurrent copy and kernel execution:          Yes with 2 copy engine(s)
  Run time limit on kernels:                     Yes
  Integrated GPU sharing Host Memory:            No
  Support host page-locked memory mapping:       Yes
  Alignment requirement for Surfaces:            Yes
  Device has ECC support:                        Disabled
  Device supports Unified Addressing (UVA):      Yes
  Supports Cooperative Kernel Launch:            Yes
  Supports MultiDevice Co-op Kernel Launch:      Yes
  Device PCI Domain ID / Bus ID / location ID:   0 / 1 / 0
  Compute Mode:
     < Default (multiple host threads can use ::cudaSetDevice() with device simultaneously) >
deviceQuery, CUDA Driver = CUDART, CUDA Driver Version = 9.0, CUDA Runtime Version = 9.0, NumDevs = 1
Result = PASS
```

Man~I feel so proud of you! 

Let's proceed to install cuDNN 7.0!

## Install cuDNN 7.0

[Copy and paste from the reference source]

The recommended way to install cuDNN 7.0 is to download all 3 .deb files. I had previously recommended using the .tgz installation approach, but found out that it didn’t allow verification by running code samples (no way to install the code samples .deb after .tgz installation).

The following steps are pretty much the same as the [installation guide](http://developer.download.nvidia.com/compute/machine-learning/cudnn/secure/v7.0.5/prod/Doc/cuDNN-Installation-Guide.pdf?MkhwIqyyLy9kGXxFq3sH3U09Fc7IIOhABICkOgXkQDchRcfJMI5F5sofrxStnIgGnEVThgKmHA8kYTAcHbHims2ytyFLanRlXyzXZpPYqfd8c7WFRu3uVLSiXUMSTp_EySt_YRsrS415WSIPIBEMsDrRqXzrBJXgTUmRr0K1ybOKh915WBlXlu1YZpbq83KzmA)using .deb files (strange that the cuDNN guide is better than the CUDA one).

1. Go to the [cuDNN download page](https://developer.nvidia.com/rdp/cudnn-download) (need registration) and select the latest cuDNN 7.0.* version made for CUDA 9.0.
2. Download all 3 .deb files: the runtime library, the developer library, and the code samples library for Ubuntu 16.04.
3. In your download folder, install them in the same order:

`$ sudo dpkg -i libcudnn7_7.0.5.15–1+cuda9.0_amd64.deb` (the runtime library),

`$ sudo dpkg -i libcudnn7-dev_7.0.5.15–1+cuda9.0_amd64.deb` (the developer library), and

`$ sudo dpkg -i libcudnn7-doc_7.0.5.15–1+cuda9.0_amd64.deb` (the code samples).

Now we can verify the cuDNN installation (below is just the official guide, which surprisingly works out of the box):

1. Copy the code samples somewhere you have write access: `cp -r /usr/src/cudnn_samples_v7/ ~`.
2. Go to the MNIST example code: `cd ~/cudnn_samples_v7/mnistCUDNN`.
3. Compile the MNIST example: `make clean && make`.
4. Run the MNIST example:` ./mnistCUDNN`. If your installation is successful, you should see `Test passed!` at the end of the output.

### Configure the CUDA and cuDNN library paths

What you do need to do, however, is exporting environment variables LD_LIBRARY_PATH in your .bashrc file:

```
# put the following line in the end or your .bashrc file
export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/local/cuda/extras/CUPTI/lib64"
```

And source it by `source ~/.bashrc`.