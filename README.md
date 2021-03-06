# pose-estimation-project

1. To run in aws - Create an Ubuntu EC2 instance
You can use the following machine Ubuntu Server 16.04 LTS (HVM), SSD Volume Type - ami-0f2ed58082cb08a4d (64-bit x86)
Make sure the volume available to the EC2 instance is greater 150GB.

2. Run the following commands to install Docker - taken from [docker installation on ubuntu guide](https://docs.docker.com/engine/install/ubuntu/)
```sh
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) \
  stable"
sudo apt-get update
sudo apt-get install docker-ce docker-compose
```

3. check Client docker engine version >= 19.03 using the following command:
`docker version`

4. On Ubuntu 18.04 or 16.04 machines with Nvidia GPUs, run the following commands - taken from [nvidia-docker](https://github.com/NVIDIA/nvidia-docker])
```sh
# Add the package repositories
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt-get update && sudo apt-get install -y nvidia-container-toolkit nvidia-cuda-toolkit
sudo systemctl restart docker
```

5. Make sure that the following command is running:
`nvidia-smi`
If not, try to reboot the machine using the reboot command:
`reboot`

6. pull the docker files code:
`git clone https://github.com/atalyaalon/pose-estimation-project.git`

7. Recommended: run the following command in tmux - run in shell:
`tmux`

8. Build the docker file:
```sh
cd pose-estimation-project/openpifpaf/
sudo docker-compose build
```

9. Run the docker file:
`sudo docker run --gpus all --shm-size=100gb --name openpifpaf bestteam/openpifpaf:latest`

Note: we did not use docker-compose in this stage since docker compose does not suppor NVIDIA GPUs yet - see https://github.com/docker/compose/issues/6691

10. To find the trained models or logs in the docker - start the openpifpaf container, and check the outputs directory:
```sh
sudo docker start openpifpaf
sudo docker exec -it openpifpaf bash
```
and run inside docker:
`ls outputs`

(To exit the container use ctrl-D)

11. To stop the openpifpaf container run:
`sudo docker stop openpifpaf`
