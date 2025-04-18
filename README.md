# trt_pose_docker 

The repo. is built on [NVIDIA-AI-IOT/trt_pose](https://github.com/NVIDIA-AI-IOT/trt_pose).

<img src="https://user-images.githubusercontent.com/4212806/67125332-71a64580-f1a9-11e9-8ee1-e759a38de215.gif" height=256/>

GEFORCE RTX 2070</br>
fps: 30

## setup docker


- pull the docker

```
docker pull nvcr.io/nvidia/pytorch:20.11-py3
```

- start docker 
```
docker run \
--device=/dev/video0:/dev/video0 \
--gpus all \
--net host \
-e DISPLAY=$DISPLAY \
-v {your_folder}:/data \
--name trtppose \
-it nvcr.io/nvidia/pytorch:20.11-py3 \
/bin/bash
```
- Enable xhost (display docker screen)
```
xhost +
```
## start docker & install Dependencies
0. Start docker 

1. Install PyTorch and Torchvision
```
pip install torch torchvision torchaudio
```
2. Install torch2trt
```
cd /data
git clone https://github.com/NVIDIA-AI-IOT/torch2trt
cd ./torch2trt
python setup.py install --plugins
```

3. Install other miscellaneous packages
```
pip install tqdm cython pycocotools
apt-get install python3-matplotlib
```
4. fix error about `X Error: BadShmSeg (invalid shared segment parameter) 128`
```
export QT_X11_NO_MITSHM=1
```
## Install trt_pose
```
cd /data
git clone https://github.com/u0251077/trt_pose_docker.git
cd trt_pose_docker
python setup.py install
```
## Run the example
[NVIDIA-AI-IOT/trt_pose](https://github.com/NVIDIA-AI-IOT/trt_pose) provide a couple of human pose estimation models pre-trained on the MSCOCO dataset. The throughput in FPS is shown for each platform

| Model | Jetson Nano | Jetson Xavier | Weights |
|-------|-------------|---------------|---------|
| resnet18_baseline_att_224x224_A | 22 | 251 | [download (81MB)](https://drive.google.com/open?id=1XYDdCUdiF2xxx4rznmLb62SdOUZuoNbd) |
| densenet121_baseline_att_256x256_B | 12 | 101 | [download (84MB)](https://drive.google.com/open?id=13FkJkx7evQ1WwP54UmdiDXWyFMY1OxDU) |

1. Download the model weights using the link in the above table.

2. Place the downloaded weights in the `tasks/human_pose` directory

3. Strat example 
```
python cv_capture_demo.py
```
