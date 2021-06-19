---
title: Ubuntu 16.04/CUDA 10.0 安裝筆記
slug: ubuntu-16.04-cuda-10.0-安裝筆記
date: 2019-10-04
categories:
    - Python
    - TensorFlow
tags:
    - GPU
    - NVIDIA
    - TensorFlow
    - Ubuntu
---

# 流程

1. 安裝 NVIDIA 顯卡驅動與相關套件  
[https://www.tensorflow.org/install/gpu#ubuntu_1604_cuda_10](https://www.tensorflow.org/install/gpu#ubuntu_1604_cuda_10)

2. 重開機

3. 測試
    ```bash
    nvidia-smi
    nvcc -V
    ```

4. 編譯測試工具，並進行測試
    ```bash
    cd /usr/local/cuda-10.0/samples
    sudo make
    cd bin/x86_64/linux/release
    sudo ./deviceQuery
    sudo ./bandwidthTest
    ```

5. 安裝 tensorflow-gpu
    ```bash
    sudo pip3 install tensorflow-gpu
    ```

6. 測試 tensorflow 能否抓到 GPU
    ```bash
    python3
    ```
    ```python
    from tensorflow.python.client import device_lib
    
    def get_available_gpus():
        local_device_protos = device_lib.list_local_devices()
        return [x.name for x in local_device_protos if x.device_type == 'GPU']
    
    get_available_gpus()
    ```

7. 若想防止未來誤更新，可使用 apt-mark 將套件 hold 住
    ```bash
    sudo apt-mark hold nvidia-418 cuda-10-0 libcudnn7 libcudnn7-dev
    ```

# 參考資料
- [GPU support  |  TensorFlow](https://www.tensorflow.org/install/gpu)
- [python – How to get current available GPUs in tensorflow? – Stack Overflow](https://stackoverflow.com/questions/38559755/how-to-get-current-available-gpus-in-tensorflow/38580201#38580201)
