## PyCharm with Docker Tutorial

Docker is a great tool for deep learning research. PyCharm is a wonderful Python IDE. This tutorial teaches you how to use PyCharm with Docker for deep learning research on Ubuntu.

#### Steps:
                
1. install [docker-ce](https://docs.docker.com/install/linux/docker-ce/ubuntu/).
2. add `$USER` to the docker group (may need reboot):
`sudo groupadd docker`
`sudo usermod -aG docker $USER`
3. install PyCharm professional:
`sudo apt-get install snap`
`sudo snap install pycharm-professional --classic`
4. install [nvidia-docker (v2)](https://github.com/NVIDIA/nvidia-docker).
5. install docker compose (suggest use python3):
`sudo apt-get install python3 python3-pip`
`pip3 install docker-compose pyyaml`
6. assume you have built a docker image, this tutorial takes the [RTFNet](https://github.com/yuxiangsun/RTFNet) docker image as the example.
7. construct a `docker-compose.yml` file like this:
```yaml
version: "2.3"
services:
  RTFNet_GPU_0: 
    runtime: nvidia
    image: docker_image_rtfnet
    volumes:
      - /home/yourname/datasets/:/opt/datasets
    shm_size: 32g
    devices: 
      - /dev/nvidia0
      - /dev/nvidia1   

  RTFNet_GPU_1: 
    runtime: nvidia
    image: docker_image_rtfnet
    volumes:
      - /home/yourname/datasets/:/opt/datasets
    shm_size: 32g
    devices: 
      - /dev/nvidia0
      - /dev/nvidia1 
```
Note: If you have just one GPU, please delete the block of `RTFNet_GPU_1` and `- /dev/nvidia1` in the device description. If you have more than two GPUs, please add blocks of `RTFNet_GPU_2`, `RTFNet_GPU_3`, etc, and modify the `devices` accordingly. The `volumes` is used to map the folder in your host machine to a folder in your docker. Please modify it according to your need. Here, we use it to map the dataset folder, which is placed in `/home/yourname/datasets/` of the host machine.
8. lauch PyCharm, File->Open, open RTFNet as a project.
9. in PyCharm, file->settings->intepreter, load the `docker-compose.yml` in PyCharm.
